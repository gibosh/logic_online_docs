---
page-id: getting-started
title: Getting Started with Logic+ Online
audience: external
status: draft
version: 1.6.0
last-reviewed: 2026-09-18
blocked-reason: Rewritten from code (left-hand nav structure, status labels, help-panel claim removed as unbuilt) but not yet walked through live — confirm before promoting to complete. Delay Analysis is now its own nav item with Traceback and Time-Impact Analysis as methods under it. All projects and Schedule Manager now open genuinely different screens (split shipped 2026-09-17) — see workspace/GAPS.md.
---

## What is Logic+ Online?

Logic+ Online is a web-based schedule analysis tool. You upload construction project schedule files (see [Supported file formats](#supported-file-formats) below) and Logic+ runs a suite of analytics to help you understand schedule health, delay risk, and critical path behaviour.

## The workflow

1. **Create a project** – in [All projects](pages/all-projects), add a project to group your schedule files (optionally organised into a group)
2. **Upload a schedule** – select the project, open [Schedule Manager](pages/schedule-manager), and drag in a schedule file (up to 100 MB)
3. **Wait for processing** – the status shows "Processing…" while Logic+ works through the file, then "Ready for analysis" once it's ready
4. **Explore the views** – once processed, use the left-hand navigation to move between modules

## Uploading multiple schedules

Upload more than one schedule to the same project to enable comparison views. For example, upload a baseline schedule and a current schedule to see how the project has shifted over time.

## Supported file formats

| Format | Source |
|--------|--------|
| `.xer` | Primavera P6 |
| `.mpp` | Microsoft Project |
| `.pp` | Asta Powerproject |
| `.xml` | MSPDI |


## Navigating the app

The left-hand navigation gives access to:

- **[Schedule Viewer](pages/gantt-viewer)** – Gantt chart, schedule comparison, and activity relationships
- **[Delay Analysis](pages/delay-analysis)** – choose a delay-analysis method: **Traceback** (trace which activities are driving project delay) or **[Time-Impact Analysis](pages/time-impact-analysis)** (build a hypothetical delay event and test its impact on Practical Completion)
- **[Analytics](pages/analytics-overview)** – schedule quality checks, bow wave compression, completion forecast, and more
- **[Schedule Manager](pages/schedule-manager)** – upload and manage the current project's schedule files
- **[All projects](pages/all-projects)** (below a divider) – create projects, organise them into groups, and choose which one you're working in

Not all views will show meaningful data unless a project with processed schedules is selected.

## Getting help

This documentation site is the current source of contextual help – there is no in-app help panel or "Learn about this page" link inside Logic+ itself yet. Use the navigation on the left, or the search box, to find the page for the view you're on.
