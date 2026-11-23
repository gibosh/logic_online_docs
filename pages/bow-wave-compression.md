---
page-id: bow-wave-compression
route: /module/5/project/:projectId
title: Bow-Wave Compression
audience: external
status: draft
version: 1.2.0
last-reviewed: 2026-09-08
blocked-reason: Content below is corrected from direct code analysis but has not yet been checked against the running app. Confirm chart interaction and hover/tooltip behaviour before promoting to complete.
---

## Overview

Work that should happen steadily over time sometimes gets pushed later and later, re-sequenced instead of resourced, until it piles up into an unrealistic crunch near the end of the project. By the time this shows in a normal Gantt view, it's often too late to add the crews, plant or subcontractor capacity needed to absorb it.

This module shows that shape forming: how many activities are scheduled to run at once, across the timeline and across schedule updates. Two panels below the chart compare updates side by side.

## Switching views

The buttons at the top of the page switch between four views, with a **Granularity** selector (Day, Week, Month or Year) alongside:

| View | What it shows |
|------|--------------|
| **Bow Wave** | Count of activities scheduled to be running on each day or period, for each uploaded schedule update, split before and after that schedule's data date |
| **Peak Concurrent** | The busiest moment in each period: the maximum number of activities running at the same time |
| **Project Progress** | A cumulative percent-complete curve built from planned start and finish dates |
| **Average Float** | The average total float across activities spanning each day or period |

A **Criticality filter** (All / Critical) next to the granularity selector applies to every view and to both panels below. "Critical" limits every chart and panel to the activities P6 flags as critical in each schedule update, before any of the calculations below run.

**Reading the shape:** a line that climbs steadily suggests work is spread evenly across the timeline. A line that stays flat early and rises steeply late is the bow wave: work has been deferred instead of resourced to run earlier. Logic+ shows the shape, but whether a given steepness is a real problem is a planning judgement; the chart doesn't score it.

**Project Progress note:** this curve is built from each activity's *planned* dates, not from what has been completed, so it shows the planned delivery shape rather than progress against it. For completed against planned, use the S-curve in [Float Burn-down & Earned Schedule](pages/float-burndown-earned-schedule.md).

## Panels

Two panels sit below the chart, and both appear once at least one schedule update is loaded.

**Schedule Summary panel**: key figures side by side for each schedule update in the project: work content, peak concurrent activities, completion window (length, in months) and completion end date. Each row is compared with either the previous update or a chosen baseline update (a toggle at the top of the panel), and the change shows as a coloured chip.

**Schedule Forensics panel**: compares pairs of schedule updates, either consecutive ones or each update against a chosen baseline, using the same toggle as the Schedule Summary panel. It always shows the baseline-to-latest comparison first. Below that, it shows only the one other comparison that scores highest on a combined measure of activity churn, relationship changes, completion-date movement, and changes in span, work and peak concurrency. Click **Show all updates** to list every other comparison in date order (not re-sorted by score). For each pair, the panel names a probable structural cause (activity re-sequencing, logic rewiring, scope change or a combination), counts relationships added, removed and retyped, gives an activity-churn percentage, and lists up to six "coordinated movements": activities whose start and finish moved together by a similar number of days. Use it to find where in the update history the biggest change happened and what kind of change most likely caused it.

## Calculation and other logic

**Data used:** every activity's planned start and finish dates in every uploaded schedule update, plus float and critical-path flags for the views that need them.

**How it's calculated:** each of the four views is calculated independently, and they don't all count "active" the same way. All four use each activity's *planned* start and finish dates, not its actual dates.

**Bow Wave** counts, for each calendar day (weekends included), how many activities span it:

> Activities running on a day = count of activities whose planned start ≤ that day ≤ planned finish

**Peak Concurrent** is more precise than a daily count. For each day, every activity's span is clipped to that day, and the day's value is the largest number of activities overlapping at any single moment within it. Two activities that each run for half the day but never overlap give a peak of 1, not 2.

**Project Progress** builds a cumulative curve, counted over **business days only** (Monday to Friday):

> A business day's activity count = number of activities whose planned start ≤ that business day ≤ planned finish
> Percent complete on a business day = (running total of that count, business days only, from the project's earliest planned start up to and including that day ÷ the same total across the whole planned span) × 100, rounded to two decimal places

It's a planned-volume curve, not a true percent-complete measure (see the Project Progress note above).

**Average Float** takes the mean across activities that have a float value for that day:

> A day's average float = mean of the float values (days) of every activity spanning that calendar day and carrying a resolvable float

Activities without a float value are left out of the average instead of counting as zero, and an activity's float counts on every calendar day it spans.

**Schedule Forensics combined score:** the comparison highlighted by default, before you click **Show all updates**, is the one that scores highest on:

> Combined change score = (2 × activity-churn %) + (10 × number of relationships added, removed, or retyped) + |days the completion date moved| + |% change in completion-window length| + |% change in total work content| + |% change in peak concurrency|

A percentage change from an earlier value to a later one is ((later − earlier) ÷ earlier) × 100, treated as 0 when the earlier value is 0. The score only chooses which comparison to show by default. It doesn't measure the largest finish-date shift, and it doesn't directly detect logic or sequencing changes between the two schedules. After you click **Show all updates**, the other comparisons are listed in date order, not ranked by this score.
