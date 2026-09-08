---
page-id: float-burndown-earned-schedule
route: /module/9/project/:projectId
title: Float Burn-down
audience: external
status: draft
version: 1.2.1
last-reviewed: 2026-09-08
blocked-reason: Re-verified 2026-09-08 against current source (frontend/src/analytics/ProjectFloatCheck/earnedSchedule.ts, floatBurnDown.ts, theilSen.ts; frontend/src/components/ProjectFloatCheck/floatProjectionCone.ts re-read fresh after a same-day upstream pull touched its chart-rendering neighbours) — calculation logic unchanged, formulas added below match the current code. Still not checked against the running app. Confirm chart interaction/hover behaviour before promoting to complete.
---

## Float Burn-down

There are two well-established ways to estimate when a project will really finish – and each one can be individually misleading.

- **Float burn-down** tracks how much protective schedule buffer (float) is being consumed over time. It can look artificially healthy when a planner re-sequences activities to protect float on paper without genuinely recovering progress.
- **Earned schedule** tracks how much work has been completed against the plan. It can look artificially healthy when the real delay is hiding in work that has not yet started – future work absorbs the risk, not past work.

Showing either one alone risks a misleading picture. Showing both side by side – deliberately framed as an optimistic bound and a pessimistic bound – means that when the two methods disagree, that gap is itself the important finding: a signal that something in the schedule is hiding risk that at least one of the methods is not seeing.

## Report details

Three summary cards, then two charts.

**Summary cards:**

| Card | Shows |
|---|---|
| Earned Schedule SPI(t) | The schedule performance index – e.g. "72% complete, at 91% of planned pace" |
| S-curve finish (work-rate) | The earned-schedule projected finish date, with a range – labelled the **optimistic bound** |
| Float-implied finish (critical path) | The float burn-down projected finish date, with a range – labelled the **pessimistic bound** |

**Float burn-down chart** – tracks the median total float of the most-critical 10% of incomplete activities across each schedule update, projected forward with a trend line.

**Progress S-curve chart** – planned vs actual completions against the earliest baseline, with remaining work projected along the baseline's own shape.

**Reading the two together:** where the S-curve finish and the float-implied finish agree, that's a consistent signal. Where they diverge, the gap itself is the finding – it means delay is hiding in work that hasn't started yet (invisible to the float-based view) or float is being spent to protect dates on paper without real progress (invisible to the earned-schedule view).

**Note:** projections need at least 3 processed schedule updates for this project – with fewer, only the observed data is shown, no trend line.

## Calculation and other logic

**Data used:** every uploaded schedule update. The float-implied side uses each activity's calculated float and calendar. The earned-schedule side treats the earliest uploaded schedule as the baseline plan, and compares it against each later update's completion status, matched activity by activity.

**How it's calculated:** the two estimates are genuinely independent and are never blended into one number.

**Float-implied finish (pessimistic bound):** each schedule update contributes one data point – the median total float, in working days, of the worst 10% of incomplete activities (summary rows, milestones, and level-of-effort hammocks excluded, since their float is structural rather than real buffer). A trend line is fitted through these points using **Theil–Sen regression**: rather than an ordinary least-squares line, the slope is the median of the slopes between every possible pair of data points, and the intercept is the median of each point's offset from that slope – a method that resists being thrown off by a single outlier update (e.g. a one-off float reset), unlike a standard trend line.

> Trend slope = median of (float at update *j* − float at update *i*) ÷ (months between update *j* and update *i*), across every pair of updates *i* < *j*
> Projected float at schedule finish = trend slope × months from the first update to the current schedule finish + trend intercept

The range around that projected float comes from a statistical prediction interval, not a fixed percentage: it uses the **Student's t-distribution** critical value for an 80% interval (P10–P90), scaled by how much the residuals scatter around the trend line and widened further the more the finish date sits beyond the observed update history – so a projection that extrapolates a long way past the last update is shown as more uncertain than one close to it. The projected float (and its range) is then converted into a calendar lateness past the current schedule finish, treating each working day of negative float as 1.4 calendar days (7 ÷ 5, to cover weekends):

> Implied finish = schedule finish + (working days of negative float × 7⁄5), rounded to the nearest day

**S-curve finish (optimistic bound):** the Earned Schedule method. The baseline upload's own activities are turned into a planned-completion curve – the percentage of baseline activities scheduled to be complete by each working day from project start. "Earned schedule" is the point on that same curve where the planned percentage-complete matches how much is *actually* complete today – in effect, asking "at what point in the original plan does today's real progress belong?":

> Actual time (working days) = working days elapsed from project start to the current data date
> Earned schedule (working days) = the working-day point on the baseline's planned-completion curve where the planned percentage-complete equals today's actual percentage-complete
> SPI(t) = earned schedule ÷ actual time

An SPI(t) of 1.0 means progress is exactly on the baseline's own pace; below 1.0 means progress is running slower than the plan assumed. The remaining baseline work is then projected forward at that same pace, along the baseline curve's own shape, to estimate a finish date – with a faster/slower pair of curves (SPI(t) ± 0.08) shown either side as the range:

> S-curve finish = current data date + (remaining baseline working days ÷ SPI(t)), converted to a calendar date
> Early/late bounds use the same formula with SPI(t) + 0.08 and SPI(t) − 0.08 in place of SPI(t)

Both are shown as separate cards and charts, deliberately labelled optimistic and pessimistic bounds rather than combined into a single figure – so a gap between them is visible as itself the finding, not averaged away.
