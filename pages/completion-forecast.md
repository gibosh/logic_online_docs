---
page-id: completion-forecast
route: /module/7/project/:projectId
title: Completion Forecast
audience: external
status: draft
version: 2.0.1
last-reviewed: 2026-09-23
blocked-reason: Full rewrite following a ground-up engine change (LUSB-1249, 2026-09-16) – this page now shares its calculation engine with Forecast Confidence rather than running its own. The previously-documented "What's driving the programme" section (driving-path activities grouped by category) could not be found anywhere in the current source and appears to have been removed in this rewrite – flagged in workspace/GAPS.md for PM confirmation before this note is cleared. Content verified against source, not yet screenshot-verified live. Fixed five internal cross-links to Forecast Confidence that were missing the `pages/` prefix every other link on the site uses.
---

## Overview

A project's reported finish date and a projection based on the schedule's own track record often disagree, sometimes by many months. Showing only the reported date hides that gap.

Completion Forecast puts the two side by side. You see the reported finish date over time, the date the projection gives instead, and the size of the gap between them, so when a project is reported as "on track" you can check that claim against the schedule's own history.

The forecast is a rules-based projection from the schedule's update history, not a Monte Carlo simulation, so the same inputs always give the same output, and every step of the projection is documented below.

**Completion Forecast uses the same calculation as [Forecast Confidence](pages/forecast-confidence.md).** Both read the same slip pace, finish window and confidence percentage, which are described in full on that page. This page explains how the projection is built in three steps, and adds a comparison with an older model and a cross-check against Float Burn-down & Earned Schedule.

## Report details

A **reported finish date** history chart, with its axis labelled "Forecast finish", plots **Reported finish** (the completion date in each update), **Projected central** (the rules-based estimate) and the **Earliest–latest range** around it, over a "Reporting date" timeline.

Three steps explain how the projection is built:

| Step | Label | What it explains |
|---|---|---|
| Step 1 | The centre — Pace × time remaining | How the central forecast date is derived from slip pace and months remaining |
| Step 2 | The width — Uncertainty, honestly sized | How the earliest–latest range's width is calculated |
| Step 3 | The bounds — Early and late finish | How the width is applied evenly either side of the central date |

A **data quality** disclosure ("*N* schedule update(s) excluded") lists any uploaded schedules left out of the analysis and the reason, using the same scope check as [Forecast Confidence](pages/forecast-confidence.md#calculation-and-other-logic).

A collapsed **Legacy v1 comparison** shows an older calculation for reference, and a collapsed **Independent second opinions** section checks the forecast against Float Burn-down & Earned Schedule.

A methodology note reads: *"Reasoned projection, not Monte Carlo. The model constants are fitted to reference programmes; this view does not apply a self-calibrated residual range."*

## Calculation and other logic

**Data used:** every schedule update uploaded for the project, tracked over time, using the same calculation as [Forecast Confidence](pages/forecast-confidence.md) but shown as a step-by-step breakdown instead of a confidence score.

**Which files count:** the same as Forecast Confidence. Updates scoring below 0.5 **Jaccard similarity** against the largest uploaded file are excluded as a different schedule's scope, and of two updates with the same date, the one with more activities is kept. See [Forecast Confidence's "Which files count"](pages/forecast-confidence.md#calculation-and-other-logic) for the exact rule and disclosure wording.

### Step 1: The centre, pace × time remaining

The central forecast date is the reported finish date moved later by the **slip pace** (the weighted pairwise drift rate described in [Forecast Confidence](pages/forecast-confidence.md#calculation-and-other-logic)) multiplied by the months remaining, capped at 1.5× the months remaining:

> Central shift = slip pace × months remaining, capped at 1.5 × months remaining
> Central finish = reported finish + central shift

### Step 2: The width, uncertainty honestly sized

The half-width of the range uses the same formula as Forecast Confidence: a volatility-scaled term or a fixed minimum, whichever is larger, narrowed as more updates come in:

> Half-width (months) = the larger of *(months remaining × (0.15 + 0.25 × volatility score))* or *(2 + 3 × volatility score)*, then narrowed by the square root of (6 ÷ number of updates used, or 2 if fewer than 2)

The volatility score is the same blend of Date bounce, Meaningful plan growth and Schedule pressure described on the Forecast Confidence page. The calculation has no adjustment for a cross-project reference library. (The older model below still has one; it was never driven by real usage data.)

### Step 3: The bounds, early and late finish

The full half-width is applied evenly either side of the central date, with no lean toward later or earlier:

> Early bound = central shift − half-width, never earlier than the reported finish date itself
> Late bound = central shift + half-width

> Forecast confidence % = 100 × (1 − (width of the range ÷ (months remaining × 0.8 + 8))), rounded, and held between 5% and 95%

The formula and band cut-offs (70% green / 40–69% amber / below 40% red) are the same as Forecast Confidence's. Because both pages run the same calculation on the same schedule history, they show the same confidence percentage for a project.

## Legacy v1 comparison

A collapsed section shows an older version of this projection for reference, labelled: *"This older model uses the worst observed slip rate and is not the headline forecast."* It shows its own central date, range and confidence percentage, calculated differently from the model above:

- Its rate is the **worst sustained rate** in the schedule's history: the highest of the lifetime average slip rate, the rate over the last six updates, and the worst rate recorded at any single update. Steps 1–3 use the weighted pairwise estimate instead.
- Its range is skewed by a fixed **trust factor of 0.6** instead of being measured from real data:

> Earlier bound = the larger of 0 or (central shift − half-width × 0.6)
> Later bound = central shift + half-width × (2 − 0.6)

It also applies a fixed cross-project reference-library adjustment to its width, which the main model doesn't use. That's another reason it's kept only as a labelled comparison and not the headline figure.

## Independent second opinions

A collapsed section checks the forecast against the same finish date calculated in two other ways: the **Float Burn-down & Earned Schedule** analytic's float-implied finish (from the programme-wide driving decile) and its earned-schedule finish. Neither is blended into the Completion Forecast figure or its confidence score. They're shown only as an independent check, and the section explains when they're unavailable (for example, when there are fewer than three usable updates with float data).

## Note

A projection needs at least two schedule updates that share a completion milestone, at least half a month apart. The more history there is, the better: a reading based on very few updates shows a wide confidence range even when the trend looks clear.

After switching projects, refresh the page before opening Completion Forecast, to make sure you're looking at the current project's figures.
