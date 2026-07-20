---
page-id: bow-wave-compression
route: /module/5/project/:projectId
title: Bow-Wave Compression
audience: external
status: draft
version: 1.0.0
last-reviewed: 2026-07-20
---

## Bow-Wave Compression — Overview

Work that should happen steadily over time sometimes gets pushed later and later — re-sequenced rather than genuinely resourced — until it piles up into an unrealistic crunch near the end of the project. By the time this is visible in a normal Gantt view, it is often too late to add the crews, plant, or subcontractor capacity needed to absorb it.

This module gives you the tools to see that shape forming: how many activities are scheduled to run at once, across the timeline and across schedule updates, plus a set of panels for comparing updates and tracking float and the critical path alongside it.

---

## Switching views

Four views are available via the buttons at the top of the page, with a **Granularity** selector (Day, Week, Month, or Year) alongside:

| View | What it shows |
|------|--------------|
| **Bow Wave** | Count of activities scheduled to be running on each day or period, for each uploaded schedule update, split before and after that schedule's data date |
| **Peak Concurrent** | The busiest moment in each period — the maximum number of activities running at the same time |
| **Project Progress** | A cumulative percent-complete curve built from planned start and finish dates |
| **Average Float** | The average total float across activities spanning each day or period |

**Reading the shape:** a line that climbs steadily suggests work is spread evenly across the timeline. A line that stays flat early and rises steeply late is the bow wave — work has been deferred rather than resourced to run earlier. Logic+ shows you this shape; deciding whether a given steepness is a real problem is a planning judgement, not something the chart scores for you.

**Project Progress note:** this curve is built from each activity's *planned* dates, not from what has actually been completed — it shows the planned delivery shape, not progress against it. For a completed-vs-planned comparison, use the S-curve on the [Float Burn-down and Earned Schedule](float-burndown-earned-schedule.md) module instead.

---

## Panels

Four panels sit alongside the chart. Schedule Summary and Schedule Explorer appear once at least one schedule update is loaded; Critical Path Explorer and Float Explorer need at least two.

**Schedule Summary panel** — key stats side by side for each schedule update loaded for the project.

**Schedule Explorer panel** — compares consecutive pairs of schedule updates. It highlights the pair with the single largest change in projected finish date, and lists the individual activities within that pair whose start date, finish date, or duration shifted the most. Use this to find where in the update history the biggest swing happened, and which activities moved the most at that point — not as a detector of logic changes themselves.

**Critical Path Explorer panel** — compares the critical path between consecutive schedule updates: which activities are new to the critical path, which have dropped off it, and which changed duration while remaining on it.

**Float Explorer panel** — shown when the **Average Float** view is active. A searchable, sortable table of every activity's float for a selected schedule update, with a total and average shown in the footer. Use this to look up float at the individual-activity level behind the Average Float chart.
