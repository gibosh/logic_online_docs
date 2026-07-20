---
page-id: getting-started
title: Getting Started with Logic+ Online
audience: external
status: draft
version: 1.0.3
last-reviewed: 2026-06-08
---

## What is Logic+ Online?

Logic+ Online is a web-based schedule analysis tool. You upload construction project schedules (exported from Primavera P6 or Microsoft Project) and Logic+ runs a suite of analytics to help you understand schedule health, delay risk, and critical path behaviour.

## The workflow

1. **Create a project** – in the Project Manager, add a project to group your schedule files
2. **Upload a schedule** – drag in an `.xer`, `.mpp`, or `.xml` file (up to 100 MB)
3. **Wait for processing** – Logic+ runs 7 analytics in the background; status shows "analysis N/7" as each completes
4. **Explore the views** – once processed, use the sidebar to navigate to the analytics views

## Uploading multiple schedules

Upload more than one schedule to the same project to enable comparison views. For example, upload a baseline schedule and a current schedule to see how the project has shifted over time.

## Supported file formats

| Format | Source |
|--------|--------|
| `.xer` | Primavera P6 |
| `.mpp` | Microsoft Project |
| `.xml` | Primavera P6 XML export |


## Navigating the app

The navigation bar gives access to four modules:

- **[Project Manager](pages/project-manager)** – project list, file upload, processing status
- **[Schedule Viewer](pages/gantt-viewer)** – Gantt chart, schedule comparison, delay analysis
- **[Delay Analysis](pages/delay-analysis)** – Traceback setup, results, save and load
- **[Analytics](pages/analytics-overview)** – schedule quality checks, bow wave compression, completion forecast, and more

Not all views will show meaningful data unless a project with processed schedules is selected.

## Getting help

Each page in Logic+ has a help panel. Look for the **Learn about this page** link to open contextual help for the view you are on.
