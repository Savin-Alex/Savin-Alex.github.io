---
title: RemovePII — personal privacy gateway
---

[RU](../../projects/removepii.html) · [EN](./removepii.html)

**2026 · Personal project · Pre-beta**

RemovePII is a web application for individuals that masks personal data in text and documents before they go to ChatGPT, Claude, Gemini or another AI service. When the answer comes back, the app restores the original values and erases the mapping.

RemovePII runs on the [Local PII Anonymizer](./local-pii-anonymizer.html) engine; the [PII Firewall](./pii-firewall.html) Chrome extension solves the same problem directly in the browser with its own engine.

<p><b>Service:</b> <a href="https://removepii.app" target="_blank" rel="noopener">RemovePII.app</a></p>

## The challenge

People send contracts, medical letters and API keys to AI tools. Browser filters cannot handle documents, and server-side tools usually keep the text. People need a way to protect text and files before sharing them, without the source text being stored anywhere.

## My contribution

**IT Project Manager, solo product: specification, architecture, development and security review.**

- Wrote the specification and privacy rules before development: one person per account, no stored source text, review before sharing, no hidden data transfer.
- Reused the tested Local PII Anonymizer engine instead of rewriting security-sensitive code.
- Ran a full code audit and fixed confirmed issues test-first: first a test that reproduces the problem, then the fix.

## What I built

<ol class="project-flow">
  <li><strong>Text or document</strong><span>The user pastes text or uploads a DOCX, text PDF, CSV or JSON file.</span></li>
  <li><strong>Review</strong><span>The service finds personal data and 18 credential types, such as API keys. The user sees every detection and every gap before sharing.</span></li>
  <li><strong>Protected version</strong><span>Values are replaced with placeholders, and the file comes back in the same format: DOCX as DOCX, CSV as CSV.</span></li>
  <li><strong>Restored answer</strong><span>When the AI service answers, the app restores the original values and erases the mapping.</span></li>
</ol>

## The result

**The web app in Russian and English and the personal API cover the full path from document upload to the restored answer; all testing used synthetic data only.**

- Source and restored text are never stored; mappings are encrypted, expire and can be erased on request.
- Every document, mapping and token belongs to one account, and other accounts have no access to it.
- A check on the running service confirmed that server logs contain no document content.
- Status: pre-beta; public mode is blocked by a startup check. Hosting for RemovePII.app is being set up, and the site may not answer yet.

## Next steps

The next stage is a closed beta. Before it, I need to remove the risk of an incomplete mapping during parallel requests, pass the acceptance and release checks and build user sign-in.

<details markdown="1"><summary>Details: decisions and risks</summary>

- Reversible masking is not called “anonymization”: with the mapping and the key, the text can be restored. So mappings are kept short-lived.
- Detection is not perfect, and the product says so.
- Images and scanned PDFs are not inspected; downloading a file with images requires an explicit acknowledgement.

</details>

**Technologies:** Python, FastAPI, PostgreSQL, Redis, Presidio, spaCy, Next.js, TypeScript, OpenAPI and Docker.
