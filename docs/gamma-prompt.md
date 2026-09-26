# Gamma Prompt

Paste this into Gamma to generate a short self-guided portfolio deck for `AI Consul`.

## Prompt

Create a 6-slide portfolio presentation for a private product project called **AI Consul**.

The audience is a recruiter, hiring manager, or someone skimming a GitHub portfolio. Keep it visual, concise, and product-focused rather than deeply technical.

Use a clean modern style with a dark or neutral professional theme. Keep each slide light on text and let visuals do most of the work.

### Project framing

AI Consul is a **privacy-first, real-time AI assistant** for conversations, interviews, meetings, and educational sessions.

It captures live audio, routes transcription across local and hybrid backends, and surfaces concise assistant suggestions in separate desktop windows.

Key positioning:
- privacy-oriented
- local-first, with explicit hybrid/cloud controls
- latency-aware
- resilient through fallback routing
- multi-window desktop UX

Avoid exaggerated claims like:
- 100% local in every mode
- universal sub-500ms latency
- fully offline in all scenarios

Instead use careful phrasing like:
- local-first with optional hybrid/cloud modes
- streaming path targets low latency
- graceful fallback when ideal backends are unavailable

### Slide structure

#### Slide 1: Title
Title: **AI Consul**
Subtitle: **Privacy-first AI assistance for conversations, interviews, and meetings**

Use a strong product-style title slide with a hero visual placeholder.

#### Slide 2: The problem
Title: **Why this matters**

Message:
- Most conversation assistants hide privacy trade-offs behind cloud defaults.
- Real-time tools become fragile when the ideal backend is unavailable.
- Users need visibility, control, and a product that degrades gracefully.

Use a simple visual layout with strong typography.

#### Slide 3: The product experience
Title: **What the product does**

Message:
- Main window to configure and run a session
- Transcript window to monitor what the system hears
- Companion overlay to surface suggestions during the conversation
- Settings that expose privacy, audio source, and fallback behavior

Use a visual-heavy layout with placeholders for 3-4 screenshots.

#### Slide 4: Under the hood
Title: **How it works**

Message:
- Electron renderer handles windows and audio capture
- Main process manages sessions and orchestration
- Transcription routing chooses the best available path
- Suggestions are generated from transcript plus context

Use a simplified architecture diagram placeholder.

#### Slide 5: Reliability story
Title: **Why the pipeline matters**

Message:
- Audio capture feeds a routing layer
- Streaming is attempted first when healthy
- Batch fallback preserves usefulness when live routes fail
- Cost, network, and engine state remain visible

Use a flow diagram placeholder with a product-engineering feel.

#### Slide 6: What this shows
Title: **Why this is a strong portfolio project**

Message:
- Desktop product thinking, not just model integration
- Real-time systems design across audio, routing, and UX
- Local-first controls as a product decision
- Reliability and observability built into the experience

End with a clean closing slide and optional GitHub link placeholder.

### Output style
- Minimal copy
- Strong headings
- Clean spacing
- Modern portfolio feel
- No code blocks
- No long paragraphs

### Visual placeholders to include
- Hero screenshot
- Main session screen
- Settings panel
- Transcript window
- Architecture diagram
- Pipeline / fallback diagram

### Notes
This is a **private codebase**, so the deck should present the product and architecture clearly without exposing implementation details or internal review notes.
