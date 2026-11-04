---
title: Local PII Anonymizer
---

[RU](../../projects/local-pii-anonymizer.html) · [EN](./local-pii-anonymizer.html)

**2026 · Personal project · Technical MVP**

Local PII Anonymizer is an offline service that replaces personal and medical data in Russian and English documents with reversible pseudonyms. It is designed for legal, HR and clinical teams that prepare confidential documents for external language models.

It is the engine of a family of three products: the [RemovePII](./removepii.html) web app runs on it, and the [PII Firewall](./pii-firewall.html) Chrome extension solves the same problem in the browser with its own engine.

## The challenge

Off-the-shelf anonymization tools usually run in the cloud, so the data goes to someone else's server. Simple regex filters fail on mixed Russian-English documents and do not validate ID checksums. For industries covered by 152-FZ, GDPR and HIPAA, this is not enough.

## My contribution

**IT Project Manager, solo project: problem framing, architecture, development and quality.**

- Framed the problem for legal, HR and healthcare use and defined two modes: reversible pseudonymization and irreversible de-identification under HIPAA Safe Harbor.
- Designed multi-stage detection: checksums first, then Presidio and spaCy models, with a local language model when needed.
- Set up automated evaluation of detection quality on a synthetic corpus.
- Extracted the engine into a stand-alone package and built a versioned web API on it.

## What I built

<ol class="project-flow">
  <li><strong>Document input</strong><span>The service accepts plain text and TXT, Markdown, CSV, JSON, PDF and DOCX files.</span></li>
  <li><strong>Data detection</strong><span>More than 17 data types: full names, Russian passports, INN, SNILS, US SSN, UK NINO, AWS keys and more. ID numbers are checked against their checksums.</span></li>
  <li><strong>Pseudonymization</strong><span>Detected values are replaced with pseudonyms; the mappings are encrypted and kept separately for each client.</span></li>
  <li><strong>Restore</strong><span>The operator restores the original values or deletes the mappings. HIPAA Safe Harbor mode de-identifies dates and ages irreversibly.</span></li>
</ol>

## The result

**A working offline pseudonymization engine for Russian and English with measured detection quality and a versioned web API.**

- Macro F1 ≈ 0.9997 on the synthetic evaluation corpus. Quality on real documents has not been measured yet.
- The engine is a separate package; its `/api/v1` web API was accepted after an independent review.
- Original and restored text, file names, mappings and keys never reach the API's database, logs or error messages.
- Status: technical MVP. Work with real personal data requires security, privacy and legal review first.

<figure class="demo-figure">
<iframe class="demo-frame demo-pii" src="{{ '/demos/local-pii-anonymizer.html' | relative_url }}" title="Interactive demo: Local PII Anonymizer" loading="lazy"></iframe>
<figcaption style="font-size:.9em;color:#667085;margin-top:.4em;">Animation: data redaction, flow through the detection cascade, and the encrypted vault. <a href="{{ '/demos/local-pii-anonymizer.html' | relative_url }}" target="_blank" rel="noopener">Open full screen ↗</a></figcaption>
</figure>

## Next steps

The next stage is a web interface on top of the API, testing on Windows and preparation for production deployment. Before launch on real data, detection quality must be checked on documents close to real customer ones.

<details markdown="1"><summary>Details: decisions and risks</summary>

- Reversible replacement is called pseudonymization, not anonymization, and the limits of Safe Harbor mode are stated explicitly.
- [Project status report (sample)](/evidence/pii-status-report.html)

</details>

**Technologies:** Python, FastAPI, PostgreSQL, Redis, OpenAPI, Streamlit, Presidio, spaCy, HuggingFace Transformers, Ollama and Docker.
