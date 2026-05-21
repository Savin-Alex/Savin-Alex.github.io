---
title: Local PII Anonymizer
---

# Local PII Anonymizer

[RU](../../projects/local-pii-anonymizer.md) · [EN](./local-pii-anonymizer.md)

Local-first offline service for reversible PII/PHI pseudonymization in RU/EN documents, designed for 152-FZ, GDPR, and HIPAA Safe Harbor contexts — so legal, HR, and clinical teams can safely prepare sensitive documents for downstream LLM/workflow use while keeping reversible mappings under operator control.

## Project Passport

| Field | Content |
|---|---|
| **Problem** | Off-the-shelf anonymization tools are either cloud-based (a leak in itself) or one-pass regex filters that break on mixed RU/EN documents and don't validate checksums. Not enough for regulated workflows. |
| **Goal** | A local-first PII/PHI pseudonymization tool that lets users safely prepare sensitive RU/EN documents for LLM/workflow use while keeping reversible mappings under customer/operator control. |
| **In scope** | Cascade detection (regex+checksum → Presidio/spaCy → Medical NER → LLM fallback); 17+ entity types; reversible pseudonymization + HIPAA Safe Harbor redaction; encrypted vault (AES-256-GCM) + tenant isolation; FastAPI/Streamlit/CLI/Docker; eval harness + CI gate; audit trail. |
| **Out of scope** | Certified production compliance; validation on real customer documents; full retention/deletion governance; production deploy (TLS / secret store / append-only audit); Windows validation. |

**Status: Amber** — strong technical progress; not yet approved for production PII without final security, privacy, legal, and deployment controls.

**Success criteria (with status):**
- Clear terminology (pseudonymization ≠ anonymization) → **met**
- Encrypted, tenant-isolated vault → **met** (AES-256-GCM, tenant-scoped restore/delete)
- Measured detector quality → **met on synthetic data** (macro F1 ≈ 0.9997; 15/17 types = 1.0) — not on real documents
- Auditable operations → **met** (audit events, metrics)
- A documented path from pilot to compliant production → **documented, not completed**

## My Role

**Role:** Junior IT PM · solo project (aiming for production); problem framing, architecture, development, and quality on me.

- Framed the problem for regulated scenarios: legal, HR, healthcare.
- Defined compliance-aware modes: reversible pseudonymization and HIPAA Safe Harbor redaction.
- Designed quality gates: tests, eval harness, CI hard-fail, audit trail, and PII-safe logging.
- Built a technical MVP that can be discussed as an AI governance / data-risk case, not only a backend service.

## Solution

A cascade detection architecture with explicit compliance modes and a reversible pseudonym vault.

- Cascade: regex with checksum → Presidio (spaCy NER) → optional Medical NER (HuggingFace) → optional LLM fallback (Ollama, via circuit breaker).
- 17+ entity types with checksum validation (RU passport, INN/SNILS/OGRN, US SSN, UK NINO, SWIFT/BIC, etc.).
- Reversible pseudonymization via pluggable Vault: InMemory / SQLite / Redis, sharded lock pool.
- HIPAA Safe Harbor mode — irreversible redaction of dates (year-only) and ages over 89 (`90+`), no vault write.
- Audit trail in structlog: 152-FZ / GDPR Art. 30 / HIPAA §164.312.

## Architecture

Clean Architecture with clear layer boundaries and a port-adapter pattern.

- `domain/` — entities, value objects, mode policies.
- `application/` — use-cases, cascade orchestration, decorator chain (FilteringEngine).
- `infrastructure/` — Presidio/spaCy/HF adapters, Vault repositories (Protocol ports + ABC), HTTP/CLI/UI.
- `tests/` — unit, integration, property-based (Hypothesis), eval harness.

## Metrics Snapshot

| Area | Signal |
|---|---|
| Tests collected | 367 (43 test files) |
| ADRs | 15 |
| Eval corpus | 4 curated golden + 200 generated synthetic docs |
| Eval quality | macro F1 ≈ 0.9997 (presidio_multi); 15/17 entity types at F1 = 1.0 |
| Entry points | FastAPI, Streamlit UI, CLI, Docker Compose |
| Vault backends | Memory, SQLite, Redis |
| Document inputs | txt, md, csv, json, pdf, docx |
| CI checks | mypy strict, ruff, bandit, pip-audit, eval hard-fail gate |

Numbers are on a synthetic corpus, used as an internal eval gate — not a production guarantee.

## Risk Register

| Risk | Level | Owner | Response |
|---|---|---|---|
| Compliance scope not formally approved (GDPR/HIPAA/152-FZ…) | High | Legal / Compliance | Confirm applicable regimes and produce a review packet before real PII launch. |
| Detector recall unproven on representative documents | High | Product / ML | Add anonymized customer-like golden data and set launch gates. |
| Vault retention, deletion, backup, key rotation incomplete | High | Security / Platform | Define retention policy, deletion SLA, backup erasure, rotation migration. |
| Production deployment controls not finalized | Medium | Platform | Lock dependencies, enforce security scans, define TLS/secret/audit/metrics profile. |
| Windows support unvalidated | Medium | Engineering | Add Windows CI and Windows 11 smoke-test evidence. |

## Reliability & Security

- Encrypted vault at rest (AES-256-GCM), row AAD binding, fail-closed encryption in the CLI.
- Tenant isolation: scoped restore/delete, tenant-based rate limiting, tenant in audit trail.
- Non-root / read-only containers, secrets via compose secret files.
- Lazy model loading + FastAPI lifespan (no cold start on first request); circuit breaker for LLM fallback.
- structlog with a PII redactor — even logs must not leak.

## Status & Next Steps

Full one-pager: [Status report (sample)](/evidence/pii-status-report.html).

**Done:** security hardening (encrypted vault, AES-GCM AAD, fail-closed CLI, tenant-scoped restore/delete, non-root containers); terminology corrected (pseudonymization, not anonymization; explicit Safe Harbor caveats); evidence base (15 ADRs, C4/data-flow, regulatory mapping, runbook, eval summary).

**Decisions needed:** product scope (pilot / beta / production?); compliance regimes in launch scope (GDPR / CCPA / HIPAA / 152-FZ / SOC 2?); deployment target; data policy (retention, deletion SLA, key custody); evaluation standard (who owns representative golden data, acceptable F1/recall gates).

**Next steps:** finish extractor (no-text PDF) + full unit/eval/security run; Windows CI + smoke test; production deployment profile (TLS, secret store, encrypted storage, audit sink, dependency lock); privacy review packet (data inventory, DPIA, retention/deletion policy); pilot go/no-go review (risks, evidence, rollback).

## Retro

- **Went well:** security and privacy posture moved from prototype to reviewable architecture.
- **To improve:** evidence must shift from synthetic confidence to representative customer-like validation.
- **Keep doing:** strict terminology, explicit risks, and audit-backed decisions before expanding scope.

## Stack

Python 3.12, FastAPI, Streamlit, Pydantic, structlog, Docker, Presidio, spaCy, HuggingFace Transformers, Ollama, Hypothesis, pytest, mypy, ruff, bandit.
