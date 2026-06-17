---
title: DemoTel KZ — Telegram Bot
---

# DemoTel KZ · Telegram support bot

[RU](../../projects/telecombot.md) · [EN](./telecombot.md)

A deterministic (no-LLM) Telegram support bot for a mobile operator (demo project, 2026): safety-first routing of customer requests with PII protection at the core.

The point is that not every task needs GenAI. For a regulated telecom, predictability, testability, and control matter more than "smart" answers — so the routing core is deliberately deterministic.

<figure class="demo-figure">
<iframe class="demo-frame demo-telecom" src="{{ '/demos/telecombot.html' | relative_url }}" title="Interactive demo: DemoTel KZ — Telegram support bot" loading="lazy"></iframe>
<figcaption>Interactive animation: request → webhook → PII masking → risk classification → route (L1 / Secure / Fraud·L2). The demo UI is in Russian. <a href="{{ '/demos/telecombot.html' | relative_url }}" target="_blank" rel="noopener">Open full screen ↗</a></figcaption>
</figure>

## Project passport

| Field | Content |
|---|---|
| **Problem** | Operator support in a messenger must answer instantly, but some requests are high-risk (SIM-swap, fraud, stolen phone). A "smart" LLM bot is dangerous here: unpredictable, hard to test, easy to push into saying what it shouldn't. And PII flows through the chat (number, IIN, OTP, cards). |
| **Goal** | A deterministic bot that safely closes routine requests at L1 and reliably escalates high-risk scenarios to the right channel, without leaking PII into processing or logs. |
| **In scope** | 50 scenarios (S01–S50) across 7 categories + operator handoff; risk-policy routing (L1 auto · Secure Web App · Fraud / L2); PII masking before processing; secret-token webhook verification; test suite and P0 gates. |
| **Out of scope** | Real billing/CRM integration; production deployment; LLM answers; multilingual support; voice channel. |

**Success criteria (with status):**
- High risk is never closed by L1 automation → **done** (bypass rule + tests)
- PII never reaches processing or logs → **done** (MSISDN / IIN / OTP / card masking before routing)
- Behavior is predictable and reproducible → **done** (0 LLM in prod, 84/84 tests)
- Critical scenarios closed before release → **done** (5/5 P0 blockers closed)
- Production integration with operator systems → **not done** (demo project, out of scope)

## My role

**Role:** Junior IT PM · solo project (demo).

- Defined the routing policy and risk classification across 50 scenarios.
- Made the key product call — no LLM in production: determinism for predictability, testability, and control.
- Built privacy-by-design: PII masking before any processing, a secret token on the webhook.
- Wrote acceptance criteria and P0 gates, plus a test per scenario and route.

## How it works

Every request goes through a deterministic pipeline — no model "guessing":

- **Telegram Bot API** — receives the update via webhook.
- **FastAPI webhook** — verifies the secret token (non-Telegram requests are dropped).
- **PII masking** — MSISDN, IIN, OTP, and card numbers are masked before routing and logging.
- **Policy & risk** — classifies the request (LOW / HIGH) by scenario.
- **Scenario router (S01–S50)** — picks a prepared answer or an escalation.

**Outcome routes:**
- **L1 · auto** — self-service (balance, packages, roaming…).
- **Secure Web App** — OTP / KYC / payment operations in a secured mini-app.
- **Fraud · L2** — high-risk scenarios into a secure, identity-checked channel.

The core rule: **high risk bypasses L1 automation** — the bot doesn't try to "solve" a SIM-swap or theft itself; it hands off to Fraud / L2 immediately.

## Security & control

- 0 LLM in production — deterministic answers that can be fully covered by tests.
- PII masking before processing; secret-token webhook verification.
- High-risk scenarios are policy-barred from closing at L1.
- 84/84 tests; 5/5 P0 blockers closed before the demo release.

## Metrics (snapshot)

| Area | Signal |
|---|---|
| Scenarios | 50 (S01–S50) |
| Categories | 7 + operator handoff |
| Tests | 84/84 passing |
| P0 blockers closed | 5/5 |
| LLM in production | 0 |
| Masked PII types | MSISDN, IIN, OTP, card numbers |
| Routes | L1 auto · Secure Web App · Fraud / L2 |

Numbers are portfolio validation evidence for a demo project, not an operator's production metrics.

## Risk register

| Risk | Mitigation |
|---|---|
| A high-risk scenario falls into an L1 auto-answer | Bypass policy + a test for every HIGH scenario |
| PII leaking into processing or logs | Masking MSISDN / IIN / OTP / cards before routing; PII-safe logging |
| Forged webhook requests | Secret-token verification; dropping non-Telegram traffic |
| Unpredictable answer to the user | No LLM in production; deterministic 50-scenario router |
| Regression when scenarios change | 84/84 tests + P0 gates as a release condition |

## Retro

- **What worked:** a pre-defined risk policy and the bypass rule for HIGH scenarios; PII masking at the entry point; full test coverage made behavior reproducible.
- **To improve:** real billing/CRM integration, multilingual support (KZ/RU/EN), load testing, production deployment.
- **Key takeaway:** PM maturity includes knowing when NOT to use GenAI — where predictability, safety, and testability matter more.

## Status

Demo / portfolio prototype. The "request → webhook → masking → risk → route" loop works and is test-covered; production integration with operator systems is out of scope.

## Stack

Python, FastAPI, PostgreSQL, Telegram Bot API, Docker
