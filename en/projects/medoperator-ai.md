---
layout: page
title: "Voice assistant for clinic appointments"
lang: en
---

[RU](../../projects/medoperator-ai.html) · [EN](./medoperator-ai.html)

**October 2026 · “Точка сборки” hackathon, DBI case · Demonstration project**

The voice assistant handles telephone enquiries and helps patients book, reschedule or cancel appointments. It also supports outbound reminders. Clinic service procedures define the conversation flow, action confirmations and when to refer an enquiry to a staff member.

## The challenge

Reduce repetitive work for reception staff and let patients resolve routine requests by phone. The assistant needs to follow service procedures, collect the required information and obtain consent before changing an appointment.

## My contribution

**I owned the product and implementation end to end, from interpreting the clinic's procedures to building the voice service and validating its workflows.**

- Turned service requirements into appointment workflows and rules for referring enquiries to staff.
- Connected telephony, speech recognition, spoken responses and the appointment system.
- Implemented action confirmation: the assistant reads back the appointment details and waits for explicit consent.
- Organized scenario testing, call-outcome tracking and a staged pilot plan.

## What I built

<ol class="project-flow">
  <li><strong>Appointment booking</strong><span>The assistant clarifies the request, helps select an available appointment and confirms the booking.</span></li>
  <li><strong>Rescheduling</strong><span>It helps choose a new time and confirms the change with the patient.</span></li>
  <li><strong>Cancellation</strong><span>It identifies the appointment to cancel and asks for confirmation.</span></li>
  <li><strong>Appointment reminders</strong><span>It calls the patient and reads out the details of an upcoming visit.</span></li>
</ol>

## The result

**Implemented the booking, rescheduling, cancellation and reminder workflows specified in the clinic's procedures. Delivered a working demo using real phone calls and a connected clinic test system.**

- 13 of 20 typical demo calls were completed without a human — 65% against a 60% target. All patients in the demo were fictional.
- Conversations follow the defined service rules and approved wording.
- Appointment changes require the assistant to read back the details and receive explicit consent.
- Rules define when an enquiry outside the standard workflow should be referred to staff.
- A call-outcome dashboard supports evaluation and planning further improvements.

## Next steps

The demo uses the organizer's system and fictional patients. The next stage is integration with a clinic's information system, support for simultaneous calls and further work on exceptional situations before a pilot.

**Technologies:** Python, Asterisk, GigaAM, Silero, GigaChat and Docker.
