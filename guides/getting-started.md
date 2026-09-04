---
page-id: getting-started
title: Getting Started with Logic+ Online
audience: external
status: draft
version: 1.3.0
last-reviewed: 2026-09-04
blocked-reason: Rewritten from code (header nav structure, status labels, help-panel claim removed as unbuilt) but not yet walked through live — confirm before promoting to complete. Describes Delay Analysis as a fourth module ahead of its actual code split (PM's call, expected ~2026-09-11) — see workspace/GAPS.md "Upcoming release changes to watch for."
---

## What is Logic+ Online?

Logic+ Online is a web-based schedule analysis tool. You upload construction project schedules exported from Primavera P6 and Logic+ runs a suite of analytics to help you understand schedule health, delay risk, and critical path behaviour.

## The workflow

1. **Create a project** – in the Project Manager, add a project to group your schedule files
2. **Upload a schedule** – drag in a `.xer` file (up to 100 MB)
3. **Wait for processing** – the status badge shows `processing` while Logic+ works through the file, then `processed` once it's ready
4. **Explore the views** – once processed, use the navigation bar at the top of the page to move between modules

## Uploading multiple schedules

Upload more than one schedule to the same project to enable comparison views. For example, upload a baseline schedule and a current schedule to see how the project has shifted over time.

## Supported file formats

| Format | Source |
|--------|--------|
| `.xer` | Primavera P6 |

Currently only `.xer` files are supported. The ability to import Microsoft Project and Asta files is planned for a later release.


## Navigating the app

The navigation bar at the top of the page gives access to four modules:

- **[Project Manager](pages/project-manager)** – project list, file upload, processing status
- **[Schedule Viewer](pages/gantt-viewer)** – Gantt chart, schedule comparison, and activity relationships
- **[Delay Analysis](pages/delay-analysis)** – traceback setup and results, reached today via the Schedule Viewer's mode switcher
- **[Analytics](pages/analytics-overview)** – schedule quality checks, bow wave compression, completion forecast, and more

Not all views will show meaningful data unless a project with processed schedules is selected.

## Getting help

This documentation site is the current source of contextual help – there is no in-app help panel or "Learn about this page" link inside Logic+ itself yet. Use the navigation on the left, or the search box, to find the page for the view you're on.
