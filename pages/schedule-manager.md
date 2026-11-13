---
page-id: schedule-manager
route: /schedule-manager
title: Schedule Manager
audience: external
status: draft
version: 2.6.0
last-reviewed: 2026-11-13
blocked-reason: 2026-11-13 – EPS Admin renamed Project Admin (v1.2.36). Archived files and Schedule logs buttons are shown only to Project Admins (ScheduleManagerPage.tsx: role === "eps_admin"; Schedule logs also in My Workspace) – the page previously implied anyone could open Archived files. Archive, restore and permanent delete all check schedule.delete (Edit or above). Verified at v1.2.40; not screenshot-verified. 2026-10-28 (later) – 2026-10-28 (later) – Corrected long-standing drift against ScheduleManagerPage.tsx and processingError.ts (PR #542 / LUSB-1329 improved upload errors and recovery): status labels are now Uploading… / Queued / Processing… / Ready / Data date required / Excluded / Processing failed (was "Ready for analysis"), a failed file has an info button explaining why (new error catalogue), Retry processing (and new Start processing) lives in the Edit schedule dialog not an actions menu, row actions are icon buttons (Open Schedule Viewer, Edit schedule, Download original, Archive schedule), and there is an Author column. 2026-10-28 – Schedule archiving (PR #551, LUSB-1349): Delete is now Archive, new Archived files dialog with Restore and permanent delete; permissions release (PR #529) adds the Manage/View access button and role-based greying; EPS Admins get a Schedule logs button. Verified against ScheduleManagerPage.tsx and ArchivedSchedules.tsx, not screenshot-verified live. Earlier notes: Rewritten following a real split from All projects (feature/table-backed-projects, 2026-09-17) – the previous "transitional state, same screen" framing no longer applies. The "a few minutes" typical processing time wasn't independently re-timed for this review. Multi-format upload (MPP/Asta PP/MSPDI XML alongside XER) is confirmed accepted at upload, but parsing fidelity for the three newer formats wasn't independently verified against a live upload – see workspace/GAPS.md. New "Custom fields" section (added 2026-09-18, LUSB-1264) documented from source (fully wired to a real backend, not a prototype), not yet screenshot-verified live. New "Excluding a schedule from analysis" section added 2026-09-23 (feature/schedule-exclusion, first pass) – this is a project-wide toggle only; the per-analytics-module local-adjustment layer described in the PM's original spec (see workspace/GAPS.md) has not shipped. Bulk-select/delete and the confirmed table column order added 2026-09-24, verified directly against `ScheduleManagerPage.tsx`, not yet screenshot-verified live.
---

## About this page

Schedule Manager is where you upload and manage the schedule files for one project – select a project first (from **[All projects](pages/all-projects.md)** or the Project dropdown at the top of the sidebar), then open **Schedule Manager** in the left-hand navigation.

A breadcrumb at the top (**[Project Name] / Files**) shows which project you're viewing – it's a label, not a clickable link back to All projects.

Creating, renaming, deleting, or filing projects into folders is done on the **[All projects](pages/all-projects.md)** page, not here.

What you can do here depends on your role on the project – see **[Permissions and Access](pages/permissions.md)**. An access button at the top of the page (**Manage access** for Owners and Project Admins, **View access** for everyone else) shows who has access. Buttons you don't have permission for are greyed out: **Viewers** can open and analyse schedules but not upload, edit, download or archive them.

## Key concepts

**Schedule** – an uploaded schedule file representing a point-in-time snapshot of the project. Multiple schedules in the same project are used for comparison and trend analysis.

## The schedule files table

Each row is one uploaded schedule. Columns, left to right: a selection checkbox, **Filename**, any [custom fields](#custom-fields) your project has defined, **Data date**, **Exclude** (see [below](#excluding-a-schedule-from-analysis)), **Status**, **Author** (who uploaded it) and **Actions**. Double-click a row to open that schedule in the Schedule Viewer.

**Selecting and archiving multiple schedules.** Tick individual row checkboxes, or the header checkbox to select every schedule at once (it shows as partially ticked if only some are selected). Once at least one is selected, an **Archive *N* schedules** button appears above the table (singular for one). Confirm in the dialog that follows – *"This moves the selected schedules to Archived files and removes them from this project"* – to archive them all in one action. Archiving isn't permanent: you can [restore](#archived-files) them later.

## Uploading schedules

Drag a file onto the page, or click **Upload** to browse for one. Multiple files can be selected and uploaded at once – they're sent one after another.

**Supported formats:** `.xer` (Primavera P6), `.mpp` (Microsoft Project), `.pp` (Asta Powerproject), and `.xml` (MSPDI). Logic+ checks the file name against these formats and rejects anything else with the message *"Use XER, MPP, Asta PP or MSPDI XML files."*

**Maximum file size:** 100 MB per file.

After upload, Logic+ processes each schedule automatically – no action needed. Files you upload together show **Queued** and then **Uploading…** in turn. The page checks for updates every few seconds while any schedule is still processing.

If an upload fails, the message says what to do – for example, files over 100 MB are rejected, and if the connection drops it's worth checking the table before re-uploading, because the file may already have arrived.

## Schedule status

| Status shown | Meaning |
|---|---|
| Uploading… | The file is being sent to Logic+ |
| Queued | Uploaded and waiting to be processed (also shown when you upload several files together) |
| Processing… | Being processed – shown for the full duration, with no visible sub-progress |
| Ready | Processing complete and a data date is set – the schedule is ready for all views (shown with a checkmark) |
| Data date required | Processing completed, but Logic+ couldn't detect a data date from the file – open **Edit schedule** to set one manually before this schedule can be used |
| Excluded | The schedule has been [excluded from analysis](#excluding-a-schedule-from-analysis) |
| Processing failed | An error occurred – click the **info** button beside the status to see why |

### When processing fails

The **info** button beside **Processing failed** (*View import error for [filename]*) explains what went wrong and what to do. The common reasons:

| Reason | What to do |
|---|---|
| **No project was found in this file** | Export a schedule that contains a project and upload it again |
| **Not a supported schedule format** | Export an XER, MPP, Asta PP or Microsoft Project XML file – renaming the extension isn't enough |
| **Duplicate Activity ID**, or **duplicate full WBS code** | The message names the first duplicate and the two items sharing it. Fix them in the source application, then export and upload a corrected file. Retrying the unchanged file will fail, and there may be more duplicates than the one named |
| **The schedule could not be read** | Check the file opens in its source application and export a fresh copy; if that fails too, contact support with the filename |
| **Timed out, or the processing service couldn't be reached** | Click **Retry processing** – you don't need to upload it again |
| **Anything else** | Retry processing; if it fails again, contact support with the filename |

To retry, open the schedule's **Edit schedule** dialog (the pencil icon) and click **Retry processing**. A schedule still showing **Queued** has **Start processing** there instead.

## Excluding a schedule from analysis

Each schedule row has an **Exclude from analysis** checkbox. Ticking it removes that schedule from the project entirely for analysis purposes – it drops out of the Schedule Viewer's schedule selectors, it can't be picked as a traceback target, and it's left out of the Calibrated profile's training data. The Status column shows **"Excluded"** in place of the normal status while it's ticked.

This is a project-wide switch – there's currently no way to exclude a schedule from one analytics module while keeping it available in another. Use it for a schedule you want to keep on record (so it isn't deleted) but don't want influencing any analysis – for example, a client-rejected upload you still want to be able to open and review individually.

## Editing a schedule

Click the pencil (**Edit schedule**) in a schedule's row to:

- Rename it (the **Name** field – this is a display name only, separate from the original uploaded file name, which is still shown alongside it in the file list)
- Set or correct its **Data date**
- See the **Original data date** Logic+ detected from the file itself (or "Not available in the source file" if it couldn't detect one – this also shows for a schedule uploaded before 2026-09-18, even if a data date was detected at the time, since older uploads weren't backfilled with this new detail)
- Click **Reset to original** to put the Data date field back to what the file itself said, undoing any manual correction – this only updates the field, you still need to click **Save**
- Fill in any **custom fields** your project has defined (see below)

## Custom fields

Click **Preferences** (top of the page) to define your own fields for this project's schedules – for example, a revision label or an internal reference. Each field is a plain text value.

- **To add a field:** click **Add field** and type a name. (Up to 100 fields per project.)
- **To reorder fields:** use the up/down arrows next to each one.
- **To delete a field:** click its trash icon. If the field already existed before this editing session, a confirmation appears first – *"Are you sure you want to delete this field? Deleting "[field name]" will remove its value from all schedules in this project when you save preferences."*
- **Schedule Display:** pick one custom field to show alongside the filename wherever schedules are listed in the Gantt chart (e.g. "Filename – Revision B").

Once a field is defined, it appears as an extra column in the schedule files table, and as an extra field in each schedule's **Edit schedule** dialog – that's where you set or change its value for a specific schedule.

Custom fields are separate from the User Defined Fields (UDFs) that come from inside a P6 or MSP schedule file itself – those are per-activity fields shown in the Gantt chart's column picker, not something you define here.

## Other actions

Each schedule row has these icon buttons (hover for the name):

- **Open Schedule Viewer** – opens the schedule in the viewer (the same as double-clicking the row). Disabled until the schedule is ready
- **Edit schedule** – see above
- **Download original** – downloads the file exactly as it was uploaded. Needs the **Edit** role – Viewers can't download
- **Archive schedule** – takes the schedule out of the project and moves it to **Archived files**. A confirmation dialog appears first ("Archive [name]?"). Archiving can be undone – see below.

## Archived files

**Project Admins** see an **Archived files** button at the top of the page, which shows schedules that have been archived from **any project** in your organisation. (Everyone else archives schedules here, but asks a Project Admin to restore or permanently delete them.) The table shows each schedule's **Filename**, **Project**, **Data date** and **Actions**.

- **To restore a schedule:** click **Restore** on its row. It goes back into its original project.
- **To delete schedules permanently:** tick one or more rows and click **Permanently delete selected (*N*)**. A confirmation – *"This permanently deletes the selected archived schedules and their source files. This cannot be undone."* – appears first. Unlike archiving, **permanent deletion can't be undone**.

Archiving, restoring and permanently deleting all need the **Edit** role or above on the project; Viewers can't do any of them. Archiving a whole project or folder is separate – see [Archiving and restoring](pages/all-projects.md#archiving-and-restoring).

## Schedule logs

Project Admins also see a **Schedule logs** button beside the upload controls. It opens a record of schedule actions across the organisation – renames, data-date and custom-field edits, exclusions, archiving, restoring, permanent deletion and preference changes (uploads aren't logged) – showing **When**, **Who**, **Action** and **Name**, with **Download TXT** to save it. See also **Logs** on [All projects](pages/all-projects.md#logs).
