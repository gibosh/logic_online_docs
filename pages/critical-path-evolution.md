---
page-id: critical-path-evolution
route: /module/4/project/:projectId
title: Critical Path Evolution
audience: external
status: draft
version: 1.2.1
last-reviewed: 2026-09-25
blocked-reason: Fixed three references to "Gantt Viewer's Delay Analysis mode" (Delay Analysis is its own module now, not a mode of the Gantt Viewer) caught in the 2026-09-25 sitewide drift audit – this page predated the 2026-09-24 module-framing correction and was missed by it since it's an Analytics page, not one of the Delay Analysis pages themselves. Content below is written and corrected from direct code analysis (frontend/src/analytics/impactWindows/, frontend/src/components/modules/CriticalPathEvolutionModule/) but has not yet been checked against the running app. Confirm chart interaction, hover behaviour, and on-screen wording before promoting to complete.
---

## Overview

At any point in a project, one activity, or a chain of activities, drives the finish date: if it slips further, the finish date moves. Which activity that is can change between reporting periods, sometimes without anyone noticing.

Critical Path Evolution shows which activity drove the project finish date in each reporting period, whether a different activity took over between updates, and how much of the delay at each period was newly caused there compared with delay carried forward from earlier in the path.

It uses the same day-by-day delay maths as [Delay Analysis](pages/delay-analysis.md), but not that module's predecessor-based traceback. For each pair of schedule updates, it builds its own driving chain from the activities flagged critical in the newer update, then runs that chain through the same delay-attribution calculation. "How the driving chain is built" below explains what this means in practice.

## Report details

There are two views, each on its own tab.

**Impact Windows Analysis** has one row per pair of consecutive schedule updates (a "window"). Each row shows the activity or chain that drove the finish-date movement in that window, split into the part already behind the data date (actual) and the part still ahead of it (prospective). Click a window row to expand it into its activities, shown as baseline-against-current movement bars: the faint bar is the previous update, the solid bar this update, coloured differently before and after the data date. Click again to collapse it. Only one window can be expanded at a time.

**WBS timeline** lays the same driving-path history across the full reporting period, grouped by work area (the schedule's highest-level WBS breakdown), so you can see which parts of the project have held the critical path over time. Click one schedule row to see its activity-level path, or click up to three rows to overlay them and compare which activities they share and where they differ. Selecting a fourth row drops the oldest selection.

## Calculation and other logic

**Data used:** every uploaded schedule update, plus each activity's critical-path status and float.

**How a window boundary is defined:** the uploaded schedule updates are sorted by data date, and a "window" is the gap between one update and the next. N uploaded updates give N−1 windows, so a project with one update has none.

**How the driving chain is built:** for each window, Logic+ takes every non-summary activity flagged critical in the newer schedule and sorts them by start date. "Critical" means zero float for an activity that had already finished by that schedule's data date (using the same float calculation as the rest of the app), or P6's own critical flag for an activity that hadn't finished. Both tests are needed because P6 often stops reporting float once an activity completes. This is simpler than the predecessor-by-predecessor traceback in Delay Analysis: it doesn't walk back through logic links from a completion milestone, and if the schedule has more than one critical path, all of them go into one list in date order.

The chain is then trimmed to the part that changed since the previous window's data date, so unaffected early activities aren't recalculated for every window. Logic+ walks the chain from its earliest activity and moves the trim point past every activity that **both** finished before the window opened **and** shows no finish-date movement (within one day) from the previous update. Everything from the last of these settled activities onward is recalculated.

**How the delay is attributed:** the trimmed chain goes through the same activity-by-activity delay-attribution calculation as Delay Analysis. Each activity's own contribution is its finish-date variance minus the previous chain activity's, so an activity that only passes delay along contributes close to nothing, and only the activity that added delay is charged for it:

> An activity's own delay charge (working days) = round(its cumulative critical-path delay this window, including its own effect) − round(the previous chain activity's cumulative critical-path delay this window)

The driving activity in a window often hasn't started yet, so its charge is split by how much of its duration has elapsed at the data date. The elapsed part counts as actual and the rest as prospective:

> Elapsed fraction = (data date − activity's start date) ÷ (activity's finish date − activity's start date), held between 0 and 1
> Actual (already-elapsed) share of the charge = round(charge × elapsed fraction); Prospective (forecast) share = charge − actual share

An activity that has finished has its whole charge counted as actual, and one that hasn't started has its whole charge counted as prospective, so the elapsed-fraction formula applies only to activities in progress.

## Important

Most delay in this view is **forecast**: it's delay in activities that haven't started or finished yet, and it hasn't happened in the way that delay in completed work has. The display marks the difference so that forecast delay isn't given the same weight as delay that's already locked in. Bear this in mind if you use the view for a delay claim or dispute.
