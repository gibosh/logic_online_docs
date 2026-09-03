---
page-id: gantt-viewer
route: /schedule-viewer/gantt
title: Schedule Viewer
audience: external
status: draft
version: 1.1.2
last-reviewed: 2026-09-03
blocked-reason: Default schedule/baseline selection on first load and exact drag/hover behaviour for column reordering are inferred from component code, not confirmed against the live app.
---

## About the Schedule Viewer

The Schedule Viewer is the central workspace for exploring and comparing construction project schedules. Open it from **Gantt Viewer** in the top navigation bar.

Use the Schedule Viewer to look at individual schedules, compare changes between updates, understand the logic connecting activities, and run delay analysis.

---

## File selection

Choose which schedule files to work with using the two selectors at the top of the page.

**Comparison** – the schedule you are analysing (typically the most recent update). Select from all schedule files uploaded for the current project. Files are labelled by data date.

**Baseline** – an earlier schedule to compare against. When set, each activity in the Gantt chart shows a second bar representing the baseline dates. Select **None** to remove the baseline comparison.

Changing either selector resets any active traceback.

**Default selections.** The first time you open a project with no schedule already chosen, the Comparison selector defaults to the most recent schedule uploaded and the Baseline selector defaults to the earliest one available. If you've visited before, your last selection for this project is restored from the page URL instead.

**Sharing a view.** The browser address bar updates automatically as you work – it reflects the selected project, Comparison schedule, Baseline schedule, and (in Delay Analysis mode) the current traceback. Copy the URL at any point and send it to someone else with access to the same project; opening it restores that exact view, including re-loading the same traceback result. There's no separate "copy link" button – the address bar itself is always the shareable link. (Added 2026-07-24, LUSB-1085.)

---

## Three modes

The Schedule Viewer operates in one of three modes, selectable from the tabs in the toolbar:

| Mode | Use it to |
|------|-----------|
| **Schedule Viewer** | View the Gantt chart, explore activity relationships, and compare activity dates across schedule versions |
| **Traceback Setup** | Select a start activity, configure the algorithm, and run a traceback analysis |
| **Delay Analysis** | Review traceback results, understand delay attribution, save and load tracebacks |

> **Note:** the first mode tab is labelled "Schedule Viewer," the same name as this page. Where this page says "Schedule Viewer" without qualification it means the whole three-mode workspace; "Schedule Viewer mode" refers specifically to the first tab.

The mode advances automatically to Delay Analysis when a traceback completes.

For full detail on each mode see:
- [Schedule Viewer mode](pages/schedule-analysis.md) – Gantt chart, activity tray, group settings
- [Traceback Setup mode](pages/traceback-setup.md) – start activity, profile, weights, exclusion criteria
- [Delay Analysis mode](pages/gantt-delay-analysis.md) – traceback results, save/load, restart options

---

## Column selector

The **Column Selection** button (next to the zoom controls, top right of the toolbar) opens a two-panel picker:

- **Available columns** – every column, grouped by category (General, Dates, Durations, Float, Percent Completes, Lists, Variance, Coding, User Defined Fields, Costs). Tick a checkbox to add a column to the activity list.
- **Selected columns** – the columns currently shown. Drag an item up or down to reorder it in the activity list. A few columns (like ID) are fixed and can't be moved or hidden.

The picker also has a date format menu and a duration format menu (controlling how dates/durations display across all columns), and a reset button that restores the default column set.

Default visible columns: ID, Activity Name, Start, Finish, Baseline Start, Baseline Finish, At Completion Duration. Note that Baseline Start/Finish are shown by default even before you select a Baseline schedule — they'll be blank until you do.

The activity tray (see [Activity Detail](#activity-detail), [Schedule History](#schedule-history)) has its own, separate column picker per tab.

---

## Group and Sort

The **Group Settings** dialog (gear icon, top right of the toolbar) controls how activities are grouped and filtered in the Gantt chart. It opens as a pop-up dialog, not an inline panel.

**Mode** (top-level choice):

| Option | Effect |
|---|---|
| Group by WBS (default) | Activities are grouped and folded under their WBS hierarchy, using the schedule's own outline |
| Use custom grouping | Enables the options below (outline level, time period, filtering, display) |
| Don't group activities | Shows a flat, ungrouped activity list |

When **Use custom grouping** is selected, these become available:

| Setting | Options | Default |
|---------|---------|---------|
| Outline Level | 1 / 2 / 3 / 4 | 3 |
| Start | Year / Month / Week | Month |
| Exclude summary activities | On / Off | On |
| Exclude level of effort activities | On / Off | On |
| Group and sort by WBS | On / Off | On |
| Show activity counts | On / Off | On |

**Start** groups activities by their current start date (not the original baseline plan) – by year, month, or week. If you're comparing against an older baseline schedule, note this follows the schedule's live dates, not the original plan. (Renamed from "Planned Start" and changed to group by current start rather than baseline planned start – LUSB-1068, still in effect as of this review.)

Click **Apply Settings** to apply your selection and close the dialog. Click **Use Default Settings** to reset to the defaults above and close the dialog.

[Full detail – Group Settings](pages/schedule-analysis.md#group-settings)

---

## Go To

The **Go To** button (an arrow icon) appears next to related activities in the activity tray. Clicking it scrolls and highlights the Gantt chart to that activity without losing your current selection.

Go To is available in:
- The **Activity relationships** tab of the activity tray (in Schedule Viewer mode)
- The **Candidate Scoring** tab of the traceback tray (in Delay Analysis mode)

[Full detail – Activity tray](pages/schedule-analysis.md#activity-tray)

---

## Schedule History

The **Changes over time** tab in the activity tray shows the selected activity's key values across every schedule upload for the project – by default Start, Finish, Total Float, and Duration – in a single table, one row per schedule version. Use its column picker to add other fields. Use it to see at a glance whether an activity has been delayed, accelerated, or had its float eroded between updates without opening multiple files.

[Full detail – Activity tray](pages/schedule-analysis.md#activity-tray)

---

## Activity Detail

The **Activity detail** tab in the activity tray shows the selected activity's fields in a two-column key-value table. By default it shows every available column except ID; use the tab's column picker to narrow this down to just the fields you care about. Use it for a full read-out of a specific activity's properties.

[Full detail – Activity tray](pages/schedule-analysis.md#activity-tray)
