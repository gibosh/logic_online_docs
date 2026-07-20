---
page-id: project-manager
route: /project-manager
title: Project Manager
audience: external
status: draft
version: 1.0.3
last-reviewed: 2026-06-08
---

## About this page

The Project Manager is the starting point for all work in Logic+. Use it to organise your schedule files into projects and to track the processing status of each upload.

## Key concepts

**Project** — a named container that groups one or more schedule files together. A project represents a single construction project or programme.

**Schedule** — an uploaded file (`.xer`, `.mpp`, or `.xml`) representing a point-in-time snapshot of a project schedule. Multiple schedules in the same project are used for comparison and trend analysis.

## Managing projects

**To create a project:**
1. Type a name in the **Add New Project** field
2. Click **Add** or press Enter
3. The new project appears in the left panel and is selected automatically

**To rename a project:**
- Click **Edit** next to the project name, type the new name, then click **Done**

**To delete a project:**
- Click **Delete** next to the project — this also removes all schedules attached to it

## Uploading schedules

1. Select a project in the left panel
2. In the right panel, drag a file onto the upload area or click **Browse Files**
3. Supported formats: `.xer` (Primavera P6), `.mpp` (Microsoft Project), `.xml`
4. Maximum file size: 100 MB per file
5. Multiple files can be selected and uploaded at once

After upload, Logic+ processes each schedule automatically. You do not need to do anything — check the status badge to follow progress.

## Schedule processing statuses

| Status | Meaning |
|--------|---------|
| `unprocessed` | File uploaded but processing has not started |
| `packetising` | Schedule is being broken into analysable units |
| `analysing` | Analytics are running — shown as "analysis N/7" as each of the 7 analytics completes |
| `processed` | All analytics complete — schedule is ready for all views |
| `failed` | An error occurred — use **Reprocess** to try again |

Processing typically completes within a few minutes depending on schedule size.

## Reprocessing a schedule

The **Reprocess** button appears when a schedule is in `failed` or `unprocessed` status, or when the analytics are flagged as out of date (this happens when the analytics pipeline is updated).

Click **Reprocess** to restart the analytics run for that schedule. The schedule data is not re-uploaded — only the analytics are re-run.
