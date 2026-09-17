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

## Forecast Confidence

When a project reports a finish date, there is often no way to tell at a glance whether that date is genuinely achievable or quietly optimistic. Some schedules lose roughly a month of completion date for every month that passes, while still looking healthy on paper – the delay is hiding in work that hasn't started yet. Others show what appears to be a recovery, but the recovery was manufactured by cutting planned durations and adding logic links, not by real progress on site.

Forecast Confidence gives you an honest trust read on the reported finish date. It's shown as a percentage, but it should be read as a band – green, amber, or red – rather than a precise score: the difference between 48% and 52% isn't meaningful, only which band you land in is. It's built from the schedule's own update history: is the finish date stable, how consistently has it moved, and how much float has the programme burned through.

A planner or project manager can use this to answer the question "should I believe the finish date?" in seconds – and if the answer is amber or red, the detail behind the signal shows exactly why.

## Report details

A confidence gauge showing a percentage, a green/amber/red read, and a risk label – **healthy**, **watch**, or **Distressed** – with a one-line explanation underneath:

| Risk label | Shown when | Explanation shown |
|---|---|---|
| healthy | Recent movement is well contained and negative float is rare | "Recent movement is managed and schedule pressure is low." |
| watch | Some pressure or one-way movement remains | "Some pressure or one-way movement remains — the forecast needs watching." |
| Distressed | The finish is slipping with no recovery, or there's no float left to absorb more delay | "The finish date is slipping with no recovery, or there's no buffer left to absorb more delay – expect this range to stay wide." |

Below the gauge, a timeline chart plots the **reported finish** at each update's data date, the **projected central** finish date, and an **earliest–latest range** either side of it. There is no separate "today" marker – the most recent point plotted is the latest update's own data date, not necessarily today's calendar date. The confidence percentage is a measure of how wide that range is relative to the time remaining – a tight range against a long remaining duration reads as high confidence; a wide range against a short remaining duration reads as low confidence.

Below the timeline, a **Schedule metrics** grid lists six metrics the model is built from, each with its own small sparkline (labelled "Last *n* estimates") and a green/amber/red status:

| Metric | What it reflects |
|---|---|
| Slip pace | How quickly the finish date is drifting later, in months of drift per month elapsed. |
| Date bounce | How much the finish-date drift has swung from update to update recently, rather than moving smoothly. |
| Meaningful plan growth | Growth in activity count since the first update, weighted towards work that's still remaining and discounted for any recovery already under way. |
| Mitigation ratio | How much of the schedule's recent slippage has been offset by pulling other work forward, rather than just absorbed as pure delay. |
| Schedule pressure | The worst share of incomplete work that's been out of float across the last few updates. |
| Reporting depth | How many accepted schedule updates the read is built from – more updates make every other metric more trustworthy. |

Each metric card also shows a calculator icon explaining its calculation and an arrow showing whether it's improving, worsening, or holding steady.

If any uploaded schedule updates were excluded from the read (see "Which files count," below), a **disclosure line** appears reading "*N* schedule update(s) excluded," which expands to list each excluded file and why.

A closing note under the grid: *"This score isn't the whole picture. The Float Burn-down & Earned Schedule analytic is a separate, independent check on the same finish date that uses different inputs – use it to verify predicted finish dates."* Read this as a second opinion, not an input to the percentage above – Forecast Confidence and Float Burn-down & Earned Schedule are calculated independently and can genuinely disagree.

## Calculation and other logic

**Data used:** every schedule update file uploaded for the project, plus the activity identified as the project completion milestone – auto-detected by name (activities named along the lines of "Practical Completion" are prioritised, then "Final/Contract/All of the Works/Project Completion," then a generic "Completion").

**Which files count:** the update with the most activities is used as the reference. Every other update is compared against it using a **Jaccard similarity** score – the number of activities the two files share, divided by the total number of distinct activities across both. A file scoring below 0.5 (fewer than half its activities in common with the reference) is excluded as a different schedule's scope, shown in the disclosure line with the reason "different scope (overlap *X*%)." Where two updates share the same data date, the one with fewer activities is excluded instead, with the reason "duplicate of *date*." At least two updates must remain, at least half a month (about 15 days) apart, or no read is produced (see Note, below).

**Slip pace** is a weighted Theil–Sen-style estimate of how fast the finish date is drifting. For every pair of remaining updates at least half a month apart, Logic+ works out how much further the finish date had drifted at the later update compared to the earlier one, divided by the time between them – then takes the **weighted median** of every one of those pairwise drift rates, weighting pairs that end more recently more heavily:

> For each pair of updates (earlier, later) at least 0.5 months apart:
> pairwise rate = (drift at later update − drift at earlier update) ÷ (months between them)
> weight = 0.9 ^ (how many updates back the later one is from the most recent)
>
> Slip pace = weighted median of every pairwise rate, never reported below zero

This replaces a simpler adjacent-updates-only comparison – checking every pair, not just each update against the one right before it, means a genuine slow drift can no longer be hidden by an occasional push followed by a pull-back.

**The forecasted (central) finish date** starts from the reported finish date and shifts it forward by slip pace, capped so one bad reading can't push the forecast out by more than one and a half times however much programme time is left:

> Forecasted finish = reported finish + (slip pace × months remaining), capped at 1.5 × months remaining

**The earliest–latest range** is drawn evenly either side of that forecasted date – both bounds now use the same full half-width, with no lean toward later or earlier:

> Half-width (months) = the larger of *(months remaining × (0.15 + 0.25 × volatility score))* or *(2 + 3 × volatility score)*, then narrowed by an update-count adjustment – the square root of (6 ÷ number of updates used, or 2 if there are fewer than 2 updates)
>
> Early bound = forecasted shift − half-width, never earlier than the reported finish date itself
> Late bound = forecasted shift + half-width

**The volatility score** combines three of the six metrics above, each scaled onto a comparable range and weighted so date bounce counts for the most, plan growth next, and schedule pressure least, using each metric's recent-window reading:

> Volatility score = 0.5 × (Date bounce ÷ 3) + 0.3 × (Meaningful plan growth ÷ 40, floored at 0) + 0.2 × (Schedule pressure ÷ 100), capped at 1.2

**Turning the range into a percentage:** the confidence percentage compares the width of the earliest–latest range against how much time is left on the project – a range that's narrow relative to the time remaining reads as high confidence, a wide one reads as low confidence:

> Confidence % = 100 × (1 − (width of the range ÷ (months remaining × 0.8 + 8))), rounded, and held between 5% and 95%

The `+ 8` is a fixed allowance built into that comparison, so a project with almost no time left doesn't automatically look highly confident just because there's barely anything left to slip. The confidence band is **70% or above is green, 40–69% is amber, below 40% is red.**

**The six metrics, in detail:**

| Metric | Calculation | Green / amber / red |
|---|---|---|
| Slip pace | The weighted pairwise drift rate described above, in months of drift per month elapsed. | below 0.4 / 0.4–0.7 / above 0.7 |
| Date bounce | The standard deviation of the month-to-month change in finish-date drift, over the last 6 update-to-update steps. | below 1 / 1–2 / above 2 (months) |
| Meaningful plan growth | Activity-count growth per WBS branch between the first and latest update, weighted toward branches carrying more of the remaining work, then reduced by the mitigation ratio below (so growth that's actively being absorbed reads lower). | below 10% / 10–25% / above 25% |
| Mitigation ratio | Averaged over the last 3 updates: working days of finish dates pulled earlier, divided by working days pushed later, capped at 1 per update (an update with nothing pushed later scores fully mitigated). Unlike the other five, a *higher* mitigation ratio is better. | below 15% / 15–40% / above 40% (good) |
| Schedule pressure | The worst share of incomplete activities sitting on negative float, across the last 3 updates. | below 10% / 10–40% / above 40% |
| Reporting depth | The number of updates the read is built from, after the coherence check above. | below 4 / 4–8 / above 8 updates |

## Note

At least two schedule updates sharing a common completion milestone, at least half a month apart, are needed to produce a read. With fewer, no signal is shown – Logic+ explains what's missing (e.g. needing a wider gap between updates) rather than showing a misleading number.
