---
title: AI Consul
---

# AI Consul

[RU](../../projects/ai-consul.md) · [EN](./ai-consul.md)

Privacy-first AI assistant for live conversations, interviews, meetings, and learning sessions.

![AI Consul overview](../../docs/images/hero-overview.svg)

AI Consul is a desktop product that gives users live transcripts and AI suggestions during a conversation while keeping privacy, fallback behavior, and audio-processing modes visible and controllable.

## Project Passport

| Field | Content |
|---|---|
| **Problem** | Conversation assistants stream audio to the cloud by default, hide their failure modes, and give users little visibility or control over how data is processed. Real-time transcription is also laggy and prone to hallucination without careful tuning. |
| **Goal** | A privacy-first desktop assistant that delivers live transcripts and AI suggestions during interviews, meetings, and learning sessions — on-device by default, with explicit privacy/fallback controls and visible engine, cost, and network status. |
| **In scope** | Multi-window desktop UX (session · transcript · always-on-top companion overlay); real-time local STT via WhisperLiveKit behind a 5-engine fallback router; local LLM (Ollama) with cloud fallback (OpenAI/Anthropic/Google); resume/JD grounding; session archive + local-first review; privacy/cost/engine status in the UI. |
| **Out of scope** | Production packaging/code-signing/update hardening; Windows native system audio; formal security review + data-retention/consent flows; multilingual real-audio validation; multi-condition benchmarks. |

**Success criteria (with status):**
- Local-first by default — transcription + LLM run on-device, cloud opt-in → **met**
- Bounded real-time lag — worst-case commit lag < ~1s, no decoder stalls → **met** (23s → <1s; 15 → 0 watchdog resets)
- Privacy invariant — no document bytes cross the main→renderer boundary → **met** (booleans only)
- Transparent failure modes — engine status, fallback, and cost visible → **met**
- Gated correctness — automated tests + worst-case-lag/spike-rate release gates → **met** (638 tests)
- Production-ready — signing, multi-platform, security review, multilingual validation, formal benchmarks → **not met** (deliberately out of scope)

## My Role

**Role:** Junior IT PM · solo project (aiming for production); product, UX, and engineering on me.

- Defined the product concept, target scenarios, and privacy-first positioning.
- Designed the desktop UX: main session screen, transcript window, and companion overlay.
- Defined a local-first/hybrid approach to audio processing with explicit privacy and fallback controls.
- Built and benchmarked the real-time transcription pipeline, including an A/B comparison of streaming decode policies.
- Made engine status, cost awareness, and privacy settings visible in the UI so technical risks are understandable to users.

## Product Experience

<figure class="demo-figure">
<iframe class="demo-frame demo-consul" src="{{ '/demos/ai-consul.html' | relative_url }}" title="Interactive demo: AI Consul live session" loading="lazy"></iframe>
<figcaption style="font-size:.9em;color:#667085;margin-top:.4em;">Interactive live-session animation: transcript, audio waveforms, and suggestion generation. <a href="{{ '/demos/ai-consul.html' | relative_url }}" target="_blank" rel="noopener">Open full screen ↗</a></figcaption>
</figure>

## Product Decisions

- Multi-window desktop UX instead of a single chat interface.
- Local-first defaults with explicit hybrid/cloud controls.
- Engine status and cost awareness as product features.
- Fallback behavior as a managed scenario, not a hidden technical error.
- Separate transcript and companion surfaces for high-focus conversations.

## Technical Highlights

- **Streaming decode A/B (measured).** Built a repeatable A/B harness comparing two real-time Whisper decode policies — LocalAgreement-2 vs AlignAtt/SimulStreaming (two paired sessions). Switching the default to AlignAtt dropped worst-observed commit lag from ~23s to under 1s, and decoder stalls (15 → 0 watchdog resets) and dropped-hallucination events (16 → 0) went to zero per session. The numbers are logged and reproducible via the A/B compare script.
- **Cold-start diagnosis.** Traced first-session hallucinations — a ~20s lag spike and a 1,476-character garbage transcript — to a model warmup that depended on a network-fetched asset and silently timed out; fixed by bundling the warmup locally.
- **Honest latency metrics.** Found that percentile latencies collapse to zero under the new policy's commit pattern, so averages alone would hide real spikes; added worst-case-lag and spike-rate checks as pre-ship gates instead.

## Metrics Snapshot

Measured on paired ~1-hour sessions, single machine (Apple Silicon, small.en); internal A/B, not a multi-condition benchmark. Full report: [A/B benchmark](/evidence/consul-ab-benchmark.html).

| Metric | Result |
|---|---|
| Worst-observed STT commit lag (decode-policy A/B) | 23s → <1s |
| Decoder stalls (watchdog resets) / session | 15 → 0 |
| Dropped hallucinated commits / session | 16 → 0 |
| Pipeline errors / session | 24 → 0 |
| Cold-start lag spike (warmup fix) | 20.2s → eliminated |
| STT engines behind one fallback router | 5 (WhisperLiveKit, whisper.cpp, Apple Speech, Deepgram, AssemblyAI) |
| LLM providers (1 local default + 3 cloud fallback) | 4 (Ollama · OpenAI · Anthropic · Google) |
| Automated tests | 638 passing |
| Document bytes crossing main→renderer IPC | 0 (booleans only — privacy invariant) |

## Risk Register

| # | Risk | Likelihood | Impact | Status | Mitigation |
|---|---|---|---|---|---|
| R1 | Model warmup depends on a network-fetched asset; if unreachable, the first session degrades | Med | High | 🔴 Realized | Hit at a real user's location: 60s warmup timeout → 20.2s commit-lag spike + a 1,476-char hallucinated transcript. Fixed by bundling the warmup asset locally and verifying warmup on boot. |
| R2 | "Local-first" can still leak data once hybrid/cloud backends are enabled | Med | High | 🟡 Mitigated | Explicit privacy mode + per-engine status surfaced in the UI; document bytes never cross the main→renderer boundary (booleans only). Residual: needs a formal data-retention/consent flow. |
| R3 | Latency presented as a guarantee without broad benchmarks | Med | Med | 🟠 Open | Numbers come from an internal A/B over a small N on one machine; needs multi-hardware/language/noise runs before becoming a public claim. |
| R4 | Cross-platform parity — native macOS audio + bundled Python unverified on Windows | High | Med | 🟠 Open | Tracked as a backlog lane (CI artifact must actually run; native WASAPI fix-or-disable). Not yet addressed. |

One realized (R1), one mitigated (R2), two open (R3–R4) — honest about what is still unproven. R1 here and the cold-start row in the metrics are the same story: a risk identified, watched as it realized, measured, and closed.

## Retro / Lessons Learned

- **What worked:** treating performance as an experiment — a repeatable A/B harness plus per-commit instrumentation turned a vague "feels laggy" into a measured worst-observed-lag win (~23s → <1s), making the decode-policy switch a data decision instead of a guess.
- **What didn't:** placeholder and environment-dependent pieces that failed silently — a stub summarizer anchored suggestions to stale context, and a model warmup depended on a network fetch and timed out at the user's location (~20s first-session lag and hallucinated transcripts).
- **What I'd change:** instrument latency and bundle/verify runtime dependencies from day one, and validate metric methodology before trusting it — percentile latencies read as "0ms" and hid real spikes until I added monotonic worst-case + spike-rate gates.

## Status

- Private repository.
- Public case study includes an interactive demo, product framing, and a measured A/B benchmark.
- Production readiness would require broader formal benchmarks, security review, packaging hardening, and wider user testing.

## Stack

Electron · TypeScript · React · Python · WhisperLiveKit (mlx-whisper / SimulStreaming) · Ollama · whisper.cpp (batch fallback)
