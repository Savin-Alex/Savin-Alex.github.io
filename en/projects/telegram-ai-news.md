---
title: Telegram AI News — governed LLM content pipeline
---

[RU](../../projects/telegram-ai-news.html) · [EN](./telegram-ai-news.html)

**May 2026 – present · Own project · Live channel**

An LLM pipeline for «ИИнтересные новости», a Russian-language Telegram channel about AI. It reads news sources, selects the stories that matter and writes short posts for people who build products with AI.

<p><b>Live channel:</b> <a href="https://t.me/ainews24by7" target="_blank" rel="noopener">ИИнтересные новости — t.me/ainews24by7</a> · 1,600+ subscribers as of 14 November 2026</p>

## The challenge

AI news channels often repost press releases or publish everything. People who build products with AI need fewer posts, with sources, numbers and a skeptical comment. Each post should explain in 30 seconds what happened and why it matters.

## My contribution

**IT Project Manager on a solo product: problem framing, architecture, development and operations.**

- Wrote the reader promise and the editorial policies on corrections, sponsorship, privacy and source trust.
- Split the model's work into stages instead of one large prompt: fact extraction, story scoring and post writing.
- Set up cost control: a cheaper model selects stories and a stronger model writes only the selected posts. The cost of every model call is recorded, and new runs stop on billing errors.
- Kept approval-required publishing until the channel launched and ran a security audit before launch. Turned on automatic publishing later, and only with its own limits.

## What I built

<ol class="project-flow">
  <li><strong>News collection</strong><span>Every 15 minutes, the pipeline reads RSS sources and removes duplicates.</span></li>
  <li><strong>Selection</strong><span>The Haiku model extracts facts and scores each story. Code re-checks the score and can only lower the model's decision.</span></li>
  <li><strong>Post writing</strong><span>The Sonnet model writes posts only for the selected stories. Every number in a post links to its source.</span></li>
  <li><strong>Publishing</strong><span>Only posts from top-trust sources publish automatically, at most 10 per run. Other posts wait for a person in the operator console.</span></li>
</ol>

## The result

**The channel runs unattended: the pipeline selects news, writes posts and publishes them automatically.**

- 1,600+ subscribers as of 14 November 2026.
- The pipeline checks the enabled sources every 15 minutes. 56 sources are configured, including blogs of Russian AI companies; only top-trust sources are enabled today.
- The main hypothesis, that readers value the filter more than the volume, is not tested yet.

## Next steps

The next step is to test the main hypothesis: measure reader retention and reactions per post and compare them with the cost of one published post.

<details markdown="1"><summary>Details: decisions and risks</summary>

- The main risk is prompt injection: hidden instructions for the model inside a news item. It cannot be fully removed, so only top-trust sources are enabled, code can only make the decision stricter, and the per-run limit bounds the damage.
- The security audit closed vulnerabilities including SSRF, token leakage and link injection.
- Prompts live in text files, so editorial rules change without code changes.

</details>

**Technologies:** Python, FastAPI, Jinja, SQLite, Anthropic SDK (Haiku 4.5, Sonnet 5), Ollama, python-telegram-bot and APScheduler.
