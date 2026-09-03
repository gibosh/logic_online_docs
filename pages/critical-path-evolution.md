---
page-id: critical-path-evolution
route: /module/4/project/:projectId
title: Critical Path Evolution
audience: external
status: draft
version: 1.0.37
last-reviewed: 2026-09-03
blocked-reason: Content below is written and corrected from direct code analysis (frontend/src/analytics/impactWindows/, frontend/src/components/modules/CriticalPathEvolutionModule/) but has not yet been checked against the running app. Confirm chart interaction, hover behaviour, and on-screen wording before promoting to complete.
---

## Critical Path Evolution – Overview

At any point in a project, one activity (or chain of activities) is actually in the driver's seat – the thing that, if it slips further, directly moves the project finish date. But that driver's seat can change hands between reporting periods, sometimes without anyone explicitly noticing.

Critical Path Evolution shows which activity was driving the project finish date in each reporting period, whether that role passed to a different activity between updates, and how much of the delay shown at each period was newly caused there versus carried forward from an earlier part of the path.

In plain terms: who was in the driver's seat, and when did the wheel change hands.

This shares its day-by-day delay math with the Gantt Viewer's Delay Analysis mode, but it does not call that mode's predecessor-based traceback. Instead, for each pair of schedule updates it builds its own driving chain from whichever activities are flagged critical in the newer update, then runs that chain through the same delay-attribution calculation. See "How it's calculated" below for what this means in practice.

## Report details

Two views, selected by tab:

**Impact Windows Analysis** – one row per pair of consecutive schedule updates (a "window"), showing the activity or chain that drove the finish-date movement in that window, split into the portion already behind the data date (actual) versus the portion still ahead of it (prospective). Click a window row to expand it into the underlying activities as baseline-versus-current movement bars (ghost = previous update, solid = this update, coloured before/after the data date); click again to collapse. Only one window can be expanded at a time.

**WBS timeline** – the same driving-path history laid out across the full reporting period, grouped by work area (the schedule's own highest-level WBS breakdown), so you can see which parts of the project have taken over the critical path over time rather than window by window. Click one schedule row for its activity-level path, or click up to three rows to overlay them and compare shared versus divergent activities; selecting a fourth row drops the oldest selection.

## Calculation and other logic

**Data used:** every uploaded schedule update, plus each activity's critical-path status and float.

**How the driving chain is built:** for each window, Logic+ takes every non-summary activity flagged critical in the newer schedule and sorts it chronologically by start date. "Critical" here means zero float for an activity that has already finished by that schedule's data date (using the same float resolver as the rest of the app), or P6's own critical flag for an activity that hasn't finished yet – P6 commonly stops reporting float once an activity completes, so the two checks are needed to cover the whole chain. This is a simpler method than the full predecessor-by-predecessor traceback the Gantt Viewer's Delay Analysis mode uses: it does not walk backward through logic links from a completion milestone, and if the schedule genuinely has more than one critical path, all of it is included as one time-ordered list rather than a single walked chain. The chain is then trimmed to just the portion that changed since the previous window's data date, so unaffected early activities aren't recomputed for every window.

**How the delay is attributed:** that trimmed chain is run through the same activity-by-activity delay-attribution calculation as Gantt Viewer's Delay Analysis mode: each activity's own contribution is its finish-date variance minus the previous chain activity's, so a pure pass-through activity contributes close to nothing and only the activity that actually added delay is charged for it. Because the driving activity in a window is often one that hasn't started yet, its charge is split by how much of its duration has already elapsed: the elapsed portion counts as actual, the remainder as prospective.

## Important

Most delay shown in this view is **forecast** – it represents delay in activities that have not yet started or finished. It has not yet happened in the way that completed-work delay has. The display makes this distinction explicit so that forecast delay is not read with the same weight as delay that is already locked in. This matters when this view is used in a delay-claim or dispute context.
