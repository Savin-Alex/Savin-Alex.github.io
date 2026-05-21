---
title: Mood&Food
---

# Mood&Food

[RU](../../projects/mood-food.md) · [EN](./mood-food.md)

PM/BA case study for a restaurant-chain waiter app. Capstone project of the IT Project Manager program (Yandex Practicum) — the full cycle from brief to solution defense.

## Project Passport

| Field | Content |
|---|---|
| **Problem** | A restaurant chain needs a waiter app; off-the-shelf systems are expensive (≈₽260,000–1,600,000/year for a 13-location chain) and often bloated. Without prioritization the product turns into a wish list — losing focus on the MVP and delivery plan. |
| **Goal** | Turn a broad product idea into a managed first release: run the full cycle from requirements and MVP scope to estimate, schedule, prototype, and a defended solution. |
| **In scope (MVP)** | Features 1.1–1.7 (auth, home screen, menu, ordering, loyalty, etc.), offline architecture (local DB, sync, queue), design of all screens, React Native. |
| **Out of scope (Post-MVP)** | Biometrics, menu search/filters, table booking, employee account, integrations (POS/acquiring) — estimated separately. |

**Status:** capstone project, team of 4.

**Success criteria (with status):**
- MVP carved out of 40+ features via MoSCoW → **met**
- Estimate and schedule prepared → **met** (estimate ≈ ₽5.27M)
- Prototype for requirements sign-off → **met** (Figma)
- Solution defended in front of reviewers → **met**
- Real development and release → **out of scope** (training project)

## My Role

**Role:** IT Project Manager / Business Analyst · team of 4 (capstone).

- Clarified the product idea and constraints; ran a competitor analysis (15 systems).
- Structured requirements and feature candidates, prioritized scope with MoSCoW.
- Produced the project passport, commercial proposal, estimate, GanttPRO schedule, and risk register.
- Built an interactive Figma prototype and defended the solution in front of reviewers.

## Metrics Snapshot

| Area | Signal |
|---|---|
| Features prioritized (MoSCoW) | 40+ |
| MVP scope | features 1.1–1.7 + offline architecture |
| Roadmap | 20+ features (MVP + Post-MVP) |
| MVP estimate | ≈ ₽5.27M (~1,474 h; team: designer / frontend / tester / manager; rate ₽3,500/h) |
| Post-MVP estimate | ≈ ₽5.76M (preliminary) |
| Competitor analysis | 15 systems (Saby Presto, iiko, r_keeper, Restik, Quick Resto…) |
| Risk register | 50+ risks with probability, severity, strategy, and plan |
| Team | 4 people (Jira) |

## Risk Register (top 5)

The full register has **50+ risks** with classification, source, root cause, probability, severity (score), strategy, and plan. Full excerpt: [Risk register](/evidence/mood-risk-register.html). Key ones:

| Risk | Prob. · Severity | Strategy | Mitigation |
|---|---|---|---|
| Backend complexity underestimated (estimated from high-level requirements) | High · 9 (Large) | React | Re-estimate once detailed requirements arrive; allocate extra resources. |
| Client unwilling to pay for edits → scope creep, technical debt | High · 6 (Large) | Transfer | Change Request as a separate spec, edit limits, "batch of edits" into releases, detailed spec. |
| Fixed price without a buffer → 30–70% overrun | High · 6 (Large) | Avoid | Analysis first → detailed spec and accurate estimate; hybrid Fix + T&M; buffer in plan and estimate. |
| "Everything at once" without prioritization → complex UI, low usability | Med · 4 (Medium) | Mitigate | MoSCoW workshop, strict MVP, PO with veto, "user advocate". |
| Team churn / burnout → delays, knowledge loss | Med · 4 (Medium) | Mitigate | Milestone bonuses, IDPs, knowledge management (Confluence/README), regular 1-to-1s. |

## PM/BA Artifacts

- Project passport · commercial proposal.
- Requirements and MVP scope · MoSCoW prioritization of 40+ features · 20+ feature roadmap.
- Competitor analysis (15 systems) · risk register (50+).
- Estimate and GanttPRO schedule · Jira backlog for a team of 4.
- Interactive Figma prototype · final presentation and defense.

## Retro

- **Went well:** the full artifact cycle (passport, 40+ MoSCoW, estimate, 15-competitor analysis, 21 risks, Gantt, Figma) reduced a broad idea to a defensible MVP; prioritization and risk analysis made scope controllable.
- **To improve:** the top risk was estimating from high-level requirements (backend underestimated); next time, do detailed analysis first → an accurate estimate, and avoid fixed price without a buffer.
- **Key takeaway:** the PM/BA value is turning a wish list into a managed MVP with a justified estimate, clear priorities, and understood risks.

## Live Prototype

Open prototype: [Mood&Food Project](https://genre-jungle-62178054.figma.site)

If the prototype asks for login details, any email and password can be used.
