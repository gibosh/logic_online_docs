---
page-id: forecast-confidence
route: /module/1/project/:projectId
title: Forecast Confidence
audience: external
status: draft
version: 1.0.0
last-reviewed: 2026-07-20
---

## Why this page exists

When a project reports a finish date, there is often no way to tell at a glance whether that date is genuinely achievable or quietly optimistic. Some schedules lose roughly a month of completion date for every month that passes, while still looking healthy on paper — the delay is hiding in work that hasn't started yet. Others show what appears to be a recovery, but the recovery was manufactured by cutting planned durations and adding logic links, not by real progress on site.

Forecast Confidence gives you an honest trust read on the reported finish date — deliberately a plain green, amber, or red signal, not a false-precision score out of 100. It is built from the schedule's own update history: is the finish date stable, does completed work back up the plan, how much float remains, has the programme been heavily reworked recently, and did past apparent recoveries actually hold?

A planner or project manager can use this to answer the question "should I believe the finish date?" in seconds — and if the answer is amber or red, the detail behind the signal shows exactly why.

## What you will see

A confidence gauge showing a percentage and a green, amber, or red read, with a one-line explanation underneath.

Below the gauge, a timeline shows **today**, the **reported finish date**, a **realistic (rules-adjusted) finish date**, and a **likely range** either side of it. The confidence percentage is a measure of how wide that likely range is relative to the time remaining — a tight range against a long remaining duration reads as high confidence; a wide range against a short remaining duration reads as low confidence.

Five factor tiles sit below the timeline, each with its own small gauge, a short trend history, and a one-line tip:

| Tile | What it reflects |
|---|---|
| Plan stability | How much the forecast finish date has moved around update to update |
| Slip rate | How fast the forecast finish date is moving out, relative to time elapsed |
| Plan churn | How much the activity count has grown since the first uploaded update |
| Schedule pressure | The peak share of incomplete activities running negative float, across all updates |
| Reporting depth | How many usable schedule updates the read is based on |

A closing note identifies the single biggest activity currently driving the forecast slip, and its working-day impact.

## Note

The confidence signal is capped at moderate until enough work has actually finished to be judged reliable. The signal measures how consistent and stable the finish date has been — it does not claim to know whether the date itself is correct.

At least two schedule updates sharing a common completion milestone are needed to produce a read. With fewer, or if no completion milestone is common to every uploaded update, no signal is shown.
