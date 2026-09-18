---
page-id: gantt-viewer
route: /schedule-viewer
title: Schedule Viewer
audience: external
status: draft
version: 2.0.0
last-reviewed: 2026-09-18
blocked-reason: Rewritten following the LUSB-1231 Schedule Viewer redesign (2026-09-17/18) – single unified page now serves all three modes, combined schedule-selection popover, new fullscreen and colour-legend controls, conditional mode-tab visibility, merged zoom/reset behaviour. Default schedule/baseline selection on first load and exact drag/hover behaviour for column reordering are inferred from component code, not confirmed against the live app. Group Settings "Start"/"Outline Level" labels are a known dev ticket to relabel — see workspace/GAPS.md.
---

## About the Schedule Viewer

The Schedule Viewer is the central workspace for exploring and comparing construction project schedules. One page, and one underlying screen, serves all three of its modes (see "Three modes," below) – open it from **Schedule Viewer** in the left-hand navigation, or reach its Traceback Setup / Delay Analysis modes directly via **[Delay Analysis](pages/delay-analysis.md)** in the navigation. Both paths land on the same page.

Use the Schedule Viewer to look at individual schedules, compare changes between updates, understand the logic connecting activities, and run delay analysis.

---

## Selecting schedules

Click the **schedule selector** at the top of the page – it shows the currently-selected Baseline and Comparison schedules stacked (e.g. "Baseline: None" / "Comparison: [latest update]"). This opens a popover with both pickers:

**Comparison** – the schedule you are analysing (typically the most recent update). Select from all schedule files uploaded for the current project. Files are labelled by data date.

**Baseline** – an earlier schedule to compare against. When set, each activity in the Gantt chart shows a second bar representing the baseline dates. Select **None** to remove the baseline comparison.

Click **Apply** in the popover to confirm your selection. Changing either schedule resets any active traceback.

**Default selections.** The first time you open a project with no schedule already chosen, the Comparison selector defaults to the most recent schedule uploaded and the Baseline selector defaults to the earliest one available. If you've visited before, your last selection for this project is restored from the page URL instead.

**Sharing a view.** The browser address bar updates automatically as you work – it reflects the selected project, Comparison schedule, Baseline schedule, and (in Delay Analysis mode) the current traceback. Copy the URL at any point and send it to someone else with access to the same project; opening it restores that exact view, including re-loading the same traceback result. There's no separate "copy link" button – the address bar itself is always the shareable link.

---

## Fullscreen

The **fullscreen** button (top right of the toolbar) expands the Schedule Viewer to fill the browser window, hiding the surrounding navigation. Click it again, or press **Escape**, to exit.

---

## Chart colour legend

An info button in the toolbar opens a popover explaining what each Gantt bar colour means – Comparison schedule activities (complete, incomplete, or spanning the data date), the Baseline schedule's bars, Critical activities (complete or incomplete), Milestones (shown in red when critical), and Summary/WBS bars.

---

## Three modes

The Schedule Viewer operates in one of three modes:

| Mode | Use it to |
|------|-----------|
| **Schedule Viewer** | View the Gantt chart, explore activity relationships, and compare activity dates across schedule versions |
| **Traceback Setup** | Select a start activity, configure the algorithm, and run a traceback analysis |
| **Delay Analysis** | Review traceback results, understand delay attribution, save and load tracebacks |

> **Note:** the first mode tab is labelled "Schedule Viewer," the same name as this page. Where this page says "Schedule Viewer" without qualification it means the whole three-mode workspace; "Schedule Viewer mode" refers specifically to the first tab.

**The mode tab strip isn't always shown.** In plain Schedule Viewer mode with no traceback loaded, there's nothing to switch to yet, so the tabs are hidden – they appear once you have a traceback in progress or completed, once you're in Traceback Setup or Delay Analysis mode, or if you arrived via the **Delay Analysis** navigation entry point (which opens straight into Traceback Setup). The mode also advances automatically to Delay Analysis when a traceback completes.

For full detail on each mode see:
- [Schedule Viewer mode](pages/schedule-analysis.md) – Gantt chart, activity tray, group settings
- [Traceback Setup mode](pages/traceback-setup.md) – start activity, profile, weights, exclusion criteria
- [Delay Analysis mode](pages/gantt-delay-analysis.md) – traceback results, save/load, restart options

---

## Column selector

The **Configure columns** button (toolbar) opens a two-panel picker:

- **Available columns** – every column, grouped by category (General, Dates, Durations, Float, Percent Completes, Lists, Variance, Coding, User Defined Fields, Costs). Tick a checkbox to add a column to the activity list.
- **Selected columns** – the columns currently shown. Drag an item up or down to reorder it in the activity list. A few columns (like ID) are fixed and can't be moved or hidden.

The picker also has a date format menu and a duration format menu (controlling how dates/durations display across all columns), and a reset button that restores the default column set.

Default visible columns: ID, Activity Name, Start, Finish, Baseline Start, Baseline Finish, At Completion Duration. Note that Baseline Start/Finish are shown by default even before you select a Baseline schedule — they'll be blank until you do.

The **Predecessors** and **Successors** columns list every linked activity's ID, relationship type, and lag, comma-separated – e.g. `A100: SS (0 days), A200: FS (-0.5 days)`.

The activity tray (see [Activity Detail](#activity-detail), [Schedule History](#schedule-history)) has its own, separate column picker per tab.

---

## Group and Sort

The **Group Settings** dialog (**Chart settings** button, toolbar) controls how activities are grouped and filtered in the Gantt chart. It opens as a pop-up dialog, not an inline panel.

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

**Outline Level** controls how many levels of WBS hierarchy are shown – Level 1 shows only the top-level groupings, Level 4 shows a deeper breakdown.

**Start** groups activities into a Year, Month, or Week bucket. Despite the name, this isn't about an activity's start date specifically – any activity shorter than the bucket size (a year, when grouping by year) lands in the same bucket regardless of which of its dates you'd use to place it.

Click **Apply Settings** to apply your selection and close the dialog. Click **Use Default Settings** to reset to the defaults above and close the dialog.

[Full detail – Group Settings](pages/schedule-analysis.md#group-settings)

---

## Jump to / Go To

A button next to related activities in the activity tray scrolls and highlights the Gantt chart to that activity, without losing your current selection. It's labelled differently depending on where you are:

- **Jump to** – in the **Activity relationships** tab, in Schedule Viewer and Traceback Setup modes
- **Go To** – in the **Candidate Scoring** tab, in Delay Analysis mode

Both buttons do the same thing; only the label and icon differ between the two trays.

[Full detail – Activity tray](pages/schedule-analysis.md#activity-tray)

---

## Schedule History

The **Changes over time** tab in the activity tray shows the selected activity's key values across every schedule upload for the project – by default Start, Finish, Total Float, and Duration – in a single table, one row per schedule version. Use its column picker to add other fields. Use it to see at a glance whether an activity has been delayed, accelerated, or had its float eroded between updates without opening multiple files.

[Full detail – Activity tray](pages/schedule-analysis.md#activity-tray)

---

## Activity Detail

The **Activity detail** tab in the activity tray shows the selected activity's fields in a two-column key-value table. By default it shows every available column except ID; use the tab's column picker to narrow this down to just the fields you care about. Use it for a full read-out of a specific activity's properties.

[Full detail – Activity tray](pages/schedule-analysis.md#activity-tray)

---

## Driving Path

The **Driving path** tab in the activity tray shows a Gantt chart of the driving-path chain leading to the selected activity, using Logic+'s own driving-logic calculation (the same logic behind the Driving Task algorithm profile) rather than a traceback result. Checkboxes above the chart let you trace predecessors, successors, or both, and toggle the relationship arrows.

[Full detail – Activity tray](pages/schedule-analysis.md#activity-tray)
