---
page-id: completion-forecast
route: /module/7/project/:projectId
title: Completion Forecast
audience: external
status: draft
version: 2.0.0
last-reviewed: 2026-09-17
blocked-reason: Full rewrite following a ground-up engine change (LUSB-1249, 2026-09-16) – this page now shares its calculation engine with Forecast Confidence rather than running its own. The previously-documented "What's driving the programme" section (driving-path activities grouped by category) could not be found anywhere in the current source and appears to have been removed in this rewrite – flagged in workspace/GAPS.md for PM confirmation before this note is cleared. Content verified against source, not yet screenshot-verified live.
---

## Completion Forecast

The reported finish date on a project and a rigorous, data-driven projection based on the schedule's own track record often disagree – sometimes by many months. Showing only the reported date hides that gap.

Completion Forecast puts both figures side by side. You see what has been reported as the finish date over time, what the data-driven adjustment says instead, and how large the gap between them is. So when someone reports a project as "on track," you can check that claim against the schedule's own history – not just take it at face value.

The forecast is produced from the schedule's update history using a rules-adjusted projection, not a Monte Carlo simulation. The same inputs always produce the same output – there is no random element. This makes the result defensible in a meeting: it is a reasoned, documented projection, not a black-box estimate.

**Completion Forecast now shares its calculation engine with [Forecast Confidence](forecast-confidence.md)** – both pages read the same slip-pace, finish-window, and confidence-percentage calculation, described in full on that page. This page focuses on how the projection builds up in three steps, plus an independent legacy comparison and a float/earned-schedule cross-check.

## Report details

A **reported finish date** history chart, an axis labelled "Forecast finish," plotting: **Reported finish** (the completion date in each update), **Projected central** (the rules-adjusted estimate), and the **Earliest–latest range** around it, over a "Reporting date" timeline.

Three explanatory steps walk through how the projection is built:

| Step | Label | What it explains |
|---|---|---|
| Step 1 | The centre — Pace × time remaining | How the central forecast date is derived from slip pace and months remaining |
| Step 2 | The width — Uncertainty, honestly sized | How the earliest–latest range's width is calculated |
| Step 3 | The bounds — Early and late finish | How the width is applied evenly either side of the central date |

A **data quality** disclosure ("*N* schedule update(s) excluded") lists any uploaded schedules left out of the analysis and why – the same coherence check described in [Forecast Confidence](forecast-confidence.md#calculation-and-other-logic).

A collapsed **Legacy v1 comparison** shows an older calculation for reference, and a collapsed **Independent second opinions** section cross-checks the forecast against Float Burn-down & Earned Schedule.

A methodology note: *"Reasoned projection, not Monte Carlo. The model constants are fitted to reference programmes; this view does not apply a self-calibrated residual range."*

## Calculation and other logic

**Data used:** every schedule update file uploaded for the project, tracked over time – the same engine as [Forecast Confidence](forecast-confidence.md), applied here with its own explanatory breakdown rather than a standalone confidence score.

**Which files count:** identical to Forecast Confidence – updates scoring below 0.5 **Jaccard similarity** against the largest uploaded file are excluded as a different schedule's scope; same-dated duplicates keep whichever has more activities. See [Forecast Confidence's "Which files count"](forecast-confidence.md#calculation-and-other-logic) for the exact rule and disclosure wording.

### Step 1 – The centre: pace × time remaining

The central forecast date is the reported finish date shifted forward by **slip pace** – the same weighted, pairwise drift-rate calculation described in [Forecast Confidence](forecast-confidence.md#calculation-and-other-logic) – multiplied by months remaining, capped at 1.5× months remaining:

> Central shift = slip pace × months remaining, capped at 1.5 × months remaining
> Central finish = reported finish + central shift

### Step 2 – The width: uncertainty, honestly sized

The uncertainty half-width uses the same formula as Forecast Confidence – a volatility-scaled term or an absolute floor, whichever is larger, narrowed as more updates come in:

> Half-width (months) = the larger of *(months remaining × (0.15 + 0.25 × volatility score))* or *(2 + 3 × volatility score)*, then narrowed by the square root of (6 ÷ number of updates used, or 2 if fewer than 2)

The volatility score is the same blend of Date bounce, Meaningful plan growth, and Schedule pressure described on the Forecast Confidence page. There is no separate calibration-library adjustment in this calculation – an earlier version of this model widened the range by a fixed amount to account for a cross-project reference library, but that adjustment was never actually driven by real usage data and has been removed.

### Step 3 – The bounds: early and late finish

The full half-width is applied evenly either side of the central date – there is no lean toward "later" or "earlier":

> Early bound = central shift − half-width, never earlier than the reported finish date itself
> Late bound = central shift + half-width

> Forecast confidence % = 100 × (1 − (width of the range ÷ (months remaining × 0.8 + 8))), rounded, and held between 5% and 95%

This is the identical formula and band cutoffs (70% green / 40–69% amber / below 40% red) as Forecast Confidence – because both pages now run the same engine on the same schedule history, this page's confidence percentage and Forecast Confidence's own reading are the same number for the same project.

## Legacy v1 comparison

A collapsed section shows an older version of this projection for reference, labelled: *"This older model uses the worst observed slip rate and is not the headline forecast."* It shows its own central date, range, and confidence percentage, calculated differently from the model above:

- Its rate is the **worst sustained rate** ever seen in the schedule's history – not the pairwise weighted estimate Steps 1–3 use, but the highest of: the lifetime average slip rate, the rate over the last six updates, and the single worst rate ever recorded at any one update.
- Its range is skewed by a fixed **trust factor of 0.6** rather than measured from real data:

> Earlier bound = the larger of 0 or (central shift − half-width × 0.6)
> Later bound = central shift + half-width × (2 − 0.6)

This older model also still applies a fixed cross-project reference-library adjustment to its width (the one removed from the main model in Step 2 above) – another reason it's kept only as a labelled comparison, not the headline figure.

## Independent second opinions

A collapsed section cross-checks the forecast against the same finish date calculated a different way: the **Float Burn-down & Earned Schedule** analytic's float-implied finish (from the programme-wide driving decile) and its earned-schedule-implied finish. Neither of these is blended into the Completion Forecast figure or its confidence score – they're shown purely as an independent check, and the section explains when they're unavailable (for example, needing at least three usable updates with float data).

## Note

At least two schedule updates sharing a common completion milestone, at least half a month apart, are needed to produce a projection. Depth of history matters: a read based on very few updates will show a wide confidence range even when the trend itself looks clear.

Refresh the page after switching projects before opening Completion Forecast, to make sure you're looking at the current project's numbers.
