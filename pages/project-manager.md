---
page-id: project-manager
route: /project-manager
title: Project Manager
audience: external
status: draft
version: 1.1.0
last-reviewed: 2026-07-29
---

## About this page

The Project Manager is the starting point for all work in Logic+. Use it to organise your schedule files into projects and to track the processing status of each upload.

## Key concepts

**Project** – a named container that groups one or more schedule files together. A project represents a single construction project or programme.

**Schedule** – an uploaded `.xer` file representing a point-in-time snapshot of a project schedule. Multiple schedules in the same project are used for comparison and trend analysis.

## Managing projects

**To create a project:**
1. Type a name in the **Add New Project** field
2. Click **Add** or press Enter
3. The new project appears in the left panel and is selected automatically

**To rename a project:**
- Click **Edit** next to the project name, type the new name, then click **Done**

**To delete a project:**
- Click **Delete** next to the project – this also removes all schedules attached to it

## Uploading schedules

1. Select a project in the left panel
2. In the right panel, drag a file onto the upload area or click **Browse Files** (drag-and-drop and click-to-browse both work on the same drop zone)
3. Supported format: `.xer` (Primavera P6) only – other file types are rejected on upload with an alert
4. Maximum file size: 100 MB per file
5. Multiple files can be selected and uploaded at once

After upload, Logic+ processes each schedule automatically. You do not need to do anything – check the status badge to follow progress.

## Schedule processing statuses

| Status | Meaning |
|--------|---------|
| `unprocessed` | File uploaded but processing has not started |
| `packetising` | Schedule is being processed – shown as "packetising…" for the full duration between upload and completion |
| `processed` | Processing complete – schedule is ready for all views |
| `failed` | An error occurred – use **Reprocess** to try again |

Processing typically completes within a few minutes depending on schedule size.

**Note:** earlier versions of this page described a separate `analysing` status with an "analysis N/7" counter as analytics ran. That granular step no longer exists in the app – `packetising` now covers the whole in-progress period, with no visible sub-progress. Confirmed directly against `ScheduleItem.tsx`'s status badge and `ScheduleRecord`'s status type (`uploading | failed | unprocessed | packetising | processed`) as of the LUSB-1060 Project Manager rework, merged 2026-07-28. This also resolves the "Logic+ runs 7 analytics… analysis N/7" line flagged as stale in `getting-started.md` across three earlier reviews – it's not that the count was wrong, the display itself was removed.

## Reprocessing a schedule

The **Reprocess** button appears when a schedule is in `failed` or `unprocessed` status. (Previous wording also mentioned reprocessing being offered when analytics were flagged as out of date – that is not what the code does; `canReprocessSchedule` only checks for `failed` or `unprocessed`.)

Click **Reprocess** to restart processing for that schedule. The schedule data is not re-uploaded.
