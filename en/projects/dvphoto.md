---
title: DVPhoto.me — trust-first computer vision
---

[RU](../../projects/dvphoto.html) · [EN](./dvphoto.html)

**October 2025 – present · Own web service · Deployed MVP**

DVPhoto.me is a web service for U.S. Diversity Visa lottery applicants. It checks the photo against the official requirements, explains the result and, when needed, fixes framing, size and file compression.

<p><b>Live service:</b> <a href="https://dvphoto.me" target="_blank" rel="noopener">dvphoto.me</a> · <a href="/demos/dvphoto-case.html">One-page summary →</a></p>

## The challenge

Applicants often learn too late that their photo does not meet the requirements. Many online checkers hide the limits of their algorithms and create false confidence. Applicants need a clear pre-check that is honest about what it can and cannot verify.

## My contribution

**IT Project Manager and hands-on delivery owner, from the product boundary to the launch of the service.**

- Set the core product rule: never present a probabilistic computer-vision result as a guarantee. So besides pass and fail, a result can also be a warning or a request for manual review.
- Set the evaluation rules: measure before changing anything, and treat any gap between site copy and code behavior as a defect. Ran a full-cycle audit and changed the product based on its findings.
- Owned the fix rules, the monetization decision and the launch of the service.

## What I built

<ol class="project-flow">
  <li><strong>Technical requirements</strong><span>The service checks JPEG format, 600×600 px dimensions, file size, color model, compression and exposure.</span></li>
  <li><strong>Face and background</strong><span>MediaPipe and OpenCV detect the face and assess head and eye position, background, sharpness, lighting and gaze.</span></li>
  <li><strong>Clear results</strong><span>32 checks: 28 automatic checks and 4 manual-review reminders. Each result shows its reason; the interface is available in 11 languages.</span></li>
  <li><strong>Safe fix</strong><span>The service only crops, downscales and re-encodes the photo, then checks it again. If a critical problem remains, no file is delivered.</span></li>
</ol>

## The result

**The service is live at dvphoto.me: applicants check their photo for free and receive a fixed file when the only problems are framing, size or compression.**

- A full-cycle audit on 763 photos from 5 public datasets ran with no crashes; 761 photos (99.7%) pass all framing checks.
- Of 18 injected defect types (1,152 defects in total), the service always catches 16.
- In 95% of cases, one upload is processed in 184 ms or less on one CPU core.
- These are measurements on public datasets and synthetic defects, not accuracy on real applications or a guarantee of acceptance.

## Next steps

The next step is to measure the false-pass rate on real accepted and rejected photos, and only then make claims about accuracy. A contradictory instruction in one case also needs a fix; no unsafe file is delivered in that case.

<details markdown="1"><summary>Details: decisions and risks</summary>

- The fix used to pad tight crops. 4 of 10 padded files showed a visible seam, so padding and upscaling were removed: if a crop is below 600 px, the service asks for a retake.
- Low-precision checks, such as face lighting, only warn and never block a result.
- A paid flow with Robokassa was built and then switched off on 22 September 2026: checks and fixes are now free.

</details>

**Technologies:** Python, FastAPI, MediaPipe Tasks, OpenCV, Pillow, PostgreSQL, Docker and Coolify.
