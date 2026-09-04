---
page-id: project-manager
route: /project-manager
title: Project Manager
audience: external
status: draft
version: 1.1.2
last-reviewed: 2026-09-04
blocked-reason: The "a few minutes" typical processing time wasn't independently re-timed for this review. This whole page is expected to be split into Project Manager (folders) + a renamed Schedule Manager (upload) within a week or two — see workspace/GAPS.md "Upcoming release changes to watch for."
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
3. The new project appears in the left panel

**Note:** if the new project isn't already selected, click it in the list to open it.

**To rename a project:**
- Click **Edit** next to the project name, type the new name, then click **Done**

**To delete a project:**
- Click **Delete** next to the project. There is no confirmation step, so make sure before you click – it can't be undone from the page. Schedules that were in the project are no longer accessible afterwards either.

## Uploading schedules

1. Select a project in the left panel
2. In the right panel, drag a file onto the upload area or click **Browse Files** (drag-and-drop and click-to-browse both work on the same drop zone)
3. Supported format: `.xer` (Primavera P6) only. Logic+ checks both the file name and the start of the file's content, so a file that isn't really a P6 export is rejected even if it's named `.xer`
4. Maximum file size: 100 MB per file
5. Multiple files can be selected and uploaded at once – they're sent one after another

After upload, Logic+ processes each schedule automatically. You do not need to do anything – check the status badge to follow progress. The page checks for updates every few seconds while a schedule is still processing.

## Schedule processing statuses

| Status | Meaning |
|--------|---------|
| `unprocessed` | File uploaded but processing has not started |
| `processing` | Schedule is being processed – shown as "processing" for the full duration between upload and completion, with no visible sub-progress |
| `processed` | Processing complete – schedule is ready for all views |
| `failed` | An error occurred – use **Reprocess** to try again |

Processing typically completes within a few minutes depending on schedule size.

**Note:** the in-progress badge now reads "processing" rather than "packetising" – the underlying status name changed as part of a schedule-status rework since this page was last reviewed (confirmed against `ScheduleItem.tsx`'s status badge and the `ScheduleStatus` type in `@lware/contracts`, now `STATUS_UPLOADING | STATUS_FAILED_TO_PROCESS | STATUS_UNPROCESSED | STATUS_PROCESSING | STATUS_PROCESSED`). There is still no separate "analysing" step or "analysis N/7" counter – that was already confirmed removed in the previous review of this page and remains gone.

If a schedule fails, the page doesn't currently explain why – you'll only see the "failed" badge and a Reprocess button. If Reprocess doesn't resolve it, contact support with the schedule name and roughly when you uploaded it.

## Reprocessing a schedule

The **Reprocess** button appears when a schedule is in `failed` or `unprocessed` status. Click **Reprocess** to restart processing for that schedule. The schedule file is not re-uploaded – Logic+ reprocesses the file you already sent.

## Removing a schedule

Click **Delete** on a schedule to remove it from the project. As with deleting a project, there is no confirmation step, so make sure before you click.
