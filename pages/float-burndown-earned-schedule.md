---
page-id: float-burndown-earned-schedule
route: /module/9/project/:projectId
title: Float Burn-down
audience: external
status: draft
version: 1.3.0
last-reviewed: 2026-09-22
blocked-reason: Corrected 2026-09-22 – the float-implied finish formula was documented as a fixed 1.4x (7/5) weekend conversion; commit 0f34cc92 (2026-09-10, "project through imported working calendars") replaced that with walking the projected negative float forward through the project's own imported calendar, so the old formula gives a materially wrong date on any 6- or 7-day construction calendar. Also added a previously-undocumented caveat: earned schedule matches by activity_id against the baseline set, so scope added after baseline is invisible to SPI(t) in either direction. Two stale code comments describing the old "business days Mon–Fri" assumption (README.md and earnedSchedule.ts's docblock in frontend/src/analytics/ProjectFloatCheck/) were also fixed in the app repo, separately from this docs change. Still not checked against the running app. Confirm chart interaction/hover behaviour before promoting to complete.
---

## Overview

There are two established ways to estimate when a project will really finish, and each can mislead on its own:

- **Float burn-down** tracks how much of the schedule's buffer (float) is being used up over time. It can look healthy when a planner re-sequences activities to protect float on paper without recovering any progress.
- **Earned schedule** tracks how much work has been completed against the plan. It can look healthy when the real delay sits in work that hasn't started yet, because future work carries the risk instead of past work.

This analytic shows both side by side, as an optimistic and a pessimistic bound. When the two methods disagree, the schedule is probably hiding risk that one of them can't see.

## Report details

Three summary cards sit above two charts.

**Summary cards:**

| Card | Shows |
|---|---|
| Earned Schedule SPI(t) | The schedule performance index, e.g. "72% complete, at 91% of planned pace" |
| S-curve finish (work-rate) | The earned-schedule projected finish date, with a range, labelled the **optimistic bound** |
| Float-implied finish (critical path) | The float burn-down projected finish date, with a range, labelled the **pessimistic bound** |

**Float burn-down chart**: tracks the median total float of the most critical 10% of incomplete activities in each schedule update, projected forward with a trend line.

**Progress S-curve chart**: planned against actual completions, measured against the earliest baseline, with the remaining work projected along the baseline's own shape.

**Reading the two together:** where the S-curve finish and the float-implied finish agree, the signal is consistent. Where they diverge, either delay is sitting in work that hasn't started (which the float view can't see), or float is being spent to protect dates on paper without real progress (which the earned-schedule view can't see).

**Note:** projections need at least 3 processed schedule updates for the project. With fewer, only the observed data shows, with no trend line.

## Calculation and other logic

**Data used:** every uploaded schedule update. The float side uses each activity's calculated float and calendar. The earned-schedule side treats the earliest uploaded schedule as the baseline plan and compares it with each later update's completion status, activity by activity.

**How it's calculated:** the two estimates are independent and never blended into one number.

**Float-implied finish (pessimistic bound):** each schedule update gives one data point: the median total float, in working days, of the worst 10% of incomplete activities. Summary rows, milestones and level-of-effort hammocks are excluded, because their float is structural, not real buffer. A trend line is fitted through these points with **Theil–Sen regression**. Instead of an ordinary least-squares line, the slope is the median of the slopes between every possible pair of points, and the intercept is the median of each point's offset from that slope. This makes the line resistant to a single outlier update, such as a one-off float reset.

> Trend slope = median of (float at update *j* − float at update *i*) ÷ (months between update *j* and update *i*), across every pair of updates *i* < *j*
> Projected float at schedule finish = trend slope × months from the first update to the current schedule finish + trend intercept

The range around the projected float comes from a statistical prediction interval, not a fixed percentage. It uses the **Student's t-distribution** critical value for an 80% interval (P10–P90), scaled by how much the points scatter around the trend line, and widened the further the finish date lies beyond the observed updates. A projection that reaches far past the last update therefore shows more uncertainty than one close to it. The projected float and its range are then turned into a date past the current schedule finish by counting that many working days forward on the **project's own imported working calendar**, so a 6-day or 7-day construction calendar lands on a different date from a standard 5-day week:

> Implied finish = schedule finish, walked forward the projected working days of negative float, through the project's own calendar

**S-curve finish (optimistic bound):** this uses the Earned Schedule method. The baseline upload's activities form a planned-completion curve: the percentage of baseline activities scheduled to be complete by each working day from project start. "Earned schedule" is the point on that curve where the planned percentage complete equals how much is *actually* complete today. In other words, it finds where today's real progress sits in the original plan:

> Actual time (working days) = working days elapsed from project start to the current data date
> Earned schedule (working days) = the working-day point on the baseline's planned-completion curve where the planned percentage-complete equals today's actual percentage-complete
> SPI(t) = earned schedule ÷ actual time

An SPI(t) of 1.0 means progress is exactly at the baseline's pace, and below 1.0 means progress is slower than planned. The remaining baseline work is then projected forward at that pace, along the baseline curve's shape, to estimate a finish date. A faster and a slower curve (SPI(t) ± 0.08) either side give the range:

> S-curve finish = current data date + (remaining baseline working days ÷ SPI(t)), converted to a calendar date
> Early/late bounds use the same formula with SPI(t) + 0.08 and SPI(t) − 0.08 in place of SPI(t)

**This side can also read optimistically for a second reason.** Actual progress is matched to the baseline by activity ID, and the baseline's activity count is the denominator throughout, so earned schedule can't see any activity added after the baseline was set: it's counted neither as remaining work nor as done. Scope added since the baseline doesn't move SPI(t) either way.

The two bounds are shown as separate cards and charts, not combined into one figure, so any gap between them stays visible.
