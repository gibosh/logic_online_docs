---
page-id: completion-forecast
route: /module/7/project/:projectId
title: Completion Forecast
audience: external
status: draft
version: 1.1.1
last-reviewed: 2026-09-08
---

## Completion Forecast

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

**How it's calculated:** update files that don't share enough activity codes with the largest uploaded file (a **Jaccard similarity** below 0.5, the same coherence check used in Forecast Confidence) are excluded as a different schedule's data. The remaining files are lined up in date order to measure how the completion milestone's forecast date has moved, how consistently, how much float has gone negative, and how well completed work has tracked the plan. The projection and its confidence range are entirely rules-based arithmetic on this history – there is no simulation or random element, so identical inputs always produce the identical output.

**What's driving the programme** is a plain count, not a weighted score: every activity currently out of spare time (or, if none are, the worst 15% by float) is sorted into a category by keyword-matching its name, and the percentage shown against each category is simply that category's share of the activity count in that group – it is not weighted by float, duration, or delay impact.

## How the projection works

The rate used for the rules-adjusted finish is the **worst sustained rate** seen across the schedule's update history – not an average, and not just the most recent trend. This is deliberate: an average can be flattered by a short-lived, unsustained recovery.

> Worst sustained slip rate = the highest of: the overall average slip rate (total slip so far ÷ total time elapsed), the slip rate measured over just the last six updates (or fewer, if there are fewer than six available), and the highest slip rate ever recorded at any single update across the whole series

> Rules-adjusted shift = worst sustained slip rate × months remaining, capped at 1.5 × months remaining
> Rules-adjusted finish = reported finish + rules-adjusted shift

The likely range around that date widens with schedule volatility – the same **volatility score** used in Forecast Confidence, blending step-to-step volatility, scope growth, and negative-float share – and narrows both as more updates come in for this project and as more projects build up this module's own benchmark database:

> Half-width of the range (months) = (0.25 × months remaining + 6 × volatility score) × update-count adjustment × benchmark-calibration adjustment
> Update-count adjustment = √(6 ÷ number of updates used, or 2 if fewer than 2 are available)
> Benchmark-calibration adjustment = 1 + 0.5 ÷ (1 + number of projects in the benchmark database ÷ 4)

Unlike Forecast Confidence's range, which leans a fixed amount further toward "later" regardless of the schedule's track record, this module's range is skewed by a **reality-backing trust factor** – how closely actual finished work has tracked the plan, checked with the same Pearson correlation used in Forecast Confidence's honesty panel. Here, that check is a direct input to the range, not just a separate read-out:

> Earlier bound = the larger of 0 or (rules-adjusted shift − half-width × trust factor)
> Later bound = rules-adjusted shift + half-width × (2 − trust factor)

The trust factor is 1 when the correlation between finished-work variance and slip is 0.6 or above, 0.6 when it's 0.3–0.6, 0.3 when it's below 0.3, and a default of 0.6 when there isn't yet enough finished work to check (fewer than 20 finished activities across fewer than 4 updates) – so a schedule with a track record of paper recoveries genuinely produces a wider, later-skewed range, and one with a track record of honest reporting produces a tighter, more evenly-balanced one.

> Forecast confidence % = 100 × (1 − (width of the range ÷ (months remaining × 0.8 + 8))), rounded, and held between 5% and 95%

This is the same style of confidence read as Forecast Confidence, but it is calculated from this module's own wider range and trust factor, not copied from the Forecast Confidence module's own number.

## Note

At least two schedule updates sharing a common completion milestone are needed to produce a projection. Depth of history matters: a read based on very few updates will show a wide confidence range even when the trend itself looks clear.

Refresh the page after switching projects before opening Completion Forecast, to make sure you're looking at the current project's numbers.
