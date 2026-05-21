---
title: IT / AI Project Manager Portfolio
---

# Alexander Savin

[RU](../) · [EN](./)

**Contacts:** [Email](mailto:thealexsavin@yandex.ru) · [Telegram](https://t.me/theCyberAlex) · [Download CV (RU, PDF)](/CV.pdf)

## IT / AI Project Manager

5 years leading projects in engineering and construction: budgets up to ₽130M, teams of up to 15, contractors, and hard deadlines. Now deliberately moving into IT/AI — I completed the IT Project Manager program at Yandex Practicum and run my own AI projects.

Focus: managing IT/AI projects — requirements, backlog, MVP, schedules, risks, and stakeholder communication. A technical background (Python, APIs, AI/LLM) lets me speak the same language as the engineering team.

---

## Project Management Experience

**Project Manager / Lead Project Engineer (ГИП)** · engineering and construction · 2015–2025

- 5 years in a leadership role: ran industrial-construction projects — largest budget ₽130M+, teams of up to 15, 5+ contractors.
- Delivered on time despite changing decision-makers and immature requirements — via weekly risk reviews and fast onboarding of new clients.
- Built scope-change and stakeholder-management processes.
- Earlier — Lead Project Engineer and acting roles in the same organization.

Planning, risks, deadlines, contractors, and stakeholders — these skills transfer directly to IT/AI projects.

---

## Transition into IT/AI

- **Yandex Practicum — IT Project Manager** (2026): the full IT-project cycle — from requirements gathering and prioritization to budgeting and defending the solution to the client (Jira, GanttPRO, Figma).
- **Codecademy — Computer Science** (2025): algorithms, data structures, programming fundamentals.
- Currently taking **Google IT Project Management** (in English). English — B2 (Upper-Intermediate).

---

## Projects

### Multi-Agent AI Development Pipeline · prototype, 2026

A production-like prototype of a managed, three-agent process: from a Jira task to a ready GitHub PR with tests, quality checks, and saved per-run artifacts. GenAI as a controlled process, not "auto-generated code".

- Built the project frame (charter, backlog with acceptance criteria, roadmap, risk register) and the process: planning → implementation → sandbox validation → review → PR.
- Added quality checks: mandatory tests, network-isolated sandbox, secret scan, human review, API budget limit.
- Confirmed local stability: 55 automated tests, clean lint, 150 run summaries across 11 tasks.

Stack: Jira, GitHub Actions, Docker, Python, Claude API, pytest

[Open project page](projects/ai-dev-pipeline.md) · [evidence](/evidence/pipeline-status-report.html)

---

### AI Consul · product in development, 2026

Privacy-first AI assistant for live conversations, interviews, meetings, and learning sessions.

- Designed a desktop product with live transcription, an always-on-top companion overlay, and session control.
- Architected a local-first/hybrid audio pipeline with explicit privacy and cloud-fallback controls — transcription and LLM run on-device by default.
- In an internal A/B, worst-observed commit lag fell from ~23s to <1s, and per-session decoder stalls (15→0) and dropped hallucinations (16→0) were eliminated — by switching the default decode policy to AlignAtt/SimulStreaming.
- Surfaced engine status, cost, and privacy settings in the UI so technical risks stay legible to non-technical users.

Stack: Electron, TypeScript, React, Python/WhisperLiveKit, Ollama

[Open project page](projects/ai-consul.md) · [evidence](/evidence/consul-ab-benchmark.html)

---

### Local PII Anonymizer · service in development, 2026

A local offline service for reversible PII/PHI pseudonymization in RU/EN documents, designed for 152-FZ, GDPR, and HIPAA Safe Harbor contexts.

- Cascade detection: regex with checksum → Presidio/spaCy → optional Medical NER → LLM fallback; 17+ entity types.
- Eval-driven CI: lab metric macro F1 ≈ 0.9997 (15/17 types = 1.0) on a synthetic corpus — an internal eval gate, not a production guarantee. 367 tests, 15 ADRs.

Stack: Python 3.12, FastAPI, Streamlit, Presidio, spaCy, Docker

[Open project page](projects/local-pii-anonymizer.md) · [evidence](/evidence/pii-status-report.html)

---

### Mood&Food · capstone project, Yandex Practicum

A PM/BA case study for a restaurant-chain mobile app.

- Ran the full PM/BA cycle: passport, proposal, requirements, MVP scope, roadmap, estimate (≈₽5.27M), GanttPRO schedule, Jira.
- Prioritized 40+ features with MoSCoW (MVP + 20+ roadmap); analyzed 15 competitors; kept a 50+ risk register.
- Built a Figma prototype and defended the solution to reviewers.

Stack: Figma, GanttPRO, Jira

[Open project page](projects/mood-food.md) · [evidence](/evidence/mood-risk-register.html)

---

## Skills

Management: Agile, Scrum, Kanban, Waterfall, requirements management, prioritization (MoSCoW), MVP planning, risk management, budgeting, stakeholder management.
Tools: Jira, GanttPRO, Figma, Miro, MS Project, Excel / Google Sheets.
Technical background: SDLC, Python, APIs (Claude / OpenAI / Jira / GitHub), prompt engineering.
