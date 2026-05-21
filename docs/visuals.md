# Visual Plan

This file defines the strongest public-facing visuals for a self-guided portfolio page.

## Best Four Visuals

### 1. Main Window
- Filename: `docs/images/session-home.png`
- Route/state: `/` after onboarding or on the primary session screen
- Why it works: it immediately shows that this is a desktop product with a clear job to do
- What to capture:
  - session mode selector
  - primary action button
  - short how-to instructions
  - clean app framing without devtools
- Suggested caption: `Main control surface for live AI-assisted sessions`
- Evidence path: `/Users/alexander/Documents/CriticalSuccess/Ai Consul/.claude/worktrees/stupefied-booth/src/renderer/components/MainWindow/MainWindow.tsx`

### 2. Settings Panel
- Filename: `docs/images/settings-panel.png`
- Route/state: `/` with the settings panel expanded
- Why it works: it makes the local-first and hybrid-control story visible in one frame
- What to capture:
  - transcription mode selector
  - audio source selector
  - performance tier
  - cost limit
  - engine status if visible
- Suggested caption: `Explicit control over privacy, audio source, and fallback behavior`
- Evidence path: `/Users/alexander/Documents/CriticalSuccess/Ai Consul/.claude/worktrees/stupefied-booth/src/renderer/components/Settings/Settings.tsx`

### 3. Transcript Window
- Filename: `docs/images/transcript-window.png`
- Route/state: `/transcript`
- Why it works: it makes the real-time transcription feature instantly understandable
- What to capture:
  - `Live Transcript` header
  - placeholder or active transcript rows
  - speaker labels or interim line if available
- Suggested caption: `Separate transcript monitor for real-time conversation tracking`
- Evidence path: `/Users/alexander/Documents/CriticalSuccess/Ai Consul/.claude/worktrees/stupefied-booth/src/renderer/components/TranscriptionWindow/TranscriptionWindow.tsx`

### 4. Companion Overlay
- Filename: `docs/images/companion-overlay.png`
- Route/state: `/companion` during an active session with live suggestions
- Why it works: this is the most distinctive product moment when populated
- What to capture:
  - 2-3 concise suggestion cards
  - transparent or always-on-top feel
  - enough surrounding desktop context to show its overlay behavior
- Suggested caption: `Always-on-top companion overlay for in-the-moment assistance`
- Evidence path: `/Users/alexander/Documents/CriticalSuccess/Ai Consul/.claude/worktrees/stupefied-booth/src/renderer/components/CompanionWindow/CompanionWindow.tsx`

## Current Asset Strategy

Three browser-rendered captures are already included in this repository:

- `docs/images/onboarding-privacy.png`
- `docs/images/session-home.png`
- `docs/images/settings-panel.png`
- `docs/images/transcript-window.png`

The companion overlay is still the most compelling live-demo visual, but it is only strong when suggestion data is present. Until an active session is captured, use the composite visual in `docs/images/ui-overview.svg` to explain the window set.

## Capture Notes
- Prefer a packaged build or a browser-rendered route without devtools visible.
- Use window-only captures rather than full-screen captures.
- Keep one accent state consistent across the set so the visuals feel like one product.
- Capture both a `PNG` for GitHub and a `2x PNG` or `SVG` alternative for slides when possible.

## Visual Order In README
1. `docs/images/hero-overview.svg`
2. `docs/images/ui-overview.svg`
3. `docs/images/session-home.png`
4. `docs/images/settings-panel.png`
5. `docs/images/transcript-window.png`
6. `docs/images/architecture.svg`
7. `docs/images/pipeline.svg`

## Optional Demo Clip
- Duration: `30-45s`
- Sequence:
  1. start on main window
  2. show settings and local-first controls
  3. switch to transcript window
  4. end with populated companion overlay
