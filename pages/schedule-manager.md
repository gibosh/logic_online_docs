---
page-id: schedule-manager
route: /schedule-manager
title: Schedule Manager
audience: external
status: draft
version: 2.7.0
last-reviewed: 2026-11-23
blocked-reason: 2026-11-23 – Updated for v1.2.41-v1.2.42: LUSB-1391 download is now a menu (Download original / Convert to Primavera XER / Convert to XML, each conversion hidden when the file is already that format); LUSB-1392 'Data date required' status removed, processed files without a data date show Ready with a 'Not set' warning in the Data date column, sidebar Project warning triangle, analytics skip undated schedules and Delay Analysis/TIA show a 'No data date' notice (MissingDataDate.tsx, SideMenu.tsx, ScheduleManagerPage.tsx). Breadcrumb corrected to All projects / [project] / Schedule Manager (both links), role badge noted. Humanizer rewrite of the prose. 2026-11-13 – EPS Admin renamed Project Admin (v1.2.36). Archived files and Schedule logs buttons are shown only to Project Admins (ScheduleManagerPage.tsx: role === "eps_admin"; Schedule logs also in My Workspace) – the page previously implied anyone could open Archived files. Archive, restore and permanent delete all check schedule.delete (Edit or above). Verified at v1.2.40; not screenshot-verified. 2026-10-28 (later) – 2026-10-28 (later) – Corrected long-standing drift against ScheduleManagerPage.tsx and processingError.ts (PR #542 / LUSB-1329 improved upload errors and recovery): status labels are now Uploading… / Queued / Processing… / Ready / Data date required / Excluded / Processing failed (was "Ready for analysis"), a failed file has an info button explaining why (new error catalogue), Retry processing (and new Start processing) lives in the Edit schedule dialog not an actions menu, row actions are icon buttons (Open Schedule Viewer, Edit schedule, Download original, Archive schedule), and there is an Author column. 2026-10-28 – Schedule archiving (PR #551, LUSB-1349): Delete is now Archive, new Archived files dialog with Restore and permanent delete; permissions release (PR #529) adds the Manage/View access button and role-based greying; EPS Admins get a Schedule logs button. Verified against ScheduleManagerPage.tsx and ArchivedSchedules.tsx, not screenshot-verified live. Earlier notes: Rewritten following a real split from All projects (feature/table-backed-projects, 2026-09-17) – the previous "transitional state, same screen" framing no longer applies. The "a few minutes" typical processing time wasn't independently re-timed for this review. Multi-format upload (MPP/Asta PP/MSPDI XML alongside XER) is confirmed accepted at upload, but parsing fidelity for the three newer formats wasn't independently verified against a live upload – see workspace/GAPS.md. New "Custom fields" section (added 2026-09-18, LUSB-1264) documented from source (fully wired to a real backend, not a prototype), not yet screenshot-verified live. New "Excluding a schedule from analysis" section added 2026-09-23 (feature/schedule-exclusion, first pass) – this is a project-wide toggle only; the per-analytics-module local-adjustment layer described in the PM's original spec (see workspace/GAPS.md) has not shipped. Bulk-select/delete and the confirmed table column order added 2026-09-24, verified directly against `ScheduleManagerPage.tsx`, not yet screenshot-verified live.
---

Schedule Manager is where you upload and manage the schedule files for one project. Select a project first, from **[All projects](pages/all-projects.md)** or the Project dropdown at the top of the sidebar, then open **Schedule Manager** in the left-hand navigation.

The breadcrumb at the top reads **All projects / [project name] / Schedule Manager**, and the first two parts take you back to All projects. Your role on the project (for example **Owner**) shows as a badge beside the page title.

You create, rename, archive and file projects into folders on the **[All projects](pages/all-projects.md)** page.

What you can do here depends on your role on the project. See **[Permissions and Access](pages/permissions.md)**. An access button at the top of the page shows who has access: it reads **Manage access** for Owners and Project Admins, and **View access** for everyone else. Buttons you don't have permission for are greyed out. **Viewers** can open and analyse schedules, but can't upload, edit, download or archive them.

## Key concepts

**Schedule**: an uploaded schedule file, which is a snapshot of the project at one point in time. Logic+ compares the schedules in a project to show trends and changes.

## The schedule files table

Each row is one uploaded schedule. The columns, left to right, are a selection checkbox, **Filename**, any [custom fields](#custom-fields) your project has defined, **Data date**, **Exclude** (see [below](#excluding-a-schedule-from-analysis)), **Status**, **Author** (who uploaded it) and **Actions**. Double-click a row to open that schedule in the Schedule Viewer.

**Selecting and archiving multiple schedules.** Tick individual row checkboxes, or tick the header checkbox to select every schedule at once (it shows as partly ticked when only some are selected). Once at least one is selected, an **Archive *N* schedules** button appears above the table (singular for one). Confirm in the dialog that follows (*"This moves the selected schedules to Archived files and removes them from this project"*) to archive them all at once. You can [restore](#archived-files) them later.

## Uploading schedules

Drag a file onto the page, or click **Upload** to browse for one. You can select several files at once, and they upload one after another.

**Supported formats:** `.xer` (Primavera P6), `.mpp` (Microsoft Project), `.pp` (Asta Powerproject) and `.xml` (MSPDI). Logic+ checks the file name against these formats and rejects anything else with the message *"Use XER, MPP, Asta PP or MSPDI XML files."*

**Maximum file size:** 100 MB per file.

Logic+ processes each schedule automatically after upload. Files you upload together show **Queued** and then **Uploading…** in turn. While any schedule is still processing, the page checks for updates every few seconds.

If an upload fails, the message says what to do. For example, files over 100 MB are rejected. If the connection drops, check the table before uploading again, because the file may already have arrived.

## Schedule status

| Status shown | Meaning |
|---|---|
| Uploading… | The file is being sent to Logic+ |
| Queued | Uploaded and waiting to be processed (also shown when you upload several files together) |
| Processing… | Being processed. It shows for the whole time, with no progress steps. |
| Ready | Processing is complete and the schedule can be opened |
| Excluded | The schedule has been [excluded from analysis](#excluding-a-schedule-from-analysis) |
| Processing failed | An error occurred. Click the **info** button beside the status to see why. |

### Schedules without a data date

Some files, such as Microsoft Project files saved without a status date, don't contain a data date. These still process and show **Ready**, but the **Data date** column shows a warning: **Not set**. A warning triangle also appears beside **Project** at the top of the sidebar. Hover it to see how many schedules in the project are missing a data date.

You can open an undated schedule in the Schedule Viewer, including its activity history, but analysis needs a data date. The analytics leave undated schedules out, and show a **No data date** notice if none of the project's schedules has one. Delay Analysis and Time-Impact Analysis show the same notice when you select an undated schedule. To fix it, open **Edit schedule** (the pencil icon) and set the **Data date**. See [Editing a schedule](#editing-a-schedule).

### When processing fails

The **info** button beside **Processing failed** (*View import error for [filename]*) explains what went wrong and what to do. The common reasons are:

| Reason | What to do |
|---|---|
| **No project was found in this file** | Export a schedule that contains a project and upload it again |
| **Not a supported schedule format** | Export an XER, MPP, Asta PP or Microsoft Project XML file. Renaming the extension isn't enough. |
| **Duplicate Activity ID**, or **duplicate full WBS code** | The message names the first duplicate and the two items sharing it. Fix them in the source application, then export and upload a corrected file. Retrying the unchanged file will fail, and there may be more duplicates than the one named. |
| **The schedule could not be read** | Check the file opens in its source application and export a fresh copy. If that fails too, contact support with the filename. |
| **Timed out, or the processing service couldn't be reached** | Click **Retry processing**. You don't need to upload the file again. |
| **Anything else** | Retry processing. If it fails again, contact support with the filename. |

To retry, open the schedule's **Edit schedule** dialog (the pencil icon) and click **Retry processing**. A schedule that still shows **Queued** has **Start processing** there instead.

## Excluding a schedule from analysis

Each schedule row has an **Exclude from analysis** checkbox. Ticking it removes the schedule from every analysis in the project: it drops out of the Schedule Viewer's schedule selectors, can't be picked as a traceback target, and is left out of the Calibrated profile's training data. While it's ticked, the Status column shows **"Excluded"** in place of the normal status.

The switch applies to the whole project, so you can't exclude a schedule from one analytic and keep it in another. Use it for a schedule you want to keep on record but not let influence any analysis, such as a client-rejected upload you still want to open and review on its own.

## Editing a schedule

Click the pencil (**Edit schedule**) in a schedule's row to:

- Rename it. The **Name** field is a display name only. The original uploaded file name still shows alongside it in the file list.
- Set or correct its **Data date**.
- See the **Original data date** Logic+ detected from the file itself. If it couldn't detect one, this reads "Not available in the source file". It also reads that for schedules uploaded before 2026-09-18, even if a data date was detected then, because older uploads weren't backfilled with this detail.
- Click **Reset to original** to put the Data date field back to the date in the file, undoing any manual correction. This only updates the field; you still need to click **Save**.
- Fill in any **custom fields** your project has defined (see below).

## Custom fields

Click **Preferences** (top of the page) to define your own fields for this project's schedules, such as a revision label or an internal reference. Each field holds plain text.

- **To add a field:** click **Add field** and type a name. A project can have up to 100 fields.
- **To reorder fields:** use the up and down arrows next to each one.
- **To delete a field:** click its trash icon. If the field existed before this editing session, a confirmation appears first: *"Are you sure you want to delete this field? Deleting "[field name]" will remove its value from all schedules in this project when you save preferences."*
- **Schedule Display:** pick one custom field to show next to the filename wherever schedules are listed in the Gantt chart, for example a revision label beside each file name.

Once you've defined a field, it appears as an extra column in the schedule files table and as an extra field in each schedule's **Edit schedule** dialog, where you set its value for that schedule.

Custom fields are separate from the User Defined Fields (UDFs) inside a P6 or MSP schedule file. UDFs are per-activity fields that appear in the Gantt chart's column picker, and you don't define them here.

## Other actions

Each schedule row has these icon buttons (hover for the name):

- **Open Schedule Viewer** opens the schedule in the viewer, the same as double-clicking the row. It's disabled until the schedule is ready.
- **Edit schedule**: see above.
- **Download** (the download icon) opens a menu. **Download original** downloads the file exactly as it was uploaded. **Convert to Primavera XER** and **Convert to XML** download a converted copy; each conversion is offered only when the file isn't already in that format. Downloading needs the **Edit** role, so Viewers can't download.
- **Archive schedule** takes the schedule out of the project and moves it to **Archived files**. A confirmation dialog appears first ("Archive [name]?"). You can undo archiving: see below.

## Archived files

**Project Admins** see an **Archived files** button at the top of the page. It lists schedules archived from **any project** in your organisation, with each schedule's **Filename**, **Project**, **Data date** and **Actions**. Other users archive schedules here but ask a Project Admin to restore or permanently delete them.

- **To restore a schedule:** click **Restore** on its row. It goes back into its original project.
- **To delete schedules permanently:** tick one or more rows and click **Permanently delete selected (*N*)**. A confirmation appears first: *"This permanently deletes the selected archived schedules and their source files. This cannot be undone."* Unlike archiving, **permanent deletion can't be undone**.

Archiving, restoring and permanently deleting all need the **Edit** role or above on the project, so Viewers can't do any of them. Archiving a whole project or folder works differently: see [Archiving and restoring](pages/all-projects.md#archiving-and-restoring).

## Schedule logs

Project Admins also see a **Schedule logs** button beside the upload controls. It opens a record of schedule actions across the organisation: renames, data-date and custom-field edits, exclusions, archiving, restoring, permanent deletion and preference changes. Uploads aren't logged. The log shows **When**, **Who**, **Action** and **Name**, and **Download TXT** saves it. See also **Logs** on [All projects](pages/all-projects.md#logs).
