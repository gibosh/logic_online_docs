---
page-id: gantt-viewer
route: /schedule-viewer
title: Schedule Viewer
audience: external
status: draft
version: 2.5.0
last-reviewed: 2026-11-23
blocked-reason: 2026-11-23 – LUSB-1392 (v1.2.42): undated schedules are labelled No data date in the selector (ScheduleCombobox.tsx) and in Changes over time; Delay Analysis shows a No data date notice for an undated selection (ScheduleViewerPage.tsx analysisBlocked). Driving path tab: profile is Driving Path, not Driving Task (profiles.ts). Section rules removed. Humanizer rewrite of the prose. 2026-11-13 – Printing: added the Show baseline data date line option (PR #556 / LUSB-1369, v1.2.36), on by default, drawn only when a Baseline schedule is selected (GanttPrintView.tsx). Baseline column values (e.g. Baseline Start) also print when those columns are chosen. Not screenshot-verified. 2026-10-28 – 2026-10-28 – New Printing section (PRs #552 / LUSB-1184: print preview step, also in Time-Impact Analysis) and "What group rows show" (PR #534 / LUSB-1303, LUSB-1137: WBS grouping and summary rollup fixes, per frontend/ROLLUP_FIELDS.md). Verified against GanttPrintView.tsx, ViewerToolbar.tsx, ScheduleViewerPage.tsx; not screenshot-verified, and the print dialog itself is the browser's own so its look varies. 2026-09-30 – Group Settings rewritten for LUSB-1125 (v1.2.15–1.2.17): dialog is now "Activity grouping and display settings" – Apply grouping / No grouping, unlimited ordered rules (Group by / Group using / Level / Units / Treat as, interval + custom ranges), activity codes, resources, UDFs groupable; old Mode radio (Group by WBS / custom / none), Outline Level 1–4 and Start Year/Month/Week are gone. Summary/LOE exclusions now default OFF (were On). Verified against GanttSettingsForm.tsx, GanttSettingsContent.tsx, GroupingEditor.tsx, groupingRules.ts, groupingFields.ts, settings.ts, useGroupingSettings.ts (localStorage, not per project). Not yet screenshot-verified. Rewritten following the LUSB-1231 Schedule Viewer redesign (2026-09-17/18) – single underlying screen also renders Delay Analysis's setup/results tabs, combined schedule-selection popover, new fullscreen and colour-legend controls, conditional tab visibility, merged zoom/reset behaviour. Schedule Viewer and Delay Analysis are documented as two separate modules with their own journeys (2026-09-24 correction) – this page covers Schedule Viewer only; Delay Analysis's method-selection/setup/results journey lives on pages/delay-analysis.md and pages/traceback-setup.md. Default schedule/baseline selection on first load and exact drag/hover behaviour for column reordering are inferred from component code, not confirmed against the live app. Group Settings "Start"/"Outline Level" labels are a known dev ticket to relabel — see workspace/GAPS.md.
---

Schedule Viewer is the module for exploring and comparing construction schedules. Open it from **Schedule Viewer** in the left-hand navigation. Use it to look at a single schedule, compare changes between updates, and follow the logic that connects activities.

**[Delay Analysis](pages/delay-analysis.md)** is a separate module with its own steps: you pick a method, work through that method's setup, then review the results. It shows schedules on this same screen (the Gantt chart, activity tray, column selector and Group Settings described below), but the two modules work independently of each other.

## Selecting schedules

Click the **schedule selector** at the top of the page. It shows the selected Baseline and Comparison schedules one above the other (for example "Baseline: None" and "Comparison: [latest update]"), and opens a popover with both pickers:

**Comparison**: the schedule you're analysing, usually the most recent update. Choose from all the schedule files uploaded to the current project. Files are labelled by data date, and a schedule without one is labelled **No data date**.

**Baseline**: an earlier schedule to compare against. When you set one, each activity in the Gantt chart shows a second bar with the baseline dates. Select **None** to remove the baseline comparison.

Click **Apply** in the popover to confirm. Changing either schedule resets any active traceback.

**Default selections.** The first time you open a project with no schedule chosen, Comparison defaults to the schedule with the latest data date and Baseline to the one with the earliest data date. This follows the data dates, not the upload order, which can differ if schedules were uploaded out of sequence. On later visits, your last selection for the project is restored from the page address.

**Schedules without a data date.** You can open and browse a schedule that has no data date, but analysis needs one. In Delay Analysis, selecting an undated schedule shows a **No data date** notice in place of the chart, asking you to set the date in [Schedule Manager](pages/schedule-manager.md#schedules-without-a-data-date).

**Sharing a view.** The browser's address bar updates as you work. It records the selected project, Comparison and Baseline schedules, and the current traceback if you're working in Delay Analysis. Copy the address at any point and send it to someone with access to the same project; opening it restores the same view, including the same traceback result. There's no separate "copy link" button, because the address itself is the link.

## Fullscreen

The **fullscreen** button (top right of the toolbar) expands the Schedule Viewer to fill the browser window and hides the surrounding navigation. Click it again, or press **Escape**, to exit.

## Printing

The **Print preview** button (the printer icon in the toolbar) opens a **Print Preview** of the chart. It's available once a schedule is loaded, and every role, including **View**, can print.

In the preview you can set:

- **Paper size**: **A3 landscape** or **A4 landscape**
- **Columns**: which activity columns to include, and how wide each one is
- **Show line numbers**
- **Remove Activity ID indentation**
- **Show data date line**, and the colour of that line
- **Show baseline data date line**, on by default. When a **Baseline** schedule is selected, a second vertical line marks the baseline's data date, so you can see how far the programme has moved on since. It doesn't appear if no baseline is selected, or if the baseline's data date falls outside the printed timeline.

Click **Continue to print dialog** to open your browser's print dialog (choose a printer, or **Save as PDF**), or **Return to chart** to go back without printing. Time-Impact Analysis has the same print preview in its toolbar.

## Chart colour legend

An info button in the toolbar opens a popover that explains each Gantt bar colour: Comparison schedule activities (complete, incomplete, or spanning the data date), the Baseline schedule's bars, Critical activities (complete or incomplete), Milestones (red when critical) and Summary/WBS bars.

## Column selector

The **Configure columns** button in the toolbar opens a two-panel picker:

- **Available columns** lists every column, grouped by category (General, Dates, Durations, Float, Percent Completes, Lists, Variance, Coding, User Defined Fields, Costs). Tick a checkbox to add a column to the activity list.
- **Selected columns** lists the columns currently shown. Drag an item up or down to reorder it. A few columns, such as ID, are fixed and can't be moved or hidden.

The picker also has a date format menu and a duration format menu, which control how dates and durations display in every column, and a reset button that restores the default columns.

The default columns are ID, Activity Name, Start, Finish, Baseline Start, Baseline Finish and At Completion Duration. Baseline Start and Baseline Finish show by default even before you select a Baseline schedule, and stay blank until you do.

The **Predecessors** and **Successors** columns list each linked activity's ID, relationship type and lag, separated by commas, for example `A100: SS (0 days), A200: FS (-0.5 days)`.

The activity tray (see [Activity Detail](#activity-detail) and [Changes over time](#changes-over-time)) has a separate column picker on each tab.

## Group and Sort

The **Chart settings** button in the toolbar opens **Activity grouping and display settings**, where you control how activities are grouped and filtered in the Gantt chart.

- **Apply grouping** groups activities by a list of rules, one rule per level of grouping, and you can add as many as you need. By default there's one rule: **WBS**, to **Level 3**.
- You can group by almost any field: WBS, **activity codes**, resources, dates, durations, float, variances, user defined fields and more. Hierarchies (WBS, activity codes, resources) group to a level you choose. Dates group by day, week, month, quarter, year or custom ranges, and numbers by exact value, equal intervals or custom ranges.
- **No grouping** shows a flat activity list.
- **Display Options** let you exclude summary or level-of-effort activities, open with groups collapsed, and show activity counts in group bars.

**What group rows show.** A group or WBS summary row's figures come from the real activities inside it, not from the summary values stored in the schedule file:

- **Dates** are the earliest start and latest finish of its activities.
- **Durations** are the working time between those dates, so two parallel activities covering the same day count as one day. Logic+ uses the activities' calendar when they all share one, and the project's default calendar otherwise.
- **Nested groups** combine the underlying activities instead of averaging the percentages shown on the groups inside them.
- **Baseline** figures include only baseline activities that match an activity currently in that group.

Your settings are remembered in this browser across projects.

[Full detail: Group Settings](pages/schedule-analysis.md#group-settings)

## Jump to / Go To

A button next to related activities in the activity tray scrolls the Gantt chart to that activity and highlights it, without changing your current selection. Its label depends on where you are:

- **Jump to** in the **Activity relationships** tab, in Schedule Viewer and while setting up a traceback
- **Go To** in the **Activity Scoring** tab, while reviewing Delay Analysis results

Both buttons do the same thing; only the label and icon differ.

[Full detail: Activity tray](pages/schedule-analysis.md#activity-tray)

## Changes over time

The **Changes over time** tab in the activity tray shows the selected activity's key values in every schedule uploaded to the project, one row per schedule version. By default it shows Start, Finish, Total Float and Duration, and the tab's column picker adds other fields. A schedule without a data date shows **No data date** in its date column. Use the tab to see whether an activity has been delayed, accelerated or lost float between updates without opening several files.

[Full detail: Activity tray](pages/schedule-analysis.md#activity-tray)

## Activity Detail

The **Activity detail** tab in the activity tray lists the selected activity's fields as a two-column table of names and values. By default it shows every available column except ID, and you can narrow this down with the tab's column picker.

[Full detail: Activity tray](pages/schedule-analysis.md#activity-tray)

## Driving Path

The **Driving path** tab in the activity tray shows a Gantt chart of the driving-path chain leading to the selected activity. It uses Logic+'s own driving-logic calculation, the same one behind the Driving Path algorithm profile, not a traceback result. Checkboxes above the chart let you trace predecessors, successors or both, and turn the relationship arrows on or off.

[Full detail: Activity tray](pages/schedule-analysis.md#activity-tray)
