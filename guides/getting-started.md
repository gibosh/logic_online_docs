---
page-id: getting-started
title: Getting Started with Logic+ Online
audience: external
status: draft
version: 1.2.0
last-reviewed: 2026-09-03
blocked-reason: Rewritten from code (header nav structure, status labels, help-panel claim removed as unbuilt) but not yet walked through live — confirm before promoting to complete.
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

Only `.xer` is currently supported — the file must also pass a P6 header check on upload. `.mpp` (Microsoft Project) and `.xml` were previously listed here as supported; that's not the case in the current app (checked directly against `useFileUploadHandlers.ts`, 2026-07-29). If multi-format support is planned for a later release, update this table then — don't reintroduce it speculatively.


## Navigating the app

The navigation bar at the top of the page gives access to three modules:

- **[Project Manager](pages/project-manager)** – project list, file upload, processing status
- **[Schedule Viewer](pages/gantt-viewer)** – Gantt chart, schedule comparison, and (as one of its three modes) Delay Analysis / traceback setup and results
- **[Analytics](pages/analytics-overview)** – schedule quality checks, bow wave compression, completion forecast, and more

Delay Analysis isn't a separate top-level module – it's reached from inside the Schedule Viewer, by switching modes. See [Schedule Viewer – Three modes](pages/gantt-viewer?id=three-modes).

Not all views will show meaningful data unless a project with processed schedules is selected.

## Getting help

This documentation site is the current source of contextual help – there is no in-app help panel or "Learn about this page" link inside Logic+ itself yet. Use the navigation on the left, or the search box, to find the page for the view you're on.
