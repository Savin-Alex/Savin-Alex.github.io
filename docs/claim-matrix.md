# Claim Matrix

This file keeps the public presentation honest by tying each portfolio claim to a source in the private repository.

## Safe Public Claims

| Public claim | Why it is safe | Repo evidence |
| --- | --- | --- |
| AI Consul is a multi-window Electron desktop app. | The app creates separate main, companion, and transcript windows. | `src/main/main.ts` |
| The product is local-first rather than cloud-only. | The UI exposes local-only and local-first modes, and the engine is configured around offline-first behavior with optional cloud fallback. | `src/renderer/components/Settings/Settings.tsx`, `src/main/main.ts`, `src/core/engine.ts` |
| It captures microphone input and can optionally include system audio. | The renderer audio manager requests microphone input and has an explicit system-audio capture path. | `src/renderer/utils/audio-capture.ts` |
| Audio is downsampled before transcription. | The worklet and session pipeline explicitly resample toward 16 kHz. | `src/renderer/public/core/audio/audio-worklet-processor.js`, `src/core/session.ts` |
| The app uses a routing strategy instead of a single transcription backend. | The router and engine both define fallbacks across multiple engines. | `src/core/audio/transcription-router.ts`, `src/core/engine.ts` |
| WhisperLiveKit is managed as a Python sidecar/runtime. | The main process starts a Python-managed WhisperLiveKit service and exposes a WebSocket URL. | `src/main/audio/python-manager.ts`, `src/main/main.ts` |
| Local `whisper.cpp` support is part of the stack. | The engine initializes `WhisperNative` and package dependencies include whisper-native tooling. | `src/core/engine.ts`, `package.json` |
| The app tracks operational metrics such as latency percentiles and cloud cost. | The performance monitor records p50, p95, p99, and session cost. | `src/core/performance-monitor.ts`, `src/renderer/components/Settings/EngineStatus.tsx` |
| Suggestions are contextual rather than generic text completions. | The engine maintains prompt, context, and RAG components before generating suggestions. | `src/core/engine.ts` |

## Claims To Phrase Carefully

| Claim to avoid or soften | Safer wording | Why |
| --- | --- | --- |
| `100% local processing` | `local-first with explicit cloud and hybrid modes` | Cloud backends are first-class when enabled. |
| `sub-500ms transcription` | `latency-aware streaming path with sub-second targets when the stack supports it` | Code contains latency targets, but the current repo does not provide a clean, public benchmark number to publish. |
| `fully offline in all modes` | `offline-capable when local models and local LLMs are available` | Some suggestion and transcription paths depend on external services if enabled. |
| `speaker diarization by default` | `speaker-aware transcript support when diarization is enabled` | Diarization is configurable and not always on. |
| `native system audio pipeline is already the primary shipped path` | `system-audio capture is supported; a native module also exists in the repo` | The native module is present, but the visible app path in this branch is renderer-driven capture. |

## Presentation Guardrails
- Prefer `local-first`, `hybrid-capable`, `latency-aware`, and `graceful fallback`.
- Avoid publishing a hard latency table until measured from the current machine and branch.
- Present the Python component as a sidecar/runtime, not as the main application.
- Use the architecture and pipeline diagrams to explain the system, not internal review documents.
