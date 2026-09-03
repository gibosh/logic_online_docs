---
page-id: forecast-variance
route: /module/2/project/:projectId
title: Forecast Variance
audience: external
status: complete
version: 1.0.37
last-reviewed: 2026-09-03
---

## Forecast Variance – Overview

When a project's finish date moves, there are two very different reasons it could have done so – and they have different implications for who is accountable and what needs to happen next.

- **Duration growth** – an activity's own duration grew, so its finish moved but its start did not. This is a productivity signal: the work itself took longer than planned.
- **Timing shift** – an activity's start date itself moved, carrying its finish with it. This can be genuine re-sequencing, a knock-on from an upstream activity, a calendar change, or a manual date override – the report measures *that the start moved*, not *why*.

Showing only that the finish date moved does not tell you which of these is responsible, or where in the project it is happening. Forecast Variance separates the two and shows, by work area, which parts of the project are reliably tracking to plan, which are being actively reworked, and which are genuinely volatile – changing significantly between updates with no offsetting recovery.

This report compares the **two most recent schedule updates** for the project – everything in it except the bottom trend chart describes that single latest window. With fewer than two updates uploaded, the module has nothing to compare and shows an empty state.

This moves the conversation from "the schedule slipped" to "here is specifically why, and where."

## Report details

Four sections, in order:

**Forecast Reliability** – a headline score out of 100 (shown as a plain number at the top of the page – there is no separate section heading for it on screen), built from how much of the schedule moved between the last two updates, how far, how widely across the project, and how much of any slip was pulled back.

**Where the Change Sits** – a diverging bar chart, one bar per WBS area, showing that area's own **originated** delay (right of the zero axis) and recovery (left). Rows are ordered busiest first. Each area is classified **Reliable**, **Active rework**, or **Volatile** (see Calculation, below). Each row also shows `+detail` (activities added to the area since the last update) and `+logic` (net predecessor links added) alongside the bar – adding detail or tightening logic is often a genuine schedule improvement rather than decay, so these are shown separately rather than folded into the score.

**Forecast Accuracy vs Forecast Change** – grouped bars comparing two things that must not be blended together: **retrospective accuracy** (solid bar – for activities that have since completed, how far the forecast finish was from the actual finish; this is verified, it already happened) against **prospective change** (hollow bar – for activities still open, how far the forecast has since moved; this is unverified, it hasn't happened yet), grouped by how far ahead of the baseline data date each activity was forecast to finish.

**Is Forecasting Improving?** – the only chart that spans the full project history rather than just the latest two updates. A trend line of mean forecast error, measured only against activities that actually finished, showing whether the schedule's forecasting has been getting more or less accurate as the project has progressed.

## Calculation and other logic

**Data used:** matched activities only – activities present in both of the two schedule updates being compared, matched by activity ID. An activity present in only one of the two updates has no baseline to compare against, so it isn't scored – but it is still counted, via `+detail`, in its area's structural change. WBS summary rows are excluded throughout, since their dates are structural rather than real. All day figures are whole working days on a P6 calendar (the activity's own calendar for activity-level maths, the project's default calendar for programme-level maths).

**Forecast Reliability score:** for each matched activity, `finishVariance = current finish − baseline finish`, `startVariance = current start − baseline start`, and `durationVariance = finishVariance − startVariance` – an exact decomposition that always sums back to the finish movement. The 0–100 score is `round(100 × (1 − penalty))`, where `penalty` combines four weighted, unit-free components: how much of the schedule moved at all (35%), how far it moved relative to the length of the reporting window (30%), how many work areas it touched (20%), and how little of any slip was recovered (15%). 100 means nothing moved; 0 means the whole schedule moved a full window's worth of time, everywhere, with nothing pulled back. Two updates that share no activities score 0, not 100 – that reads as "cannot be checked," not a clean bill of health.

**Where the Change Sits – originated charge:** if activity A slips 10 days and its successor B slips the same 10 days purely because A pushed it, B did not cause anything, so charging both would double-count and smear one root cause across the whole downstream chain. Each activity's *originated* charge is its own finish variance minus its **driving predecessor's** finish variance – the driving predecessor being whichever predecessor has the least free float (ties broken by lowest internal activity ID). This collapses the network into one driver per activity, so a pure downstream cascade nets to zero and only the activity that actually caused the movement is charged. An area is marked **Reliable** if its total movement is below 10% of its own working-day content (a floor that scales with the area's size, not an absolute day count); otherwise it's **Active rework** if delay and recovery are roughly balanced (a 40–60% split) or **Volatile** if the movement is lopsided in one direction with little offset. "WBS area" here means the first named WBS summary level that has more than one sibling – not necessarily the very top level of the WBS, if a project's structure doesn't branch that high up.

**Forecast Accuracy vs Forecast Change – horizon bands:** each activity is placed into a band by how many months ahead of the baseline data date it was forecast to finish (`0–1 mo`, `1–3 mo`, `3–6 mo`, `6–12 mo`, `12 mo+`, closed at the top so exactly 1 month lands in `0–1 mo`), measured on the project calendar's own month length rather than an assumed 30-day month. Activities already due or overdue at the baseline data date fall outside these bands and are currently dropped rather than measured – see Note, below. Each bar shows the mean *absolute* error in days, so early and late errors don't cancel into a false "accurate" reading; a signed mean shown alongside indicates whether the bias runs early or late.

**Is Forecasting Improving? – trend:** unlike the other three sections, this walks every consecutive pair of updates across the project's full history, not just the latest two. For each pair, it measures the error – in working days – between the earlier update's forecast finish and the actual finish, for every activity that finished during that window (activities already actualised as of the earlier update are excluded, since their outcome was already known and tests nothing new). Windows in which nothing finished are left off the chart rather than shown as zero. The "improving?" read compares only the first and last plotted points, not a fitted trend line, and needs at least two plotted windows to call a direction.

## Note

A known limitation, tracked as an open item in the code: activities that are already due or overdue at the earlier update's data date are dropped from the Forecast Accuracy vs Forecast Change chart rather than being measured, so that chart can under-represent near-term activities. This does not affect the 1/3/6/12-month band boundaries themselves, which are handled correctly. This is a tracked product issue, not a data-quality problem with your schedule.

The Overview above simplifies the underlying calculation's vocabulary slightly for readability: what's described here as a "timing shift" is measured in the calculation as "did the activity's start date move" – not "did a logic relationship change." A start date can also move because of a calendar change or a manual date override, not only because of re-sequencing, so treat this as a close approximation of logic change rather than a literal detector of edited relationships.
