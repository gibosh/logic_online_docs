---
page-id: forecast-confidence
route: /module/1/project/:projectId
title: Forecast Confidence
audience: external
status: draft
version: 1.4.0
last-reviewed: 2026-09-04
blocked-reason: Content verified directly against ForecastConfidenceModule source and its call path from the live route, but exact on-screen wording (labels, tooltips) still needs a live-app screenshot before promoting to complete. Worked examples added from representative inputs run through the real formula, not from a live traceback.
---

## Forecast Confidence – Overview

When a project reports a finish date, there is often no way to tell at a glance whether that date is genuinely achievable or quietly optimistic. Some schedules lose roughly a month of completion date for every month that passes, while still looking healthy on paper – the delay is hiding in work that hasn't started yet. Others show what appears to be a recovery, but the recovery was manufactured by cutting planned durations and adding logic links, not by real progress on site.

Forecast Confidence gives you an honest trust read on the reported finish date – deliberately a plain green, amber, or red signal, not a false-precision score out of 100. It is built from the schedule's own update history: is the finish date stable, how consistently has it moved, and how much float has the programme burned through.

A planner or project manager can use this to answer the question "should I believe the finish date?" in seconds – and if the answer is amber or red, the detail behind the signal shows exactly why.

## Report details

A confidence gauge showing a percentage and a green, amber, or red read, with a one-line explanation underneath.

Below the gauge, a timeline shows the **data date of each uploaded update**, the **reported finish date**, a **forecasted (rules-adjusted) finish date**, and an **early/late likely range** either side of it. There is no separate "today" marker – the most recent point plotted is the latest update's own data date, not necessarily today's calendar date. The confidence percentage is a measure of how wide that likely range is relative to the time remaining – a tight range against a long remaining duration reads as high confidence; a wide range against a short remaining duration reads as low confidence.

Below the timeline, a **Reliability Factors** grid lists the individual metrics the model is built from, each with its own small sparkline (the last 13 updates) and a green/amber/watch status word:

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

## Calculation and other logic

**Data used:** every schedule update file uploaded for the project, plus the activity identified as the project completion milestone – auto-detected by name (activities named along the lines of "Practical Completion" are prioritised, then "Final/Contract/All of the Works/Project Completion," then a generic "Completion").

**How it's calculated:** update files that don't share enough activity codes with the largest uploaded file (fewer than half in common) are treated as a different schedule's data and excluded from the read. The remaining files are lined up in date order to track how far the completion milestone's forecast date has moved and how fast. That trend is combined with two other signals into the confidence percentage: how consistent the movement has been update to update, and how much float has gone negative across the project. The confidence band is **60% or above is green, 40–59% is amber, below 40% is red.**

The Forecast honesty panel's planned-vs-actual comparison is **not** one of these three inputs – it's computed and shown separately, and is capped at a moderate read until at least 20 activities have finished across at least 4 updates, since fewer than that isn't enough to judge reliably. Don't read the honesty panel as explaining the confidence percentage; the two can genuinely disagree.

## Worked examples: what drives a high, medium, or low confidence

The Reliability Factors don't add up to the confidence percentage – they explain it. Each one widens or narrows the likely-range window described above; the confidence percentage then falls out of comparing that window's width against how much time is left. Two schedules with very different Reliability Factors can land on a similar confidence percentage, and that's expected.

The three examples below use the same illustrative project throughout – six monthly updates so far, six months of programme left, reported finish 15 December – with only the schedule's underlying health changing between them. They're representative inputs, not pulled from a real run, but the confidence percentages they produce are worked through the actual formula.

**High confidence – around 65–70%**

The Reliability Factors all score healthy: task-count growth low and steady, step-to-step volatility low (the forecast finish barely moves update to update), negative-float share low, and the slip-rate factors all close to flat – the schedule is tracking close to plan.

What you'd see: the forecasted finish sits only a few days past the reported one (18 December vs. 15 December), and the window stretches from the reported date itself out to around 25 April – wide in absolute terms, but narrow relative to the six months still on the clock. That's the shape behind high confidence in Logic+: not a pinpoint date, but a window that isn't ballooning outward.

**Medium confidence – around 40–55%**

The Reliability Factors are mixed: moderate task-count growth, moderate step-to-step volatility, negative-float share creeping up, and a slip rate that's clearly non-zero but not extreme – a combination that comes across as worth watching, rather than healthy or in trouble.

What you'd see: the forecasted finish moves out to around 11 January, and the window widens out to mid-July – more than a month of central shift, and a window wider than the time remaining. This is the zone where the reported date needs real scrutiny before it's repeated to a client.

**Low confidence – below 40%**

Several Reliability Factors are in trouble at once: high task-count growth, high step-to-step volatility, a high negative-float share, and a slip rate that's losing a meaningful fraction of a month for every month that passes.

What you'd see: the forecasted finish moves to late February – more than two months past what's reported – and the window stretches out to early December the *following* year, nearly doubling the whole remaining duration. At this point the reported date isn't a useful anchor on its own; the width of the window is the real message.

## Note

At least two schedule updates sharing a common completion milestone are needed to produce a read. With fewer, no signal is shown.
