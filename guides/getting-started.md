---
page-id: getting-started
title: Getting Started with Logic+ Online
audience: external
status: draft
version: 1.5.0
last-reviewed: 2026-09-15
blocked-reason: Rewritten from code (left-hand nav structure, status labels, help-panel claim removed as unbuilt) but not yet walked through live — confirm before promoting to complete. Delay Analysis is now its own nav item with Traceback and Time-Impact Analysis as methods under it, and Schedule Manager/All projects both currently open the same screen (transitional state) — see workspace/GAPS.md.
---

## What is Logic+ Online?

Logic+ Online is a web-based schedule analysis tool. You upload construction project schedules exported from Primavera P6 and Logic+ runs a suite of analytics to help you understand schedule health, delay risk, and critical path behaviour.

## The workflow

1. **Create a project** – in [Schedule Manager](pages/schedule-manager), add a project to group your schedule files (**All projects**, further down the left-hand navigation, currently opens this same screen – see [Schedule Manager](pages/schedule-manager) for why)
2. **Upload a schedule** – drag in a `.xer` file (up to 100 MB)
3. **Wait for processing** – the status badge shows `processing` while Logic+ works through the file, then `processed` once it's ready
4. **Explore the views** – once processed, use the left-hand navigation to move between modules

## Uploading multiple schedules

Upload more than one schedule to the same project to enable comparison views. For example, upload a baseline schedule and a current schedule to see how the project has shifted over time.

## Supported file formats

| Format | Source |
|--------|--------|
| `.xer` | Primavera P6 |

Currently only `.xer` files are supported. The ability to import Microsoft Project and Asta files is planned for a later release.


## Navigating the app

The left-hand navigation gives access to:

- **[Schedule Viewer](pages/gantt-viewer)** – Gantt chart, schedule comparison, and activity relationships
- **[Delay Analysis](pages/delay-analysis)** – choose a delay-analysis method: **Traceback** (trace which activities are driving project delay) or **[Time-Impact Analysis](pages/time-impact-analysis)** (build a hypothetical delay event and test its impact on Practical Completion)
- **[Analytics](pages/analytics-overview)** – schedule quality checks, bow wave compression, completion forecast, and more
- **[Schedule Manager](pages/schedule-manager)** – project list, file upload, processing status
- **All projects** (below a divider) – the project-level landing page; right now it opens the same screen as Schedule Manager – see [Schedule Manager](pages/schedule-manager) for the current state and what's planned

Not all views will show meaningful data unless a project with processed schedules is selected.

## Getting help

This documentation site is the current source of contextual help – there is no in-app help panel or "Learn about this page" link inside Logic+ itself yet. Use the navigation on the left, or the search box, to find the page for the view you're on.
