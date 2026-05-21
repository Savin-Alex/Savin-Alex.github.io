---
title: "AI Consul — Decode-policy A/B (benchmark)"
---

# AI Consul — Decode-policy A/B (benchmark)

[← back to the AI Consul project](/projects/ai-consul.html)

_Sample engineering artifact: a reproducible A/B comparison of two real-time Whisper decode policies, extracted from session logs by a committed script (`scripts/policy-ab-compare.sh`). Numbers are worst-observed over two paired sessions on one machine (Apple Silicon, `small.en`) — an internal A/B, not a multi-condition benchmark._

**Source logs:** `Logs/policy2-hard.log` (baseline · LocalAgreement-2) · `Logs/policy1-hard.log` (variant · AlignAtt/SimulStreaming)

| Metric | Policy 2 (baseline) | Policy 1 (variant) | How to verify |
|---|---|---|---|
| Worst-case commit lag | 23,332 ms | 624 ms | `grep -oE '"commitLagMs":[0-9]+' Logs/policy2-hard.log \| sort -t: -k2 -n \| tail -1` |
| p50 / p95 / p99 lag | 0 / 4852 / 15228 ms | 0 / 0 / 0 ms | `policy-ab-compare.sh` session-stats block |
| Watchdog resets | 15 | 0 | `grep -c 'resetting WLK' Logs/policy2-hard.log` |
| Dropped hallucinated commits | 16 | 0 | `grep -c 'Dropping likely hallucinated commit' Logs/policy2-hard.log` |
| Clean-prefix recoveries | 153 | 0 | `grep -c 'clean.prefix' Logs/policy2-hard.log` |
| Errors | 24 | 0 | session-stats `err=` field |
| Duration / transcriptions | 3836 s / 3131 | 2771 s / 3443 | session-stats `dur=` / `tx=` |

**Reproduce the whole block:** `bash scripts/policy-ab-compare.sh Logs/policy2-hard.log Logs/policy1-hard.log`

## Cold-start case (fully logged)
**Source log:** `Logs/logs6`

| Event | Value | Location |
|---|---|---|
| Warmup failed (network-fetched asset) | timeout after 60,000 ms | `Logs/logs6` — "warmup timeout after 60000ms" |
| Peak cold-start commit lag | 20,204 ms | `Logs/logs6` — `commitLagMs:20204` |
| Hallucinated commit dropped | 1,476 chars | `Logs/logs6` — "Dropping likely hallucinated commit … textLen:1476" |
| Watchdog reset triggered | lastCommitLagMs 20,204 | `Logs/logs6` — "Watchdog: resetting WLK … reason: commit-lag stall" |

**Fix:** bundle the warmup asset locally (`assets/warmup-jfk.wav`) and verify warmup on boot.

_Honest framing: a single worst-case value from two non-length-matched sessions is an observation, not a guarantee — broader benchmarking across hardware, languages, and noise is still needed before presenting it as one._
