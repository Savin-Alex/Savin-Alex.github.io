---
title: PII Firewall — zero-network browser extension
---

[RU](../../projects/pii-firewall.html) · [EN](./pii-firewall.html)

**2026 · Personal project · Release candidate v0.5.0**

PII Firewall is a Chrome extension for AI chat users (ChatGPT, Claude, Gemini, Perplexity, Poe). It masks personal data in the browser before a prompt is sent and restores it in the answer locally.

It is part of a family of three products: the offline [Local PII Anonymizer](./local-pii-anonymizer.html) engine and the [RemovePII](./removepii.html) web app on that engine solve the same problem, while PII Firewall has its own engine and no network access.

## The challenge

People paste contracts, documents and bank details into AI chats. Cloud “PII filters” ask users to trust one more server with the same data. Users need protection that works before the data leaves the page, and a privacy claim they can check themselves.

## My contribution

**IT Project Manager, solo product: problem framing, architecture, development and release preparation.**

- Chose zero network access as the core promise and limited the extension's permissions to `storage`, `activeTab` and `contextMenus`.
- Built a dependency-free detection engine in TypeScript.
- Defined confidence levels per data type: a bare number without context is not masked with the same certainty as an ID with a valid checksum.
- Prepared the release under the Chrome Web Store User Data and Limited Use policies.

## What I built

<ol class="project-flow">
  <li><strong>Send interception</strong><span>Intercepts Enter and the send button and offers: mask and send, send as is, or cancel.</span></li>
  <li><strong>Data detection</strong><span>Russian, US, UK, Chinese and UAE identifiers, bank cards, IBAN, passport MRZ, secrets and person names in Russian case forms. ID numbers are checked against their checksums.</span></li>
  <li><strong>Masking</strong><span>Values are replaced with placeholders such as <code>[PERSON_1]</code> or <code>[RU_INN_1]</code> before sending to the chat.</span></li>
  <li><strong>Local restore</strong><span>The original values are restored in the answer on the user's machine.</span></li>
</ol>

## The result

**The extension recognizes 36 data types with zero network permissions; a v0.5.0 package is built for the Chrome Web Store.**

- Without network permissions, the extension cannot send data anywhere; anyone can check this in `manifest.json`.
- Detection is checked on a synthetic corpus with “traps”: similar values that must not be masked. No real personal data was used.
- Status: release candidate. The package and the store checklist are ready. The new UK, MRZ, China and UAE detectors are not released yet.

<figure class="demo-figure">
<iframe class="demo-frame demo-firewall" src="{{ '/demos/pii-firewall.html' | relative_url }}" title="Interactive demo: PII Firewall" loading="lazy"></iframe>
<figcaption>Animation: type a prompt with personal data → mask it in the browser → send the masked text → restore the answer locally. All data is synthetic; the demo UI is in Russian. <a href="{{ '/demos/pii-firewall.html' | relative_url }}" target="_blank" rel="noopener">Open full screen ↗</a></figcaption>
</figure>

## Next steps

The next step is publishing to the Chrome Web Store and collecting user reports of false masking and missed data. A future paid team mode is designed as a separate self-hosted build, so the free version keeps zero network access.

<details markdown="1"><summary>Details: decisions and risks</summary>

- Name detection is heuristic, so some names may be missed; addresses are not detected.
- The extension is not a certified information-security tool; the README and store listing say so.

</details>

**Technologies:** TypeScript, Chrome Extension Manifest V3, Vite and Web Crypto (AES-GCM).
