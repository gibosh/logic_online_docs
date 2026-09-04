---
page-id: bow-wave-compression
route: /module/5/project/:projectId
title: Bow-Wave Compression
audience: external
status: draft
version: 1.0.2
last-reviewed: 2026-09-04
blocked-reason: Content below is corrected from direct code analysis but has not yet been checked against the running app. Confirm chart interaction and hover/tooltip behaviour before promoting to complete.
---

## Bow-Wave Compression – Overview

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

**Schedule Forensics panel** – compares pairs of schedule updates (consecutive, or each update against a chosen baseline – same toggle as the Schedule Summary panel). It always shows the baseline-to-latest comparison first, then – on request via a "Show all updates" button – every other comparison, ranked by a combined score of activity churn, relationship changes, completion-date movement, and span/work/peak-concurrency swings. For each pair it names a probable structural cause (activity re-sequencing, logic rewiring, scope change, or a combination), counts relationships added/removed/retyped, gives an activity-churn percentage, and lists up to six "coordinated movements" – activities whose start and finish shifted together by a similar number of days. Use this to find where in the update history the biggest swing happened and what kind of change most likely drove it.

## Calculation and other logic

**Data used:** every activity's planned start and finish dates across every uploaded schedule update, plus float and critical-path flags for the relevant views.

**How it's calculated:** the four views are calculated independently rather than being different slices of one shared number, and they don't all count "active" the same way. Bow Wave counts how many activities are active on each calendar day, including weekends. Peak Concurrent Tasks uses actual activity timestamps rather than a simple daily count, so it captures the true busiest moment within a day, not just which activities touch that day. Project Progress is calculated over **business days only** (Monday to Friday) rather than calendar days. Average Float is a mean across activities with a resolvable float value for that day – activities with no calculable float are left out of the average rather than counted as zero, and a task's float is repeated across every calendar day it spans. In the Schedule Forensics panel, the pair shown by default is whichever comparison scores highest on a combined measure of activity churn, relationship changes, completion-date movement, and span/work/peak-concurrency swings – not a single "largest finish-date shift" measure, and not a direct detector of logic or sequencing changes between the two schedules.
