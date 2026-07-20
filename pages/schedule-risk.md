---
page-id: schedule-risk
route: /module/8/project/:projectId
title: Schedule Risk Analysis (SRA)
audience: external
status: draft
version: 1.0.0
last-reviewed: 2026-07-20
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
| Preset | Conservative, Balanced, or Opportunity-rich starting ranges |

## Note

This model does not account for correlation between unrelated activities, cascading risk effects, or resource constraints – it is a schedule-logic and duration-uncertainty model, not a full project risk simulation. Treat the ranking as a guide to where to focus attention, not a complete risk assessment on its own.
