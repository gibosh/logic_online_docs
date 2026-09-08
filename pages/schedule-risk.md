---
page-id: schedule-risk
route: /module/8/project/:projectId
title: Schedule Risk Analysis (SRA)
audience: external
status: complete
version: 1.2.1
last-reviewed: 2026-09-08
---

## Schedule Risk Analysis (SRA)

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

**How it's calculated:** each simulation run draws a random duration multiplier for every activity or WBS block from its three-point range (optimistic/most likely/pessimistic, as a percentage of plan, set by the chosen preset or a per-block override), using a **triangular distribution** – the standard three-point-estimate shape for this kind of model, weighted toward the most-likely value rather than spread evenly between the extremes:

> For optimistic value *a*, most likely value *m*, pessimistic value *b*, and a random draw *u* between 0 and 1: the crossover point *c* = (*m* − *a*) ÷ (*b* − *a*). If *u* < *c*, the drawn multiplier = *a* + √(*u* × (*b* − *a*) × (*m* − *a*)); otherwise the drawn multiplier = *b* − √((1 − *u*) × (*b* − *a*) × (*b* − *m*))
> Simulated duration = planned duration × drawn multiplier

In "Summarised" detail, every activity in the same WBS block shares one drawn multiplier per run, so the block moves together; in "Full" detail, every activity draws its own multiplier independently. The schedule's logic is then re-evaluated forward with these simulated durations, and the resulting project finish date is recorded – repeated for the chosen number of iterations.

If a risk register is included, each risk is tested independently on every run: it "fires" if a separate random draw is below its stated likelihood, and if it fires, its day-impact is drawn the same way, from its own minimum/most-likely/maximum range, using the triangular-distribution formula above (an opportunity's drawn impact is subtracted rather than added). A risk aimed at a specific activity adds its drawn days straight onto that activity's simulated duration; a risk aimed at a WBS block instead stretches that block's duration proportionally:

> Block stretch factor = 1 + (drawn day-impact ÷ the block's own internal critical-path length), never allowed below 0.05 – so a risk can't shrink a block to less than 5% of its length

Across every iteration, two separate statistics are produced. The **Tornado ranking** is a **Pearson correlation** between each activity or WBS area's random draw across all iterations and the resulting project finish date across those same iterations, ranked by the size of that correlation regardless of direction:

> Tornado rank = |Pearson correlation between the area's per-iteration random draw and the per-iteration project finish date|, highest first

The **criticality index** asks a different question – not how much an area's uncertainty moves the finish date, but how often that area actually ends up being the thing the finish date depends on:

> Criticality index (%) = the percentage of iterations in which that activity or WBS area contained the activity that actually determined the finish date in that run (the one with the latest calculated finish after the forward pass)

An area can rank high on one measure and low on the other – for example, a naturally short, low-uncertainty activity that's nonetheless on the driving path in every run scores a high criticality index but a modest Tornado rank, because varying its duration barely moves the outcome.

Date constraints already in the schedule (Start On, Finish On, Finish On or After, Mandatory Start, Mandatory Finish, As Late As Possible, and others) are not applied during the simulation – every constraint type is uniformly set aside ("neutralised"), not selectively removed, so the simulation reflects the schedule's logic and durations alone. This is also why, under the "forward-pass logic" plan-baseline setting, the model's own deterministic (no-uncertainty) finish date can differ from P6's constrained finish date – it is the schedule's real logic, unclamped. Each run uses a fresh random seed taken from the current time – running the simulation again on unchanged inputs and settings produces a new random draw and a slightly different result, not a repeat of the previous run.

## Note

This model does not account for correlation between unrelated activities, cascading risk effects, or resource constraints – it is a schedule-logic and duration-uncertainty model, not a full project risk simulation. Treat the ranking as a guide to where to focus attention, not a complete risk assessment on its own.
