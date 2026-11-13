---
page-id: gantt-viewer
route: /schedule-viewer
title: Schedule Viewer
audience: external
status: draft
version: 2.4.0
last-reviewed: 2026-11-13
blocked-reason: 2026-11-13 – Printing: added the Show baseline data date line option (PR #556 / LUSB-1369, v1.2.36), on by default, drawn only when a Baseline schedule is selected (GanttPrintView.tsx). Baseline column values (e.g. Baseline Start) also print when those columns are chosen. Not screenshot-verified. 2026-10-28 – 2026-10-28 – New Printing section (PRs #552 / LUSB-1184: print preview step, also in Time-Impact Analysis) and "What group rows show" (PR #534 / LUSB-1303, LUSB-1137: WBS grouping and summary rollup fixes, per frontend/ROLLUP_FIELDS.md). Verified against GanttPrintView.tsx, ViewerToolbar.tsx, ScheduleViewerPage.tsx; not screenshot-verified, and the print dialog itself is the browser's own so its look varies. 2026-09-30 – Group Settings rewritten for LUSB-1125 (v1.2.15–1.2.17): dialog is now "Activity grouping and display settings" – Apply grouping / No grouping, unlimited ordered rules (Group by / Group using / Level / Units / Treat as, interval + custom ranges), activity codes, resources, UDFs groupable; old Mode radio (Group by WBS / custom / none), Outline Level 1–4 and Start Year/Month/Week are gone. Summary/LOE exclusions now default OFF (were On). Verified against GanttSettingsForm.tsx, GanttSettingsContent.tsx, GroupingEditor.tsx, groupingRules.ts, groupingFields.ts, settings.ts, useGroupingSettings.ts (localStorage, not per project). Not yet screenshot-verified. Rewritten following the LUSB-1231 Schedule Viewer redesign (2026-09-17/18) – single underlying screen also renders Delay Analysis's setup/results tabs, combined schedule-selection popover, new fullscreen and colour-legend controls, conditional tab visibility, merged zoom/reset behaviour. Schedule Viewer and Delay Analysis are documented as two separate modules with their own journeys (2026-09-24 correction) – this page covers Schedule Viewer only; Delay Analysis's method-selection/setup/results journey lives on pages/delay-analysis.md and pages/traceback-setup.md. Default schedule/baseline selection on first load and exact drag/hover behaviour for column reordering are inferred from component code, not confirmed against the live app. Group Settings "Start"/"Outline Level" labels are a known dev ticket to relabel — see workspace/GAPS.md.
---

## About the Schedule Viewer

Schedule Viewer is the module for exploring and comparing construction project schedules – open it from **Schedule Viewer** in the left-hand navigation. Use it to look at individual schedules, compare changes between updates, and understand the logic connecting activities.

**[Delay Analysis](pages/delay-analysis.md)** is a separate module, with its own journey – pick a method, work through that method's setup, then review results. It reuses this same schedule-viewing screen (the Gantt chart, activity tray, column selector, and Group Settings all described below) simply because it also needs to display schedule files – that's a shared building block, not a shared journey. Nothing about how you use Schedule Viewer depends on Delay Analysis, or vice versa.

---

## Selecting schedules

Click the **schedule selector** at the top of the page – it shows the currently-selected Baseline and Comparison schedules stacked (e.g. "Baseline: None" / "Comparison: [latest update]"). This opens a popover with both pickers:

**Comparison** – the schedule you are analysing (typically the most recent update). Select from all schedule files uploaded for the current project. Files are labelled by data date.

**Baseline** – an earlier schedule to compare against. When set, each activity in the Gantt chart shows a second bar representing the baseline dates. Select **None** to remove the baseline comparison.

Click **Apply** in the popover to confirm your selection. Changing either schedule resets any active traceback.

**Default selections.** The first time you open a project with no schedule already chosen, the Comparison selector defaults to the schedule with the latest data date and the Baseline selector defaults to the one with the earliest data date – not upload order, which can differ if schedules are uploaded out of sequence. If you've visited before, your last selection for this project is restored from the page URL instead.

**Sharing a view.** The browser address bar updates automatically as you work – it reflects the selected project, Comparison schedule, Baseline schedule, and (if you're working within Delay Analysis) the current traceback. Copy the URL at any point and send it to someone else with access to the same project; opening it restores that exact view, including re-loading the same traceback result. There's no separate "copy link" button – the address bar itself is always the shareable link.

---

## Fullscreen

The **fullscreen** button (top right of the toolbar) expands the Schedule Viewer to fill the browser window, hiding the surrounding navigation. Click it again, or press **Escape**, to exit.

---

## Printing

The **Print preview** button (the printer icon in the toolbar) opens a **Print Preview** of the chart. It's available once a schedule is loaded, and every role – including **View** – can print.

In the preview you can set:

- **Paper size** – **A3 landscape** or **A4 landscape**
- **Columns** – which activity columns to include, and how wide each one is
- **Show line numbers**
- **Remove Activity ID indentation**
- **Show data date line**, and the colour of that line
- **Show baseline data date line** – on by default. When a **Baseline** schedule is selected, a second vertical line marks the baseline's data date, so you can see how far the programme has been updated since. It doesn't appear if no baseline is selected, or if the baseline's data date falls outside the printed timeline.

Click **Continue to print dialog** to open your browser's print dialog (choose a printer, or **Save as PDF**), or **Return to chart** to go back without printing. Time-Impact Analysis has the same print preview in its toolbar.

---

## Chart colour legend

An info button in the toolbar opens a popover explaining what each Gantt bar colour means – Comparison schedule activities (complete, incomplete, or spanning the data date), the Baseline schedule's bars, Critical activities (complete or incomplete), Milestones (shown in red when critical), and Summary/WBS bars.

---

## Column selector

The **Configure columns** button (toolbar) opens a two-panel picker:

- **Available columns** – every column, grouped by category (General, Dates, Durations, Float, Percent Completes, Lists, Variance, Coding, User Defined Fields, Costs). Tick a checkbox to add a column to the activity list.
- **Selected columns** – the columns currently shown. Drag an item up or down to reorder it in the activity list. A few columns (like ID) are fixed and can't be moved or hidden.

The picker also has a date format menu and a duration format menu (controlling how dates/durations display across all columns), and a reset button that restores the default column set.

Default visible columns: ID, Activity Name, Start, Finish, Baseline Start, Baseline Finish, At Completion Duration. Note that Baseline Start/Finish are shown by default even before you select a Baseline schedule — they'll be blank until you do.

The **Predecessors** and **Successors** columns list every linked activity's ID, relationship type, and lag, comma-separated – e.g. `A100: SS (0 days), A200: FS (-0.5 days)`.

The activity tray (see [Activity Detail](#activity-detail), [Changes over time](#changes-over-time)) has its own, separate column picker per tab.

---

## Group and Sort

The **Chart settings** button in the toolbar opens **Activity grouping and display settings**, where you control how activities are grouped and filtered in the Gantt chart.

- **Apply grouping** groups activities using a list of rules – each rule is one level of grouping, and you can add as many as you need. By default there's one rule: **WBS**, to **Level 3**.
- Group by almost any field: WBS, **activity codes**, resources, dates, durations, float, variances, user defined fields and more. Hierarchies (WBS, activity codes, resources) group to a chosen level; dates by day, week, month, quarter, year or custom ranges; numbers by exact value, equal intervals or custom ranges.
- **No grouping** shows a flat activity list.
- **Display Options** let you exclude summary or level-of-effort activities, open with groups collapsed, and show activity counts in group bars.

**What group rows show.** A group or WBS summary row's figures are calculated from the real activities inside it, not from the summary values stored in the schedule file:

- **Dates** – the earliest start and latest finish of its activities.
- **Durations** – the working time between those dates (so two parallel activities covering the same day count as one day, not two). Logic+ uses the activities' calendar when they all share one, otherwise the project's default calendar.
- **Nested groups** combine the underlying activities, rather than averaging the percentages shown on the groups inside them.
- **Baseline** figures only include baseline activities that match an activity currently in that group.

Your settings are remembered in this browser across projects.

[Full detail – Group Settings](pages/schedule-analysis.md#group-settings)

---

## Jump to / Go To

A button next to related activities in the activity tray scrolls and highlights the Gantt chart to that activity, without losing your current selection. It's labelled differently depending on where you are:

- **Jump to** – in the **Activity relationships** tab, in Schedule Viewer and while setting up a traceback
- **Go To** – in the **Activity Scoring** tab, while reviewing Delay Analysis results

Both buttons do the same thing; only the label and icon differ between the two trays.

[Full detail – Activity tray](pages/schedule-analysis.md#activity-tray)

---

## Changes over time

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
