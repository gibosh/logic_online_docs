---
page-id: schedule-manager
route: /schedule-manager
title: Schedule Manager
audience: external
status: draft
version: 2.0.0
last-reviewed: 2026-09-18
blocked-reason: Rewritten following a real split from All projects (feature/table-backed-projects, 2026-09-17) – the previous "transitional state, same screen" framing no longer applies. The "a few minutes" typical processing time wasn't independently re-timed for this review. Multi-format upload (MPP/Asta PP/MSPDI XML alongside XER) is confirmed accepted at upload, but parsing fidelity for the three newer formats wasn't independently verified against a live upload – see workspace/GAPS.md.
---

## About this page

Schedule Manager is where you upload and manage the schedule files for one project – select a project first (from **[All projects](pages/all-projects.md)** or the Project dropdown at the top of the sidebar), then open **Schedule Manager** in the left-hand navigation.

A breadcrumb at the top (**[Project Name] / Files**) shows which project you're viewing – it's a label, not a clickable link back to All projects.

Creating, renaming, deleting, or grouping projects themselves is done on the **[All projects](pages/all-projects.md)** page, not here.

## Key concepts

**Schedule** – an uploaded schedule file representing a point-in-time snapshot of the project. Multiple schedules in the same project are used for comparison and trend analysis.

## Uploading schedules

Drag a file onto the page, or click **Upload** to browse for one. Multiple files can be selected and uploaded at once – they're sent one after another.

**Supported formats:** `.xer` (Primavera P6), `.mpp` (Microsoft Project), `.pp` (Asta Powerproject), and `.xml` (MSPDI). Logic+ checks the file name against these formats and rejects anything else with the message *"Use XER, MPP, Asta PP or MSPDI XML files."*

**Maximum file size:** 100 MB per file.

After upload, Logic+ processes each schedule automatically – no action needed. The page checks for updates every few seconds while any schedule is still processing.

## Schedule status

| Status shown | Meaning |
|---|---|
| Processing… | File uploaded and/or being processed – shown for the full duration between upload and completion, with no visible sub-progress |
| Ready for analysis | Processing complete and a data date is set – schedule is ready for all views (shown with a checkmark) |
| Data date required | Processing completed, but Logic+ couldn't detect a data date from the file – open **Edit schedule** to set one manually before this schedule can be used |
| Processing failed | An error occurred – use **Retry processing** to try again |

If a schedule fails, the page doesn't currently explain why – you'll only see "Processing failed" and a **Retry processing** action. If retrying doesn't resolve it, contact support with the schedule name and roughly when you uploaded it.

## Editing a schedule

Open a schedule's actions menu (**...**) and choose **Edit schedule** to:

- Rename it (the **Name** field – this is a display name only, separate from the original uploaded file name, which is still shown alongside it in the file list)
- Set or correct its **Data date**
- See the **Original data date** Logic+ detected from the file itself (or "Not available in the source file" if it couldn't detect one)
- Click **Reset to original** to put the Data date field back to what the file itself said, undoing any manual correction – this only updates the field, you still need to click **Save**

## Other actions

Each schedule's actions menu (**...**) also offers:

- **Download original** – downloads the file exactly as it was uploaded
- **Retry processing** – only shown when the schedule's status is "Processing failed"; re-processes the file you already sent, without needing to re-upload it
- **Delete** – removes the schedule from the project. A confirmation dialog appears first ("This removes the schedule file from this project.") – read it before confirming, as this can't be undone afterwards.
