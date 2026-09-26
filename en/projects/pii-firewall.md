---
title: PII Firewall — zero-network browser extension
---

[RU](../../projects/pii-firewall.html) · [EN](./pii-firewall.html)

A browser extension that detects and reversibly masks personal data **inside the browser**, before a prompt goes to an AI chat (ChatGPT, Claude, Gemini, Perplexity, Poe), and restores the original values in the answer on the user's machine.

The main product decision is **zero network**: the extension asks for no host permissions, so it cannot send data anywhere. Anyone can check this claim in `manifest.json`.

<figure class="demo-figure">
<iframe class="demo-frame demo-firewall" src="{{ '/demos/pii-firewall.html' | relative_url }}" title="Interactive demo: PII Firewall" loading="lazy"></iframe>
<figcaption>Animation: type a prompt with personal data → mask it in the browser → send the masked text → restore the answer locally. All data is synthetic; the demo UI is in Russian. <a href="{{ '/demos/pii-firewall.html' | relative_url }}" target="_blank" rel="noopener">Open full screen ↗</a></figcaption>
</figure>

## Project passport

| Field | Content |
|---|---|
| **Problem** | People paste contracts, IDs, and bank details into AI chats. Cloud "PII filters" ask users to trust one more server with the same data. |
| **Goal** | Mask personal data before it leaves the page, with a privacy claim a user can verify, and restore it locally in the answer. |
| **In scope** | A dependency-free TypeScript detection engine; 36 data types; reversible placeholders; a guard that intercepts sending; popup and settings; optional encrypted vault; RU/EN interface; Chrome Web Store readiness. |
| **Out of scope** | Certified information-security status; address detection; any server-side processing in the free build. |

## My role

**Role:** IT Project Manager · solo product; framing, architecture, development, and release preparation.

- Chose zero network as the core promise and kept the permission list to `storage`, `activeTab`, and `contextMenus`.
- Defined confidence levels per data type, so a bare number without context is not masked with the same certainty as a checksum-valid ID.
- Prepared the store release against the Chrome Web Store User Data and Limited Use policies.

## Solution design

1. **Normalization:** NFKC plus removal of combining marks, with an offset map back to the original text.
2. **Homoglyph folding:** Cyrillic look-alike letters fold to Latin for Latin patterns, so `еxample@mail` with a Cyrillic "е" is still caught.
3. **Candidates → validation:** regex candidates pass checksum, format, or context checks (INN, SNILS, OGRN, card Luhn, IBAN mod-97, MRZ check digits, China ID ISO 7064, Emirates ID).
4. **Overlap resolution:** the longest span wins; on a tie, checksum beats secret, secret beats structured, structured beats person.
5. **Masking and restore:** values become placeholders such as `[PERSON_1]` or `[RU_INN_1]`; the answer is restored locally.
6. **Guard mode:** intercepts Enter and the send button and offers "mask and send", "send as is", or "cancel".

## Verifiable signals

| Signal | Value |
|---|---|
| Network permissions | **0** host permissions (Manifest V3) |
| Data types | 36: RU, US, UK, China, and UAE identifiers, cards, IBAN, SWIFT, passport MRZ, IP addresses, secrets, and person names with Russian case forms |
| Automated tests | 117 tests (checksums, detectors, corpus, vault, settings, i18n, content script) |
| Synthetic corpus | 123 cases with positives and "trap" negatives; no real personal data |
| Release | v0.5.0 package built; store listing drafted in RU |

## Risk register

| Risk | Control / next gate |
|---|---|
| A name or ID is missed | Stated limits: name detection is heuristic; grow the corpus with every new detector and its negatives |
| A number is masked by mistake | Context-dependent confidence levels; noisy types such as date of birth are off by default |
| A user reads the extension as a certified security tool | The README and store listing say it is not certified |
| A future paid tier breaks the zero-network promise | The team mode is designed as opt-in and self-hosted, and it is documented as separate from the zero-network build |

## Status and next gate

**Status: Amber / release candidate.** The v0.5.0 package and the store readiness checklist are ready. New UK, MRZ, China, and UAE detectors are not released yet.

Next gate: publish to the Chrome Web Store, then measure false-mask and missed-entity reports from real users.

## Stack

TypeScript, Chrome Extension Manifest V3, Vite, Web Crypto (AES-GCM), Vitest
