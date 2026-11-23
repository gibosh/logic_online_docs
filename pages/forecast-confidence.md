---
page-id: forecast-confidence
route: /module/1/project/:projectId
title: Forecast Confidence
audience: external
status: draft
version: 2.0.0
last-reviewed: 2026-09-17
blocked-reason: Full rewrite following a ground-up engine and UI change (LUSB-1249, 2026-09-16). Content verified directly against the current ForecastConfidenceModule source (analyze.ts/predict.ts/series.ts) and its call path, but exact on-screen wording still needs a live-app screenshot before promoting to complete. The previous worked-examples section (specific percentages and dates run through the old formula) has been removed rather than carried forward inaccurately — new worked examples need recalculating against this engine before publishing; see workspace/GAPS.md.
---

A reported finish date doesn't tell you at a glance whether it's achievable or optimistic. Some schedules lose about a month of completion date for every month that passes while still looking healthy on paper, because the delay sits in work that hasn't started yet. Others appear to recover, but the recovery came from cutting planned durations and adding logic links, not from progress on site.

Forecast Confidence rates how far you can trust the reported finish date. It's shown as a percentage, but read it as a band (green, amber or red) rather than a precise score: the difference between 48% and 52% means nothing, only the band does. It's built from the schedule's own update history: how stable the finish date is, how consistently it has moved, and how much float the programme has used up.

A planner or project manager can use it to decide quickly whether to believe the finish date. If the answer is amber or red, the detail behind the signal shows why.

## Report details

A confidence gauge shows a percentage, a green, amber or red reading, and a risk label (**healthy**, **watch** or **Distressed**) with a one-line explanation underneath:

| Risk label | Shown when | Explanation shown |
|---|---|---|
| healthy | Recent movement is well contained and negative float is rare | "Recent movement is managed and schedule pressure is low." |
| watch | Some pressure or one-way movement remains | "Some pressure or one-way movement remains — the forecast needs watching." |
| Distressed | The finish is slipping with no recovery, or there's no float left to absorb more delay | "The finish date is slipping with no recovery, or there's no buffer left to absorb more delay – expect this range to stay wide." |

Below the gauge, a timeline chart plots the **reported finish** at each update's data date, the **projected central** finish date, and an **earliest–latest range** either side of it. There's no separate "today" marker: the latest point plotted is the latest update's data date, which may not be today's date. The confidence percentage measures how wide that range is compared with the time remaining. A tight range with a long time remaining reads as high confidence, and a wide range with little time remaining reads as low confidence.

Below the timeline, a **Schedule metrics** grid lists the six metrics the model is built from. Each has a small sparkline (labelled "Last *n* estimates") and a green, amber or red status:

| Metric | What it reflects |
|---|---|
| Slip pace | How quickly the finish date is drifting later, in months of drift per month elapsed. |
| Date bounce | How much the finish-date drift has swung from update to update recently, instead of moving smoothly. |
| Meaningful plan growth | Growth in activity count since the first update, weighted toward work still remaining and reduced for any recovery already under way. |
| Mitigation ratio | How much of the schedule's recent slippage has been offset by pulling other work forward, instead of being absorbed as delay. |
| Schedule pressure | The largest share of incomplete work with negative float across the last few updates. |
| Reporting depth | How many accepted schedule updates the reading is built from. More updates make every other metric more reliable. |

Each metric card also has a calculator icon that explains its calculation, and an arrow showing whether it's improving, worsening or holding steady.

If any uploaded schedule updates were left out of the reading (see "Which files count" below), a **disclosure line** reads "*N* schedule update(s) excluded". Expand it to see each excluded file and the reason.

A closing note under the grid reads: *"This score isn't the whole picture. The Float Burn-down & Earned Schedule analytic is a separate, independent check on the same finish date that uses different inputs – use it to verify predicted finish dates."* Treat that analytic as a second opinion. It's calculated independently of Forecast Confidence and doesn't feed into the percentage, so the two can disagree.

## Calculation and other logic

**Data used:** every schedule update uploaded for the project, plus the activity identified as the project's completion milestone. Logic+ detects the milestone by name: activities named along the lines of "Practical Completion" come first, then "Final/Contract/All of the Works/Project Completion", then a generic "Completion".

**Which files count:** the update with the most activities is the reference. Every other update is compared with it using a **Jaccard similarity** score: the number of activities the two files share, divided by the total number of distinct activities across both. A file scoring below 0.5 (fewer than half its activities in common with the reference) is treated as a different schedule's scope and excluded, with the reason "different scope (overlap *X*%)" in the disclosure line. Where two updates share the same data date, the one with fewer activities is excluded, with the reason "duplicate of *date*". At least two updates must remain, at least half a month (about 15 days) apart, or no reading is produced (see Note below).

**Slip pace** is a weighted Theil–Sen-style estimate of how fast the finish date is drifting. For every pair of remaining updates at least half a month apart, Logic+ works out how much further the finish date had drifted at the later update than at the earlier one, divided by the time between them. It then takes the **weighted median** of all those pairwise drift rates, giving more weight to pairs that end more recently:

> For each pair of updates (earlier, later) at least 0.5 months apart:
> pairwise rate = (drift at later update − drift at earlier update) ÷ (months between them)
> weight = 0.9 ^ (how many updates back the later one is from the most recent)
>
> Slip pace = weighted median of every pairwise rate, never reported below zero

Because every pair of updates is compared, not only neighbouring ones, an occasional push followed by a pull-back can't hide a steady slow drift.

**The forecast (central) finish date** starts from the reported finish date and moves it later by the slip pace. It's capped so that one bad reading can't push the forecast out by more than one and a half times the programme time left:

> Forecasted finish = reported finish + (slip pace × months remaining), capped at 1.5 × months remaining

**The earliest–latest range** sits evenly either side of that forecast date, with the same half-width on both sides:

> Half-width (months) = the larger of *(months remaining × (0.15 + 0.25 × volatility score))* or *(2 + 3 × volatility score)*, then narrowed by an update-count adjustment – the square root of (6 ÷ number of updates used, or 2 if there are fewer than 2 updates)
>
> Early bound = forecasted shift − half-width, never earlier than the reported finish date itself
> Late bound = forecasted shift + half-width

**The volatility score** combines three of the six metrics, each scaled to a comparable range and weighted so date bounce counts most, plan growth next and schedule pressure least, using each metric's recent reading:

> Volatility score = 0.5 × (Date bounce ÷ 3) + 0.3 × (Meaningful plan growth ÷ 40, floored at 0) + 0.2 × (Schedule pressure ÷ 100), capped at 1.2

**Turning the range into a percentage:** the confidence percentage compares the width of the earliest–latest range with the time left on the project. A range that's narrow compared with the time remaining reads as high confidence, and a wide one as low confidence:

> Confidence % = 100 × (1 − (width of the range ÷ (months remaining × 0.8 + 8))), rounded, and held between 5% and 95%

The `+ 8` is a fixed allowance, so a project with almost no time left doesn't look highly confident just because there's little time left to slip. The bands are **70% or above green, 40–69% amber, and below 40% red.**

**The six metrics in detail:**

| Metric | Calculation | Green / amber / red |
|---|---|---|
| Slip pace | The weighted pairwise drift rate described above, in months of drift per month elapsed. | below 0.4 / 0.4–0.7 / above 0.7 |
| Date bounce | The standard deviation of the month-to-month change in finish-date drift, over the last 6 update-to-update steps. | below 1 / 1–2 / above 2 (months) |
| Meaningful plan growth | Activity-count growth per WBS branch between the first and latest update, weighted toward branches carrying more of the remaining work, then reduced by the mitigation ratio below, so growth that's being absorbed reads lower. | below 10% / 10–25% / above 25% |
| Mitigation ratio | Averaged over the last 3 updates: working days of finish dates pulled earlier, divided by working days pushed later, capped at 1 per update (an update with nothing pushed later scores fully mitigated). For this metric alone, *higher* is better. | below 15% / 15–40% / above 40% (good) |
| Schedule pressure | The largest share of incomplete activities with negative float, across the last 3 updates. | below 10% / 10–40% / above 40% |
| Reporting depth | The number of updates the reading is built from, after the scope check above. | below 4 / 4–8 / above 8 updates |

## Note

A reading needs at least two schedule updates that share a completion milestone, at least half a month apart. With fewer, Logic+ shows no signal and explains what's missing (for example, a wider gap between updates).
