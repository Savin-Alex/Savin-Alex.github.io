---
title: RemovePII — personal privacy gateway
---

[RU](../../projects/removepii.html) · [EN](./removepii.html)

A web application for individuals: paste text or upload a document, review what was found, and get a protected version to share with ChatGPT, Claude, Gemini, or any other service. When the answer comes back, RemovePII restores the original values and then erases the mapping.

The product rule is **never call reversible masking "anonymization"**. Anyone who holds both the mapping and the key can restore the text, so the product says exactly that and keeps the mapping short-lived.

<p><b>Service:</b> <a href="https://removepii.app" target="_blank" rel="noopener">RemovePII.app</a></p>

## Project passport

| Field | Content |
|---|---|
| **Problem** | People send contracts, medical letters, and API keys to AI tools. Browser-only filters cannot handle documents, and server tools usually keep the text. |
| **Goal** | Let one person protect text and documents before sharing, with no stored source text and a mapping that expires or can be erased at once. |
| **In scope** | RU/EN web app and personal API; review of detections before sharing; same-format protection of DOCX, text PDF, CSV, and JSON; restore and erase; 18 credential types; per-account data isolation; retention worker. |
| **Out of scope** | Teams and shared accounts; image and scanned-PDF inspection; a claim that personal use makes anyone legally compliant; public launch before the release gates pass. |

## My role

**Role:** IT Project Manager · solo product; specification, architecture, development, and security review.

- Wrote the product specification and its privacy invariants before any code: one person per account, no stored source text, review before disclosure, no hidden data egress, fail closed.
- Reused the tested engine from [Local PII Anonymizer](./local-pii-anonymizer.html) with a traceable link to the exact source commit, instead of rewriting security-sensitive code.
- Ran a full repository audit, then did the fixes test-first: every confirmed finding got a failing regression test before the fix.

## Solution design

1. **Web app + personal API** over one security boundary. Every resource belongs to one server-derived account; one account can never read another account's documents, mappings, or tokens.
2. **Detection engine**: checksum-validated regex, Presidio/spaCy NER for RU and EN, and a credential pack (OpenAI, Anthropic, AWS, Stripe, GitHub keys, private keys, database connection strings, and more).
3. **Review screen**: the person sees every detection and every gap before protected text leaves the service.
4. **Same-format output**: a protected DOCX comes back as DOCX, and a CSV as CSV. If a file contains images, the download requires an explicit acknowledgement that images were not inspected.
5. **Storage rules**: source and restored text are never stored; reversible mappings are encrypted and expire; a retention worker enforces the limits.
6. **Launch guard**: public consumer mode is disabled by a check at startup, not only by deployment convention.

## Verifiable signals

| Signal | Value |
|---|---|
| Automated tests | 1,252 Python tests and 140 web tests passed after the audit fixes |
| Repository audit (Sep 2026) | 18 findings (7 high, 11 medium, 0 critical); first fix pass complete, 1 high finding still open |
| Credential detection | 18 types, each with tested formats |
| Architecture decisions | 43 recorded decisions (ADRs) |
| Document formats | DOCX, text PDF, CSV, and JSON, with the same format on output |

All testing uses synthetic data only. No real personal data was processed.

## Risk register

| Risk | Control / next gate |
|---|---|
| Undetected personal data stays in the text | The review screen shows detections before sharing; the product says recall is not perfect |
| The mapping and key together re-identify the text | Short expiry, instant erase, encryption at rest; the product never uses the word "anonymous" |
| Server logs capture document content | Audit finding fixed: a live HTTP probe confirmed logs contain no content |
| A race during parallel writes leaves a partial mapping | Open high finding; must be closed before a closed beta |
| Images in a document carry personal data | Images are not inspected; the download needs an explicit acknowledgement |

## Status and next gate

**Status: Amber / pre-beta.** The web app, API, document handling, and retention run on synthetic data. The address [RemovePII.app](https://removepii.app) is reserved and the hosted deployment is in setup, so the site may not answer yet. Production sign-in is not built, and public mode is locked by a startup check.

Next gate: close the open high finding, pass the acceptance and release checks, then open a closed beta.

## Stack

Python 3.12, FastAPI, PostgreSQL, Redis, Presidio, spaCy, Next.js, TypeScript, OpenAPI, Docker, pytest
