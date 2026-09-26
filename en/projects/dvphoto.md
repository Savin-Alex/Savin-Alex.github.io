---
title: DVPhoto.me — trust-first computer vision
---

[RU](../../projects/dvphoto.html) · [EN](./dvphoto.html)

A trust-first validation service for Diversity Visa photos: 32 checks (28 automatic technical, biometric, and provenance checks plus 4 manual-review reminders), explainable outcomes, and a deterministic fix that only crops, downscales, and re-encodes.

The key product decision is to **never present probabilistic computer-vision output as a guarantee**. This case shows what that rule costs in practice: a measurement-first audit found that the product was shipping defective files, and the product changed to match the evidence.

<p><b>Live service:</b> <a href="https://dvphoto.me" target="_blank" rel="noopener">dvphoto.me</a> · <a href="/demos/dvphoto-case.html">One-page summary →</a></p>

## Project passport

| Field | Content |
|---|---|
| **Problem** | Applicants may discover a bad photo too late, while many online checkers conceal algorithmic limits and create false confidence. |
| **Goal** | Provide understandable pre-validation and safely correct only framing, size, and JPEG compression; direct everything else to a retake or manual review. |
| **In scope** | Upload and validation; 32 checks; face landmarks and background segmentation; deterministic auto-fix with an output gate; evaluation harness on labeled public datasets; 11 UI languages; EN/RU guides; deployment. |
| **Out of scope** | A U.S. Department of State acceptance guarantee; background or appearance edits; padding or upscaling; forensic claims about manipulation; automatic validation of every requirement. |

## My role

**Role:** IT Project Manager · hands-on delivery owner.

- Defined the product boundary and an honest result taxonomy: `pass`, `warning`, `fail`, and `skipped/manual review`.
- Set the evaluation discipline: measure a threshold before changing it, fit on training sets, run a frozen holdout once, and treat any gap between site copy and code as a defect.
- Ran a full-cycle audit on 763 photos and 1,152 injected defects, then turned its findings into product decisions instead of footnotes.
- Owned the fix policy, the output gate, the monetization decision, and the deployment.

## Solution design

1. **Technical rules** check JPEG format, 600×600 px dimensions, file size, color model, compression, exposure, and an EXIF capture date when present.
2. **MediaPipe Tasks + OpenCV** detect a face and landmarks, then estimate head and eye geometry, background, sharpness, lighting, gaze, expression, and roll.
3. **Provenance checks** read metadata only. They produce heuristic signals and are explicitly not forensic guarantees.
4. A **decision layer** distinguishes an already-valid photo, a deterministically fixable case, and a required retake.
5. The **fix engine** only crops a bounded square from the original, downscales, and re-encodes. An **output gate** re-validates the file: any critical `fail`, `warning`, or `skipped` result blocks delivery.

## Evidence: what the audit measured

| Signal | Value |
|---|---|
| Full-cycle run | 763 photos from 5 public datasets · **0** crashes · 761 (99.7%) pass all framing checks |
| Injected defects | 1,152 mutations of 18 types · 16 types always caught · 12 files (1.0%) slipped through, all from one known limit (an out-of-focus object has no edge) |
| Output gate | 128 status combinations (32 checks × 4 statuses): every `fail` and every critical `warning`/`skipped` blocks the fixed file |
| Fix geometry | 10,000 deterministic geometries: every crop stays inside the original and never goes below 600 px |
| Latency | p95 184 ms per original upload on one CPU core |
| Automated suite | 633 passed, 1 failed (a stale SEO test), 1 skipped |

These are measurements on public research datasets and synthetic defects. They are not production accuracy on real applicant uploads, and they are not an acceptance guarantee.

## Decisions the evidence forced

| Finding | Decision |
|---|---|
| The fix padded tight crops with one flat color. Of 10 padded files delivered, **4 had a visible seam** — a photo pasted on a canvas. | The fix no longer pads or upscales. If a crop cannot reach 600 px, the result is "retake", not a file. |
| The evaluation harness always padded, while the product did not. Accuracy measured on 4 of 5 datasets described the harness, not the product. | Background accuracy is judged only on sets where the harness matches the product; the harness must mirror the product. The trap is written into the audit prompt. |
| A frozen holdout showed Background precision of **24%**. | Recalibrated on training data: **51.9%** precision on the holdout, with recall falling from 76.7% to 46.7%. The trade-off is recorded, not hidden. |
| Face Lighting has **6.4%** precision; Color Channel Balance warns on most photos of one studio set. | Both stay warning-only and never block a result. |
| Red-Eye and Headgear returned "not auto-checked" on 763 of 763 photos; the AI-edit check fires only when metadata survives. | Copy must match code: check names that promise more than the detector does are flagged for renaming or removal (open item). |

The paid flow (Robokassa, a credit ledger, refund handling) was built and then switched off on 22 September 2026. Checks and fixes are now free, with optional tips.

## Risk register

| Risk | Control / next gate |
|---|---|
| False pass on a non-compliant photo | Critical failures and critical warnings block the file; measure false-pass rate on real rejected photos, not only synthetic defects |
| False fail from a heuristic | Low-precision checks stay at warning; calibrate across skin tones, ages, and lighting conditions |
| A user reads the score as acceptance probability | Label it a quality score; show a disclaimer and official-guidance links |
| Biometric data and uploaded images | Analyze a bounded copy; keep evaluation photos out of source control; hosting region is named on the site |
| A check name promises more than the detector does | Rename or remove checks that never return a verdict; keep the site-versus-code audit in the release checklist |

## Status and next gate

**Status: Amber / deployed MVP.** The service runs at [dvphoto.me](https://dvphoto.me), in Docker on a VPS in Poland. The audit evidence is strong, but one high-priority finding is open: a critical `warning` can still receive `fixability=valid`, which gives the user an inconsistent instruction (no unsafe file is delivered).

Next gate: close that finding, fix the stale test, then measure false-pass rate on a labeled set of real accepted and rejected uploads before any accuracy claim.

## Stack

Python, FastAPI, MediaPipe Tasks, OpenCV, Pillow, PostgreSQL, Docker, Coolify, pytest
