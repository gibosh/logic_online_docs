---
page-id: float-burndown-earned-schedule
route: /module/9/project/:projectId
title: Float Burn-down and Earned Schedule
audience: external
status: draft
version: 1.1.0
last-reviewed: 2026-07-20
blocked-reason: Content below matches the component source directly (including its own in-app explanatory copy), but has not yet been checked against the running app. Confirm chart interaction/hover behaviour before promoting to complete.
---

## Why this page exists

There are two well-established ways to estimate when a project will really finish — and each one can be individually misleading.

- **Float burn-down** tracks how much protective schedule buffer (float) is being consumed over time. It can look artificially healthy when a planner re-sequences activities to protect float on paper without genuinely recovering progress.
- **Earned schedule** tracks how much work has been completed against the plan. It can look artificially healthy when the real delay is hiding in work that has not yet started — future work absorbs the risk, not past work.

Showing either one alone risks a misleading picture. Showing both side by side — deliberately framed as an optimistic bound and a pessimistic bound — means that when the two methods disagree, that gap is itself the important finding: a signal that something in the schedule is hiding risk that at least one of the methods is not seeing.

## What you will see

Three summary cards, then two charts.

**Summary cards:**

| Card | Shows |
|---|---|
| Earned Schedule SPI(t) | The schedule performance index — e.g. "72% complete, at 91% of planned pace" |
| S-curve finish (work-rate) | The earned-schedule projected finish date, with a range — labelled the **optimistic bound** |
| Float-implied finish (critical path) | The float burn-down projected finish date, with a range — labelled the **pessimistic bound** |

**Float burn-down chart** — tracks the median total float of the most-critical 10% of incomplete activities across each schedule update, projected forward with a trend line.

**Progress S-curve chart** — planned vs actual completions against the earliest baseline, with remaining work projected along the baseline's own shape.

**Reading the two together:** where the S-curve finish and the float-implied finish agree, that's a consistent signal. Where they diverge, the gap itself is the finding — it means delay is hiding in work that hasn't started yet (invisible to the float-based view) or float is being spent to protect dates on paper without real progress (invisible to the earned-schedule view).

**Note:** projections need at least 3 processed schedule updates for this project — with fewer, only the observed data is shown, no trend line.
