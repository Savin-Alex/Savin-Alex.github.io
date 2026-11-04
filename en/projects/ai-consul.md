---
title: AI Consul
---

[RU](../../projects/ai-consul.html) · [EN](./ai-consul.html)

**November 2025 – present · Desktop app · Prototype**

AI Consul is a desktop assistant for interviews, meetings and learning sessions. It shows a live transcript and AI suggestions and processes audio on the device by default.

## The challenge

Conversation assistants usually send audio to the cloud and hide their failures. Users cannot see where their data is processed or what it costs. Real-time transcription without careful tuning lags behind speech and adds phrases that nobody said.

## My contribution

**Solo project: I owned the product, UX, engineering and project delivery.**

- Defined the product concept, target scenarios and the principle that data stays on the device by default.
- Designed a three-window interface: session, transcript and suggestions on top of other windows.
- Built real-time speech recognition and chose the transcription algorithm based on an A/B comparison.
- Reviewed real session logs and closed each failure type with a permanent safeguard and an automated test.

## What I built

<ol class="project-flow">
  <li><strong>Live transcription</strong><span>Speech is recognized on the device; if an engine fails, the app switches to a backup engine.</span></li>
  <li><strong>AI suggestions</strong><span>A local language model uses the resume and the job description; cloud models act as a backup.</span></li>
  <li><strong>Status in the interface</strong><span>Users see which engine is running, when the cloud is in use and what requests cost.</span></li>
  <li><strong>Session archive</strong><span>Past conversations are saved and can be reviewed locally.</span></li>
</ol>

## The result

**A working prototype with a live transcript and suggestions; by default, audio and text stay on the device.**

- In an A/B comparison of two algorithms, the worst-observed transcript lag dropped from about 23 s to under 1 s.
- Recognition stalls fell from 15 to 0 per session.
- Fixed a lag spike of about 20 s on first launch: the model warm-up no longer depends on the network.
- The cloud is only a backup that the user chooses, and the interface shows when it is in use.

<figure class="demo-figure">
<iframe class="demo-frame demo-consul" src="{{ '/demos/ai-consul.html' | relative_url }}" title="Interactive demo: AI Consul live session" loading="lazy"></iframe>
<figcaption style="font-size:.9em;color:#667085;margin-top:.4em;">Interactive live-session animation: transcript, audio waveforms, and suggestion generation. <a href="{{ '/demos/ai-consul.html' | relative_url }}" target="_blank" rel="noopener">Open full screen ↗</a></figcaption>
</figure>

## Next steps

So far the measurements come from one computer. A release needs testing on different hardware and languages, Windows support, a security review and testing with more users.

<details markdown="1"><summary>Details: decisions and risks</summary>

- **A failure at a real user's location.** The model warm-up downloaded a file from the network and timed out for the user. The first session then showed a lag spike and invented text. I bundled the file with the app and added a warm-up check at startup.
- **Honest latency measurement.** Under the new algorithm, latency percentiles read as zero and hid real spikes. Before each release I now check the worst-observed lag and the share of spikes.

</details>

**Technologies:** Electron, TypeScript, React, Python, WhisperLiveKit, whisper.cpp, Ollama, OpenAI, Anthropic and Google models.
