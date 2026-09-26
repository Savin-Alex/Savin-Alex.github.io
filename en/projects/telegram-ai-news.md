---
title: Telegram AI News — governed LLM content pipeline
---

[RU](../../projects/telegram-ai-news.html) · [EN](./telegram-ai-news.html)

An LLM content pipeline and a local operator console for a Russian-language Telegram channel about AI. It is not a "publish everything" aggregator: it reads sources every 15 minutes, filters and scores stories with LLMs, and writes drafts. The channel runs **unattended today**, and the rails that make that safe are in the code, not in a person.

The case is about running generative AI as a controlled process: staged LLM calls with structured output, code that can only make a model's decision stricter, a hard cost boundary, and publishing rails that bound what an unattended run can do.

<p><b>Live channel:</b> <a href="https://t.me/ainews24by7" target="_blank" rel="noopener">ИИнтересные новости — t.me/ainews24by7</a> · 283 subscribers (22 September 2026)</p>

## Project passport

| Field | Content |
|---|---|
| **Problem** | AI news channels either repost press releases or publish everything. Readers who build with AI need fewer posts with sources, numbers, and a skeptical comment. |
| **Goal** | Every post explains in 30 seconds what happened, why it matters to practitioners, and which number or source to click. |
| **In scope** | RSS intake from managed sources (only top-trust sources are enabled today); dedup; LLM extract → score → synthesize; draft queue and digest slots; localhost operator console; editorial policies; static archive site; cost and usage reporting. |
| **Out of scope** | Covering the whole AI market; being the fastest aggregator; following links to full article bodies; generated cover images in v1. |

**Testable v1 hypothesis:** a Russian-speaking audience that already follows AI values the filter more than the volume.

## My role

**Role:** IT Project Manager · solo product; problem framing, architecture, development, and operations.

- Wrote the reader promise, the "we do not promise" list, and the editorial policies (corrections, sponsorship, privacy, source trust) before growth.
- Recorded 20 design decisions with trade-offs, from "multi-stage pipeline instead of one mega-prompt" to "fail-safe scoring always returns SKIP".
- Set the release gates: approval-required publishing until launch, a security audit before launch, and CI with a locked dependency install. Automatic publishing came later, and only with its own rails.

## Solution design

```
RSS → Fetch → Dedup → Extract (Haiku 4.5) → Score (Haiku 4.5)
                                               ↓
                                  SKIP · WRITE_QUEUE · WRITE_NOW
                                               ↓
                              Synthesize (Sonnet 5) → Stored draft
                                               ↓
          Top-trust source: published · any other source: held as a draft
                                               ↓
                                  Telegram / digest slot
```

- **One LLM call per stage**, each with structured output. Prompts live in text files, so editorial changes need no code change.
- **Cheap models filter, the strong model writes:** Haiku 4.5 extracts and scores; Sonnet 5 writes only the posts that passed. Ollama is a local alternative for every stage.
- **Trust rules in the output:** every number links to its source; low-trust (tier-3) sources get a "rumor" prefix; a fixed taxonomy of 8 categories.

## Controls

| Control | How it works |
|---|---|
| Publishing mode | Two modes: approval-required and automatic. The console starts paused, and switching to automatic needs a typed confirmation. The channel now runs in automatic mode |
| Rails for unattended runs | Only top-trust (tier 1) sources are enabled, and only they may publish by themselves. Any lower-trust source is held as a draft for a person. At most 10 autonomous posts per run act as a kill switch |
| Prompt-injection boundary | Feed content is wrapped as data; code recomputes the score and can only downgrade the model's verdict; out-of-range scores force `SKIP` |
| Link integrity | Source buttons are rebound to the real item source; inline links must point to the item or its extracted numbers |
| Cost boundary | Only sources up to a configured trust tier enter the LLM pipeline; every LLM response is recorded with model, stage, tokens, and cost |
| Spend circuit breaker | Auth or billing errors (401/402) open a persistent breaker and stop new fetch runs; a backlog cutoff prevents paying for stale retries after recovery |
| Fetch security | SSRF protection: every redirect hop is validated, the connection is pinned to the validated IP, response bodies are capped |

## Verifiable signals

| Signal | Value |
|---|---|
| Automated tests | 243 test functions; CI runs tests, a hash-locked install, `pip-audit`, and an end-to-end test |
| Security audit | 13 findings remediated (token leakage, model-controlled publishing, link injection, SSRF, CSP, file permissions); no open critical findings as of July 2026 |
| Design decisions | 20 recorded, each with the alternative and the trade-off |
| Managed sources | 56 feeds configured, including Russian AI company blogs; only top-trust feeds are switched on |

## Risk register

| Risk | Control / next gate |
|---|---|
| Prompt injection through a feed item | Cannot be fully removed. Only top-trust sources are enabled, code can only make a verdict stricter, and the per-run cap bounds the damage |
| A wrong number reaches readers | Every number must link to a source; a written corrections policy (reply to the post, never a silent delete) |
| LLM spend grows without value | Tier boundary, per-stage cost records, and the billing breaker |
| Growth before trust | Editorial and sponsorship policies are written before the public launch |

## Status and next gate

**Status: Amber / live.** The pipeline, console, and controls are built, and the public channel is live with 283 subscribers (22 September 2026).

Next gate: measure the v1 hypothesis (retention and reactions per post) against the cost per published post.

## Stack

Python 3.11+, FastAPI, Jinja, SQLite, Anthropic SDK (Haiku 4.5, Sonnet 5), Ollama, python-telegram-bot, APScheduler, pytest, GitHub Actions
