---
page-id: completion-forecast
route: /module/7/project/:projectId
title: Completion Forecast
audience: external
status: draft
version: 1.0.0
last-reviewed: 2026-07-20
---

## Completion Forecast – Overview

The reported finish date on a project and a rigorous, data-driven projection based on the schedule's own track record often disagree – sometimes by many months. Showing only the reported date hides that gap.

Completion Forecast puts both figures side by side. You see what has been reported as the finish date over time, what the data-driven adjustment says instead, and how large the gap between them is. So when someone reports a project as "on track," you can check that claim against the schedule's own history – not just take it at face value.

The forecast is produced from the schedule's update history using a rules-adjusted projection, not a Monte Carlo simulation. The same inputs always produce the same output – there is no random element. This makes the result defensible in a meeting: it is a reasoned, documented projection, not a black-box estimate.

## Report details

A **reliability index** and a plain-language verdict on how far to trust the projection.

The **reported finish date**, a **rules-adjusted finish date**, and a **likely range** around it, alongside a **forecast confidence** percentage – the same style of confidence read as the [Forecast Confidence](forecast-confidence.md) module, calculated for this module's own longer-range projection.

A chart plotting the reported finish date over each update, against the rules-adjusted estimate and its confidence band, so you can see how the two lines have tracked (or diverged) over time.

**What's driving the programme** – the activities currently on the driving (critical) path, grouped into categories such as construction, systems and testing, design and approvals, procurement, and handover, with the categories carrying the most driving activities highlighted.

A **data quality** note showing which completion milestone the projection is anchored to, and which uploaded schedules (if any) were excluded from the analysis and why.

## Calculation and other logic

**Data used:** every schedule update file uploaded for the project, tracked over time – the same underlying trend-analysis approach as [Forecast Confidence](forecast-confidence.md), applied here to produce a projected finish date and range rather than a standalone confidence score.

**How it's calculated:** update files that don't share enough activity codes with the largest uploaded file are excluded as a different schedule's data. The remaining files are lined up in date order to measure how the completion milestone's forecast date has moved, how consistently, how much float has gone negative, and how well completed work has tracked the plan. The projection and its confidence range are entirely rules-based arithmetic on this history – there is no simulation or random element, so identical inputs always produce the identical output.

## How the projection works

The rate used for the rules-adjusted finish is the **worst sustained rate** seen across the schedule's update history – not an average, and not just the most recent trend. This is deliberate: an average can be flattered by a short-lived, unsustained recovery. The projection also checks how well past reported progress has matched actual completed work – a schedule with a track record of paper recoveries produces a wider, less confident range than one with a track record of honest reporting.

## Note

At least two schedule updates sharing a common completion milestone are needed to produce a projection. Depth of history matters: a read based on very few updates will show a wide confidence range even when the trend itself looks clear.
