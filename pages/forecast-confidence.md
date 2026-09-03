---
page-id: forecast-confidence
route: /module/1/project/:projectId
title: Forecast Confidence
audience: external
status: draft
version: 1.2.0
last-reviewed: 2026-09-03
blocked-reason: Content verified directly against ForecastConfidenceModule source and its call path from the live route, but exact on-screen wording (labels, tooltips) still needs a live-app screenshot before promoting to complete.
---

## Forecast Confidence – Overview

When a project reports a finish date, there is often no way to tell at a glance whether that date is genuinely achievable or quietly optimistic. Some schedules lose roughly a month of completion date for every month that passes, while still looking healthy on paper – the delay is hiding in work that hasn't started yet. Others show what appears to be a recovery, but the recovery was manufactured by cutting planned durations and adding logic links, not by real progress on site.

Forecast Confidence gives you an honest trust read on the reported finish date – deliberately a plain green, amber, or red signal, not a false-precision score out of 100. It is built from the schedule's own update history: is the finish date stable, how consistently has it moved, and how much float has the programme burned through.

A planner or project manager can use this to answer the question "should I believe the finish date?" in seconds – and if the answer is amber or red, the detail behind the signal shows exactly why.

## Report details

A confidence gauge showing a percentage and a green, amber, or red read, with a one-line explanation underneath.

Below the gauge, a timeline shows the **data date of each uploaded update**, the **reported finish date**, a **forecasted (rules-adjusted) finish date**, and an **early/late likely range** either side of it. There is no separate "today" marker – the most recent point plotted is the latest update's own data date, not necessarily today's calendar date. The confidence percentage is a measure of how wide that likely range is relative to the time remaining – a tight range against a long remaining duration reads as high confidence; a wide range against a short remaining duration reads as low confidence.

Below the timeline, a **Reliability ingredients** grid lists the individual metrics the model is built from, each with its own small sparkline history and a green/amber/watch status word:

| Metric | What it reflects |
|---|---|
| Task-count growth | How much the activity count has grown since the first uploaded update |
| Overall slip ratio | How far the forecast finish has moved, relative to time elapsed |
| Step-to-step volatility | How much the forecast finish jumps around from one update to the next, rather than drifting smoothly |
| Median float (latest) | The typical spare time left across incomplete activities in the most recent update |
| Negative-float share (latest) | The share of incomplete activities currently out of spare time |
| Peak negative-float share | The worst that negative-float share has been across the whole series, not just the latest update |
| Recent / normalised / central slip rate | Three variants of "how fast is the finish date moving out," measured over different windows, used together to settle on the rate the confidence model actually trusts |

A separate **Forecast honesty** panel sits alongside this grid, showing how well completed work has tracked the plan (comparing planned vs. actual finish dates for everything that's finished so far). This is shown for information – **it is not one of the inputs to the confidence percentage itself** (see Calculation, below); read it as a second opinion, not a component of the gauge.

A closing note is intended to identify the single biggest activity currently driving the forecast slip, and its working-day impact — **this is not yet wired up and currently always shows a "No driver data" placeholder regardless of the project**, a known engine gap rather than a data problem with your schedule.

## Calculation and other logic

**Data used:** every schedule update file uploaded for the project, plus the activity identified as the project completion milestone – auto-detected by name (activities named along the lines of "Practical Completion" are prioritised, then "Final/Contract/All of the Works/Project Completion," then a generic "Completion").

**How it's calculated:** update files that don't share enough activity codes with the largest uploaded file (fewer than half in common) are treated as a different schedule's data and excluded from the read. The remaining files are lined up in date order to track how far the completion milestone's forecast date has moved and how fast. That trend is combined with two other signals into the confidence percentage: how consistent the movement has been update to update, and how much float has gone negative across the project. The confidence band is **60% or above is green, 40–59% is amber, below 40% is red.**

The Forecast honesty panel's planned-vs-actual comparison is **not** one of these three inputs – it's computed and shown separately, and is capped at a moderate read until at least 20 activities have finished across at least 4 updates, since fewer than that isn't enough to judge reliably. Don't read the honesty panel as explaining the confidence percentage; the two can genuinely disagree.

## Note

Two mechanisms intended to make Forecast Confidence get smarter as more projects are run through Logic+ exist in the code but are **not currently functioning**: the shared calibration library that's meant to sharpen the model's thresholds only ever holds one project's data at a time (a bug, not a "needs more data yet" state), and a second, more sophisticated per-programme calibration system exists fully built but isn't connected to anything yet. Neither affects the correctness of a read for a single project today – both are about the model improving across projects over time, which currently isn't happening.

At least two schedule updates sharing a common completion milestone are needed to produce a read. With fewer, or if no completion milestone is common to every uploaded update, the module is intended to show no signal rather than a spurious one – this specific case has not yet been confirmed against the live app.
