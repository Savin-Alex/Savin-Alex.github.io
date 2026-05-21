# AI Consul Deck

Six-slide self-guided portfolio deck for GitHub, PDF export, or Gamma/Keynote recreation.

## Slide 1
**Title:** AI Consul  
**Subtitle:** Privacy-first AI assistance for conversations, interviews, and meetings

**Visual**
- Use `docs/images/hero-overview.svg`

**On-slide copy**
- Real-time audio capture, transcription routing, and contextual suggestions
- Multi-window desktop UX built for high-focus conversations
- Local-first by default, with explicit hybrid and cloud controls

## Slide 2
**Title:** The Product Problem

**Visual**
- Use a simple text-led layout with one supporting UI crop or icon row

**On-slide copy**
- Most conversation assistants hide privacy trade-offs behind cloud defaults
- Real-time tools also fail badly when the ideal backend is unavailable
- Users need clarity, fallback behavior, and control over where their data goes

## Slide 3
**Title:** The Experience

**Visual**
- Use `docs/images/ui-overview.svg`

**On-slide copy**
- Main window to configure and start a session
- Transcript window to monitor what the system hears
- Companion overlay to surface suggestions without taking over the screen
- Settings that expose privacy, performance, and audio choices directly

## Slide 4
**Title:** High-Level Architecture

**Visual**
- Use `docs/images/architecture.svg`

**On-slide copy**
- Electron renderer handles the user-facing windows and audio capture
- Main process manages session state, IPC, and backend coordination
- Context, prompts, and suggestion generation sit downstream from transcription

## Slide 5
**Title:** Why The Pipeline Matters

**Visual**
- Use `docs/images/pipeline.svg`

**On-slide copy**
- Attempt the best live route first
- Fall back to buffered batch transcription when needed
- Keep cost, network, and engine state visible instead of hidden
- Design for graceful degradation, not just peak-demo performance

## Slide 6
**Title:** What This Project Shows

**Visual**
- Minimal closing slide with 3-4 proof points

**On-slide copy**
- Desktop product thinking, not just model integration
- Real-time systems design across audio, routing, and UI
- Strong local-first story with explicit hybrid controls
- A portfolio piece that combines UX, reliability, and AI infrastructure choices

## Export Notes
- Keep each slide under 35 words when possible.
- Use one accent color across all slides.
- Prefer full-bleed visuals over dense bullet lists.
- If you later add live screenshots, replace the composite visuals before exporting a public PDF.
