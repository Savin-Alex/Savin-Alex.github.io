---
title: "Local PII Anonymizer — Status One-Pager (sample)"
---

# Local PII Anonymizer — Portfolio Status One-Pager

[← back to the Local PII Anonymizer project](/projects/local-pii-anonymizer.html)

_Sample PM artifact: an executive one-pager for a personal project in development._

**Date:** 2026-05-21
**Overall status:** Amber — strong technical progress; not yet approved for production PII without final security, privacy, legal, and deployment controls.

## Charter
Deliver a local-first PII/PHI pseudonymization tool that lets users safely prepare sensitive RU/EN documents for downstream LLM/workflow use while keeping reversible mappings under customer/operator control. Success means: clear privacy terminology, encrypted and tenant-isolated vault behavior, measured detector quality, auditable operations, and a documented path from internal pilot to compliant production deployment.

## Metrics
| Area | Current signal |
|---|---|
| Tests collected | 367 (43 test files) |
| ADRs | 15 |
| Eval corpus | 4 curated golden + 200 generated synthetic docs |
| Eval quality | macro F1 ≈ 0.9997 on `presidio_multi`; 15/17 entity types at F1 = 1.0 |
| Entry points | FastAPI, Streamlit UI, CLI, Docker Compose |
| Vault backends | Memory, SQLite, Redis (AES-256-GCM at rest) |

## Top risks
- Production compliance not certified: GDPR/CCPA/HIPAA/152-FZ applicability needs qualified legal review.
- Detection quality not proven on representative customer documents; current metrics are largely synthetic.
- Retention/deletion governance incomplete: no vault TTL, backup erasure, or key-rotation workflow yet.
- Deployment readiness incomplete: dependency pinning, append-only audit sink, TLS profile, secret management.
- Windows support planned but not validated (no Windows CI / smoke-test evidence).

## Decisions needed
- Product scope: internal pilot, customer beta, or production launch?
- Compliance scope: which regimes in launch scope (GDPR / CCPA / HIPAA / 152-FZ / SOC 2)?
- Deployment target: local desktop, Docker Desktop, managed K8s/ECS, or on-prem?
- Data policy: vault retention period, deletion SLA, backup erasure, key custody.
- Evaluation standard: who owns representative golden data; acceptable recall/F1 gates.

## Next steps
1. Finish extractor WIP; full unit/eval/security run; update docs with final extractor guarantees.
2. Add Windows CI + Windows 11 smoke-test evidence (API, UI, CLI, SQLite vaults, Cyrillic/long paths).
3. Production deployment profile: TLS ingress, secret store, encrypted storage, audit-log sink, dependency lock.
4. Privacy review packet: data inventory, DPIA-style risk assessment, retention/deletion policy, legal questions.
5. Pilot go/no-go review: constraints, residual risks, test evidence, rollback plan, support boundaries.

## Retro
- **Went well:** security and privacy posture moved from prototype to reviewable architecture.
- **To improve:** evidence must shift from synthetic confidence to representative customer-like validation.
- **Keep doing:** strict terminology, explicit risks, and audit-backed decisions before expanding scope.
