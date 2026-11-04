---
layout: page
title: "TripGPT — a travel plan from one request"
lang: en
---

[RU](../../projects/tripgpt.html) · [EN](./tripgpt.html)

**September–October 2026 · Ostrovok Hackathon · Web and Telegram demo**

TripGPT helps travelers plan a trip in one conversation: it finds transport and hotels, builds a day-by-day itinerary and calculates the budget. Users can compare options, change dates or preferences and receive an updated plan.

## The challenge

Planning a trip usually means searching for tickets, choosing accommodation, checking travel times and calculating expenses in separate places. Changing one decision often means revisiting the others. TripGPT brings these steps into one service that respects the traveler's preferences and keeps the trip consistent.

## My contribution

**I owned the product and implementation end to end, from the idea and user workflows through development and validation.**

- Defined the journey from the first request to a complete travel plan.
- Built the website and Telegram bot with shared planning and revision features.
- Added budget and schedule checks to keep the selected options consistent.
- Organized user-scenario testing and prepared the demonstration release.

## What I built

<ol class="project-flow">
  <li><strong>Travel preferences</strong><span>The traveler describes destinations, dates, budget and interests in everyday language.</span></li>
  <li><strong>Suggested options</strong><span>The service finds transport, hotels and places to visit.</span></li>
  <li><strong>A complete plan</strong><span>The itinerary is organized by day, with automatic budget and travel-time checks.</span></li>
  <li><strong>Flexible revisions</strong><span>The traveler can compare alternatives, change preferences and export the plan to a calendar.</span></li>
</ol>

## The result

**A working travel-planning demo on the web and in Telegram, covering the journey from a request to a daily itinerary, budget and simulated checkout.**

- Transport, accommodation and activities appear in one plan.
- Ticket and hotel costs are shown separately from estimated spending on food, local transport and activities.
- Travelers can revise the trip in conversation while keeping their existing choices.
- The interface makes clear which prices and offers come from sources and which are estimates or examples.

![TripGPT travel plan and budget]({{ '/docs/images/tripgpt-budget.png' | relative_url }})

*Demonstration screen. Checkout is simulated; the displayed prices belong to the example trip.*

## Next steps

The next stage is to connect real booking and payment, expand the sources of offers and verify the service on mobile devices. The current version demonstrates the travel-planning workflow.

**Technologies:** Python, FastAPI, React, TypeScript, SQLite, Docker and language models.
