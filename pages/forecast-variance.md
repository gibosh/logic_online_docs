---
page-id: forecast-variance
route: /module/2/project/:projectId
title: Forecast Variance
audience: external
status: complete
version: 1.1.1
last-reviewed: 2026-09-08
---

A finish date can move for two different reasons, and they point to different people and different next steps:

- **Duration growth**: an activity's own duration grew, so its finish moved but its start didn't. This is a productivity signal: the work took longer than planned.
- **Timing shift**: an activity's start date moved, carrying its finish with it. This can be re-sequencing, a knock-on from an upstream activity, a calendar change or a manual date override. The report measures *that the start moved*, not *why*.

Knowing only that the finish date moved doesn't tell you which of these caused it, or where in the project. Forecast Variance separates the two and shows, by work area, which parts of the project are tracking to plan, which are being reworked, and which are volatile, changing a lot between updates with no offsetting recovery.

The report compares the **two most recent schedule updates** for the project. Everything except the bottom trend chart describes that latest window. With fewer than two updates uploaded, there's nothing to compare and the module shows an empty state.

## Report details

The report has four sections, in this order.

**Forecast Reliability** is a headline score out of 100, shown as a plain number at the top of the page with no section heading of its own. It's built from how much of the schedule moved between the last two updates, how far it moved, how widely across the project, and how much of any slip was pulled back.

**Where the Change Sits** is a diverging bar chart with one bar per WBS area. Each bar shows the area's own **originated** delay to the right of the zero axis and its recovery to the left, with the busiest areas first. Each area is classified **Reliable**, **Active rework** or **Volatile** (see Calculation below). Each row also shows `+detail` (activities added to the area since the last update) and `+logic` (net predecessor links added) beside the bar. Adding detail or tightening logic often improves a schedule, so these are shown separately and don't count toward the score.

**Forecast Accuracy vs Forecast Change** compares, in grouped bars, two things that must be kept apart. **Retrospective accuracy** (solid bar) covers activities that have since completed: how far the forecast finish was from the actual finish, which is verified because it has happened. **Prospective change** (hollow bar) covers activities still open: how far the forecast has since moved, which is unverified. The bars are grouped by how far ahead of the baseline data date each activity was forecast to finish.

**Is Forecasting Improving?** is the only chart that covers the project's full history instead of the latest two updates. It plots the mean forecast error, measured only against activities that actually finished, to show whether the schedule's forecasts have become more or less accurate over the project.

## Calculation and other logic

**Data used:** matched activities only, meaning activities present in both of the two schedule updates, matched by activity ID. An activity present in only one update has no baseline to compare against, so it isn't scored, but it still counts toward its area's structural change through `+detail`. WBS summary rows are excluded throughout, because their dates are structural, not real. All day figures are whole working days on a P6 calendar: the activity's own calendar for activity-level maths, and the project's default calendar for programme-level maths.

**Forecast Reliability score:** for each matched activity, the finish movement is split into the only two things that can cause it:

> Finish variance = current finish date − baseline finish date
> Start variance = current start date − baseline start date (zero if either date is missing)
> Duration variance = finish variance − start variance

The split is exact, because start variance and duration variance always add up to the finish variance. An activity whose finish moved but whose start didn't has run long, and one whose start and finish moved together has been re-sequenced.

The 0–100 score is:

> Reliability score = round(100 × (1 − penalty)), or 0 outright if the two updates share no matched activities at all
>
> penalty = 0.35 × (share of matched activities that moved at all) + 0.30 × (average slip size ÷ length of the reporting window, capped at 100%) + 0.20 × (share of work areas that moved) + 0.15 × (how little of any slip was recovered)

100 means nothing moved. 0 means the whole schedule moved by a full window's worth of time, everywhere, with nothing pulled back. The "how little was recovered" part applies only where the schedule actually lost time. It's one minus the recovered share (time pulled back ÷ total time pushed out and pulled back), measured against an even 50/50 split, so a programme that lost no time isn't penalised for having nothing to recover. Two updates that share no matched activities score 0, not 100, meaning they can't be checked.

**Where the Change Sits, originated charge:** if activity A slips 10 days and its successor B slips the same 10 days only because A pushed it, B caused nothing. Charging both would count the same delay twice and spread one root cause across the whole downstream chain, so each activity is charged only for its own movement:

> Originated charge = activity's own finish variance − its driving predecessor's finish variance

The driving predecessor is the predecessor with the least free float, with ties broken by the lowest internal activity ID. This gives each activity one driver, so a pure downstream knock-on nets to zero and only the activity that caused the movement is charged.

An area is **Reliable** if it didn't move, or if its total movement (delay plus recovery) is below 10% of its own working-day content, a threshold that scales with the area's size. Otherwise it's **Active rework** if delay makes up 40–60% of that movement, roughly balanced against recovery, or **Volatile** if the split is more one-sided. "WBS area" means the first named WBS summary level with more than one sibling, which may not be the top level of the WBS if a project's structure doesn't branch that high.

**Forecast Accuracy vs Forecast Change, horizon bands:** each activity goes into a band by how far ahead of the baseline data date it was forecast to finish:

> Months ahead = working time between the baseline data date and the activity's baseline finish date ÷ the project calendar's own minutes-per-month value

Using the project calendar instead of an assumed 30-day month means the bands mean the same thing whichever calendar a project runs on. The bands are `0–1 mo`, `1–3 mo`, `3–6 mo`, `6–12 mo` and `12 mo+`, each including its upper limit, so exactly 1 month falls in `0–1 mo`. Activities already due or overdue at the baseline data date fall outside these bands and are currently left out (see Note below).

Each bar and its signed label are built the same way, whether the activity is retrospective or prospective:

> Day difference for one activity = −(outcome finish date − baseline finish date), so a late finish reads as a negative number
> Bar height for a band = average of the absolute day differences across that band's activities
> Signed mean shown alongside = the plain (non-absolute) average of the same day differences, showing whether the bias runs early or late

Bar heights use absolute values so that early and late errors can't cancel out and look accurate.

**Is Forecasting Improving?, trend:** unlike the other three sections, this one looks at every consecutive pair of updates across the project's full history.

> Forecast error for one activity in one window = |actual finish date − the earlier update's forecast finish date|, in working days
> Mean forecast error for a window = average of that error across every activity that finished during the window

Activities already finished at the earlier update are excluded, because their outcome was already known. Windows in which nothing finished are left off the chart instead of showing as zero. The "improving?" reading compares only the first and last points plotted, not a fitted trend line, and needs at least two plotted windows to show a direction.

## Note

This is a known product issue, not a problem with your schedule: activities already due or overdue at the earlier update's data date are left out of the Forecast Accuracy vs Forecast Change chart instead of being measured, so the chart can under-represent near-term activities. The 1, 3, 6 and 12-month band boundaries themselves are handled correctly.

"Timing shift" in the overview is measured as "did the activity's start date move", not as "did a logic relationship change". A start date can also move because of a calendar change or a manual date override, so treat a timing shift as a close approximation of a logic change, not a direct detection of edited relationships.
