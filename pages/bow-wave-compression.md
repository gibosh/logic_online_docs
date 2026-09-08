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

## Bow-Wave Compression

Work that should happen steadily over time sometimes gets pushed later and later – re-sequenced rather than genuinely resourced – until it piles up into an unrealistic crunch near the end of the project. By the time this is visible in a normal Gantt view, it is often too late to add the crews, plant, or subcontractor capacity needed to absorb it.

This module gives you the tools to see that shape forming: how many activities are scheduled to run at once, across the timeline and across schedule updates, plus two panels for comparing updates side by side.

---

## Switching views

Four views are available via the buttons at the top of the page, with a **Granularity** selector (Day, Week, Month, or Year) alongside:

| View | What it shows |
|------|--------------|
| **Bow Wave** | Count of activities scheduled to be running on each day or period, for each uploaded schedule update, split before and after that schedule's data date |
| **Peak Concurrent** | The busiest moment in each period – the maximum number of activities running at the same time |
| **Project Progress** | A cumulative percent-complete curve built from planned start and finish dates |
| **Average Float** | The average total float across activities spanning each day or period |

A **Criticality filter** (All / Critical) sits alongside the granularity selector and applies to every view, plus both panels below: "Critical" restricts every chart and panel to only the activities P6 itself flags as critical for that schedule update, before any of the calculations below run.

**Reading the shape:** a line that climbs steadily suggests work is spread evenly across the timeline. A line that stays flat early and rises steeply late is the bow wave – work has been deferred rather than resourced to run earlier. Logic+ shows you this shape; deciding whether a given steepness is a real problem is a planning judgement, not something the chart scores for you.

**Project Progress note:** this curve is built from each activity's *planned* dates, not from what has actually been completed – it shows the planned delivery shape, not progress against it. For a completed-vs-planned comparison, use the S-curve on the [Float Burn-down and Earned Schedule](float-burndown-earned-schedule.md) module instead.

---

## Panels

Two panels sit below the chart, both appearing once at least one schedule update is loaded.

**Schedule Summary panel** – key stats side by side for each schedule update loaded for the project: work content, peak concurrent activities, completion window (length, in months), and completion end date. Each row is compared against either the previous update or a chosen baseline update (toggle at the top of the panel), with the change shown as a coloured delta chip.

**Schedule Forensics panel** – compares pairs of schedule updates (consecutive, or each update against a chosen baseline – same toggle as the Schedule Summary panel). It always shows the baseline-to-latest comparison first. Below that, it shows just the single other comparison that scores highest on a combined measure of activity churn, relationship changes, completion-date movement, and span/work/peak-concurrency swings – until a "Show all updates" button is clicked, which lists every other comparison in date order (not re-sorted by that score). For each pair it names a probable structural cause (activity re-sequencing, logic rewiring, scope change, or a combination), counts relationships added/removed/retyped, gives an activity-churn percentage, and lists up to six "coordinated movements" – activities whose start and finish shifted together by a similar number of days. Use this to find where in the update history the biggest swing happened and what kind of change most likely drove it.

## Calculation and other logic

**Data used:** every activity's planned start and finish dates across every uploaded schedule update, plus float and critical-path flags for the relevant views.

**How it's calculated:** the four views are calculated independently rather than being different slices of one shared number, and they don't all count "active" the same way. All four use each activity's *planned* start and finish dates (not its best-available or actual dates).

**Bow Wave** counts, for each calendar day (weekends included), how many activities span it:

> Activities running on a day = count of activities whose planned start ≤ that day ≤ planned finish

**Peak Concurrent** goes a level more precise than a daily headcount: for each day, every activity's span is clipped to that day's boundaries, and the day's value is the highest number of activities overlapping at any single instant within it – not just how many activities touch the day, but the busiest moment inside it. Two activities that each run for half the day but never overlap read as a peak of 1, not 2.

**Project Progress** builds a cumulative curve, but counted over **business days only** (Monday to Friday):

> A business day's activity count = number of activities whose planned start ≤ that business day ≤ planned finish
> Percent complete on a business day = (running total of that count, business days only, from the project's earliest planned start up to and including that day ÷ the same total across the whole planned span) × 100, rounded to two decimal places

This is a planned-volume curve, not a true percent-complete measure – see the Project Progress note above.

**Average Float** takes the mean across activities with a resolvable float value for that day:

> A day's average float = mean of the float values (days) of every activity spanning that calendar day and carrying a resolvable float

Activities with no calculable float are left out of the average rather than counted as zero, and a task's float value is repeated across every calendar day it spans.

**Schedule Forensics combined score:** the single comparison highlighted by default (the one shown before "Show all updates" is clicked) is whichever pairing scores highest on:

> Combined change score = (2 × activity-churn %) + (10 × number of relationships added, removed, or retyped) + |days the completion date moved| + |% change in completion-window length| + |% change in total work content| + |% change in peak concurrency|

where a percentage change from an earlier value to a later one is ((later − earlier) ÷ earlier) × 100 (treated as 0 when the earlier value is 0). This is not a single "largest finish-date shift" measure, and it is not a direct detector of logic or sequencing changes between the two schedules – it only picks which one comparison is worth surfacing by default. Once "Show all updates" is clicked, the rest of the comparisons are listed in date order, not re-ranked by this score.
