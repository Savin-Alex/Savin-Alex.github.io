---
title: AI Delivery Pipeline
---

# Multi-Agent AI Development Pipeline

[RU](../../projects/ai-dev-pipeline.md) · [EN](./ai-dev-pipeline.md)

A production-like prototype (2026): a three-agent workflow that takes a small task from a Jira issue to a ready GitHub Pull Request — with human oversight, quality checks, and traceable decisions.

The point is to show that GenAI/LLM in delivery is not "one-shot code generation" but a controlled process with quality, risk, and cost in view.

## Project Passport

| Field | Content |
|---|---|
| **Problem** | AI coding demos are a one-step scenario (paste a task → get code → check by hand). That helps productivity, but it isn't a delivery process: no intake, decomposition, validation, review, traceability, or human accountability. |
| **Goal** | Show how GenAI embeds into delivery not as "auto-generated code" but as a controlled workflow with acceptance criteria, quality gates, risk management, metrics, and an evidence package. |
| **In scope** | Jira issue intake → planning → implementation → sandbox validation → reviewer approval → GitHub PR; run evidence; rollback plan; production-like governance via GitHub Actions. |
| **Out of scope** | Parallel execution; extra agents (DevOps / documentation); production containerization; automating large, vague tasks. |

**Success criteria (with status):**
- AI output untrusted until explicit checks pass (tests, sandbox, secret scan, review) → **met**
- dry-run by default, real-run only under control (GitHub Actions) → **met**
- Local loop is stable → **met** (55 tests, clean lint)
- Per-run evidence (summary, metrics, logs) → **met** (150 run summaries across 11 issue groups)
- Production deploy, metrics dashboard, approval matrix → **not met** (next steps)

## My Role

**Role:** Junior IT Project Manager · solo project (prototype).

- Owned the goal, scope, backlog, roadmap, Definition of Done, risk register, quality gates, metrics, and the documentation used to verify results.
- Designed the process Jira → planning → implementation → validation → review → PR with two modes: a safe dry-run (no changes) and a controlled real run.
- Added quality checks: tests, isolated sandbox execution, secret scan, human review, and a budget guard on API spend.

## How the Process Works

The process is split into a safe dry-run and a controlled real run — so it's visible that the agents don't push code on their own, but work inside a managed loop.

- Jira issue intake with acceptance criteria.
- Planning and an implementation proposal.
- Validation in a restricted sandbox (no network access).
- Quality checks and a mandatory human review.
- GitHub Pull Request creation.
- An artifact bundle for the run.

## Checks and Control

AI output is treated as untrusted until it passes explicit checks:

- Mandatory tests and lint.
- Sandbox execution with no network access.
- Secret scan before creating the PR.
- Human review.
- Budget guard on model/API usage.
- Run artifacts: summary, stage statuses, metrics snapshot, sandbox logs, and per-issue history.

## Metrics Snapshot

| Metric | Signal |
|---|---|
| Run success rate | share of successful pipeline runs |
| Sandbox pass rate | share of runs that passed sandbox validation |
| Retry rate | frequency of re-iterations after failed tests / review |
| Cost per run | approximate cost of one GenAI workflow run |
| Stage cycle time | duration of individual stages |
| Failure distribution | error distribution by stage |
| Lead time to PR | time from run start to a ready PR |
| **Validation snapshot** | **55 automated tests pass, lint clean; 150 run records across 11 issue groups** |

Numbers are portfolio validation evidence, not a production benchmark. Status report: [Status report (sample)](/evidence/pipeline-status-report.html).

## Risk Register

Each risk carries probability, impact, severity, owner, trigger, mitigation, contingency, and review cadence. Key ones:

| Risk | Mitigation |
|---|---|
| Unsafe or non-working AI-generated code | mandatory tests, Docker sandbox, reviewer approval, retry loop |
| Secret leakage in generated code | secret scan, no `.env` in sandbox, sanitization rules for evidence |
| Uncontrolled real-run | dry-run by default, controlled GitHub Actions workflow, governance checks |
| LLM API budget overrun | budget guard, cost tracking, cost-per-run metric |
| Incomplete evidence artifacts | summary.json, sandbox logs, metrics snapshot, issue-level artifact history |

## How I Built It (by phase)

- **Prototype:** Python script — task → Claude (structured output) → pytest in isolation, up to 3 iterations on test failures.
- **Integrations:** thin Jira and GitHub clients with no AI (if the link doesn't work without AI, nothing else can be trusted).
- **Agents:** Planner (plan + acceptance criteria), Coder (code + tests), Reviewer (correctness, style, coverage, safety).
- **Orchestrator:** the full loop Jira → plan → code → review iterations → branch and PR → link back to Jira.
- **Analytics:** run over a set of tasks, collect metrics, analyze failures.

## Architecture

- `agents/` — prompts, schemas, and wrappers for planner, coder, and reviewer.
- `integrations/` — Jira and GitHub clients.
- `orchestrator/` — the main loop, retries, metrics, and error handling.
- `tests/` — unit tests for clients and orchestration logic.
- `docs/` — charter, backlog, roadmap, DoD, risk register, quality gates, runbook, rollback playbook, evidence package, metrics, retro, decision log, demo cases.

## Deliberate Limitations

- No parallel execution.
- No extra agents (DevOps, documentation).
- No production containerization in the first version.
- No attempt to automate large, vague tasks.

## Retro

- **Went well:** linked Jira → AI stages → sandbox validation → review → GitHub PR into one delivery flow; quality gates make the process controllable; run evidence explains every run; docs cover scope, risks, metrics, rollback, and acceptance.
- **To improve:** a metrics dashboard; an approval matrix for PM / Tech Lead / Security / Operator; an incident report template; more benchmark runs; a public sanitized evidence bundle.
- **Key takeaway:** the PM value here isn't "building an AI pipeline" — it's turning a technical experiment into a clear, measurable, controllable delivery process.

## Status

Working local prototype. The Jira → … → PR loop runs; run artifacts are saved for audit and review.

## Stack

Python, Claude API, Jira Cloud, GitHub API, GitHub Actions, pytest, Docker
