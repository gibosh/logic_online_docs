---
page-id: schedule-risk
route: /module/8/project/:projectId
title: Schedule Risk Analysis (SRA)
audience: external
status: complete
version: 1.1.1
last-reviewed: 2026-09-03
---

## Schedule Risk Analysis (SRA) – Overview

A finish date on its own doesn't tell you how much uncertainty sits behind it, or which risks matter most to it. Schedule Risk runs a quantitative schedule risk analysis – the kind of Monte Carlo simulation planners traditionally run in a separate risk tool – directly against your uploaded schedule, and ranks the risk drivers by how much they actually move the finish date.

Keeping this analysis inside Logic+, against the same schedule data as the rest of your reliability picture, means the risk view doesn't live disconnected in a separate spreadsheet nobody re-reads.

**Note:** the simulation itself runs entirely in your browser – the schedule and risk register are never sent anywhere to compute the result, and saved risk models are kept in your browser's local storage only.

## What you provide

- **A schedule** – selected from the schedules already uploaded to the project; no separate file upload is needed for this module.
- **A risk register** (optional) – paste rows copied from a spreadsheet, with columns for risk ID, the activity or WBS area it targets, risk name, type, likelihood, and a minimum/most likely/maximum duration impact. Column headers are matched automatically.

## Report details

A **Tornado chart** ranking the activities or WBS areas that most affect the finish date – by duration uncertainty, by risk register impact, or both together, depending on the analysis mode selected.

Headline finish-date statistics at **P50, P80, and P90** (the 50th, 80th, and 90th percentile finish dates across the simulation), plus a full distribution you can inspect. You can also drill into individual milestones to see their own P50/P80/P90 distribution.

## Options

| Setting | What it controls |
|---|---|
| Analysis mode | Uncertainty only, risk register only, or both combined |
| Detail level | Summarised (WBS block level) or full (every activity) |
| Iterations | 1,000 / 2,000 / 5,000 simulation runs |
| Risk-model level | The WBS granularity risk ranges are applied at |
| Preset | Conservative, Balanced + opportunity, or Opportunity-rich starting ranges |
| Plan baseline | What the simulated finishes are compared against: the schedule's own forward-pass logic date, or P6's scheduled finish. Because constraints are neutralised for the simulation (see below), the two can legitimately differ — this setting only changes the reference line results are read against, not the simulation itself |

## Calculation and other logic

**Data used:** the selected schedule, plus an optional pasted risk register (risk ID, target activity or WBS area, likelihood, and a minimum/most-likely/maximum day impact per risk). Risk register rows are matched to activities by **exact activity code match** – there is no fuzzy or approximate matching, so a mistyped or reformatted activity code in a pasted row will not match and that risk will be excluded from the run rather than partially matched.

**How it's calculated:** each simulation run draws a random duration for every activity or WBS block from its three-point range (optimistic/most likely/pessimistic, set by the chosen preset or a per-block override), re-evaluates the schedule's logic forward, and records the resulting project finish date – repeated for the chosen number of iterations. If a risk register is included, each risk independently "fires" per its own likelihood on each iteration and adds its day impact on top. Across every iteration, the Tornado ranking measures how strongly each activity or WBS area's duration correlates with a later finish; the criticality index separately measures how often that area actually ended up on the driving path. Date constraints already in the schedule (Start On, Finish On, Finish On or After, Mandatory Start, Mandatory Finish, As Late As Possible, and others) are not applied during the simulation – every constraint type is uniformly set aside ("neutralised"), not selectively removed, so the simulation reflects the schedule's logic and durations alone. This is also why, under the "forward-pass logic" plan-baseline setting, the model's own deterministic (no-uncertainty) finish date can differ from P6's constrained finish date – it is the schedule's real logic, unclamped. Each run uses a fresh random seed – running the simulation again on unchanged inputs and settings produces a new random draw and a slightly different result, not a repeat of the previous run. (This was not always true: a hardcoded seed previously made every run identical regardless of inputs – fixed in LUSB-1100, merged 2026-07-28. If you're validating results against an older screenshot or write-up, expect the exact numbers to differ now even on the same file.)

## Note

This model does not account for correlation between unrelated activities, cascading risk effects, or resource constraints – it is a schedule-logic and duration-uncertainty model, not a full project risk simulation. Treat the ranking as a guide to where to focus attention, not a complete risk assessment on its own.
