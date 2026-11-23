---
page-id: schedule-analysis
route: /schedule-viewer
title: Schedule Viewer Detail
audience: external
status: draft
version: 1.6.0
last-reviewed: 2026-11-23
blocked-reason: 2026-11-23 – LUSB-1392: Changes over time shows No data date for undated schedules (ActivityTabs.tsx formatDateGbOr). Driving Task dash has hover text Driving path unavailable (DrivingTaskFlag.tsx). Humanizer rewrite of the prose. 2026-09-30 – Group Settings rewritten for LUSB-1125 (v1.2.15–1.2.17): dialog is now "Activity grouping and display settings" – Apply grouping / No grouping, unlimited ordered rules (Group by / Group using / Level / Units / Treat as, interval + custom ranges), activity codes, resources, UDFs groupable; old Mode radio (Group by WBS / custom / none), Outline Level 1–4 and Start Year/Month/Week are gone. Summary/LOE exclusions now default OFF (were On). Verified against GanttSettingsForm.tsx, GanttSettingsContent.tsx, GroupingEditor.tsx, groupingRules.ts, groupingFields.ts, settings.ts, useGroupingSettings.ts (localStorage, not per project). Not yet screenshot-verified. Retitled and reframed 2026-09-25 – was "Gantt Viewer – Schedule Viewer mode", describing Schedule Viewer as "the first of three mode tabs." That framing was corrected sitewide 2026-09-24 (Schedule Viewer and Delay Analysis are two separate modules with separate journeys, not shared modes of one workspace) but this page was missed in that pass despite being linked from 7+ other pages as the canonical Gantt/activity-tray reference – caught in the 2026-09-25 sitewide drift audit. Updated following the LUSB-1231 Schedule Viewer redesign (2026-09-17/18) – "Jump to" relabel, default active tab change, merged zoom behaviour, toolbar button wording. Further updated same day for feature/gantt-smooth-zoom (LUSB-1063) – zoom-in ceiling, Ctrl/Cmd+scroll smooth zoom, and the new synced scrollbar strip. Exact drag/resize feel (column drag-reorder, panel/column/tray resize handles) verified from component structure only, not tested live. Group Settings "Start"/"Outline Level" labels are a known dev ticket to relabel — see workspace/GAPS.md.
---

This page is the detailed reference for the [Schedule Viewer](pages/gantt-viewer.md)'s Gantt chart and activity tray: how to read the chart, resize and configure it, and what each activity-tray tab shows. Delay Analysis shows its results on the same screen (see [Delay Analysis](pages/delay-analysis.md)), so most of this page applies in either module.

## The Gantt chart

The chart has two panels:

- **Left panel**: the activity list, with configurable columns. Drag the divider on the right edge of the panel to resize it, or drag the edge of a column header to resize that column.
- **Right panel**: the Gantt bars, drawn against a timeline. Use the zoom controls in the toolbar to zoom in, or to reset to a 1x fit-to-width view. You can't zoom out further than 1x fit-to-width, and zooming in stops once the timeline reaches its finest detail; each zoom button greys out at its limit. To zoom smoothly around your cursor instead, hold **Ctrl** (or **Cmd**) and scroll the mouse wheel over the chart.

A scrollbar strip below the chart keeps the activity list and timeline panels in step. Drag either half to scroll both, or use your keyboard once the strip has focus.

Click the arrow next to a WBS code to expand or collapse its summary row. Activities appear as horizontal bars from their start to their finish dates, and milestones as diamonds.

When a **Baseline** schedule is selected, each activity also shows a thinner bar below its current bar, in the same row, for the baseline dates. The activity list's Baseline Start and Baseline Finish columns show by default even with no baseline chosen, and stay blank until you select one.

**Click any activity** to select it and open the activity tray at the bottom of the page.

## Activity tray

The tray is collapsed by default. Click **Expand** in the tray's header to open it; while it's open, the same control reads **Minimize**, and it's the only way to open or close the tray. With an activity selected, the tray shows four tabs, starting on **Activity relationships**. Each tab has its own column picker (top right of the tabs), so you can choose which fields it shows. The tray header shows the selected activity's ID. Drag the top edge of the open tray to change its height. Each table's column header stays at the top as you scroll its rows.

### Activity relationships

Shows all the predecessors and successors of the selected activity.

| Column | Description |
|--------|-------------|
| ID | Activity ID of the related activity |
| Activity Name | Name of the related activity |
| Relationship Type | How the activities are linked: FS (Finish-to-Start), SS (Start-to-Start), FF (Finish-to-Finish) or SF (Start-to-Finish) |
| Critical | Whether the related activity is on Logic+'s calculated critical path |
| Driving Task | Predecessor rows only. Shows whether the schedule's own logic and dates say this predecessor drives the selected activity, independent of any traceback. A red flag marks the direct driver. An amber flag means it drives the activity indirectly, further back along the chain; hover it to see the chain. No icon means it isn't on the driving path, and a dash (hover text *Driving path unavailable*) means the driving path couldn't be calculated. See [How the Traceback method works](pages/traceback-engine.md#driving-path) for what "driving" means here. |
| Jump to | A button that moves the Gantt chart to the related activity. The same action is labelled **Go To** in the Delay Analysis tray's Activity Scoring tab; see [Traceback Results](pages/gantt-delay-analysis.md#activity-scoring). |

If the selected activity has no relationships, a message says so.

### Driving path

Shows a Gantt chart of the driving-path chain leading to the selected activity. It uses the same logic as the Driving Task column above, drawn as a chart instead of a flag on each row. Three checkboxes above the chart, all on by default, control what's traced and drawn:

- **Predecessors** traces backward from the selected activity.
- **Successors** traces forward from the selected activity.
- **Relationship arrows** draws the links between activities on the path.

Switch off Predecessors or Successors to trace in one direction only, or both to show just the selected activity. This tab has its own column picker (default columns: ID, Start, Finish). Select an activity to fill it; with nothing selected, or with a WBS summary row selected, it shows a prompt instead of a chart. On larger schedules, "Calculating driving path…" shows briefly while Logic+ works out the chain.

### Changes over time

Shows the selected activity's key values in every schedule uploaded to the project, one row per schedule version, identified by data date. A schedule without a data date shows **No data date** in that column.

The default columns are Start, Finish, Total Float (days) and Duration, and the column picker adds others.

Use this tab to see whether an activity has been delayed, accelerated or lost float between schedule updates.

### Activity detail

Shows the selected activity's fields as a two-column table of names and values. By default it includes every available column except ID; use the tab's column picker to narrow it down.

## Group Settings

The **Chart settings** button in the toolbar opens **Activity grouping and display settings**. At the top, choose **Apply grouping** to group activities with the rules below, or **No grouping** for a flat activity list.

### Grouping Options

Grouping is built from a list of rules. The first rule makes the top-level groups, the second groups activities within each of those, and so on. By default there's one rule: **WBS**, to **Level 3**.

- **Add grouping** adds a rule. Drag a rule's handle to reorder it, or click its bin button to remove it.
- You can use the same field more than once, for example WBS to Level 1, then an activity code, then WBS again to a deeper level.

For each rule, choose:

**Group by**: the field to group on. The list is searchable and organised by column category:

- Standard activity fields: dates, durations, float, status, activity type, calendar, critical, variances, costs and more
- **WBS**, **WBS Name** and **WBS Path**
- **Resources**, following your schedule's resource hierarchy
- **Coding**: every activity code type in the schedule, following each code's own hierarchy
- **User Defined Fields**: your schedule's custom fields
- Traceback fields (Score, Defined Link, Low Confidence, Traced Position), which show as *Available in Traceback Setup* and can be used only there

**Group using**: the options depend on the kind of field.

| Field | Group using |
|---|---|
| WBS, Resources, activity codes | **Hierarchy level**, then choose a **Level**. Levels deeper than the current schedule goes are marked *(not in this schedule)* |
| Dates | **Day**, **Week**, **Month** (default), **Quarter**, **Year** or **Custom ranges** |
| Text and Yes / No fields | **Exact values** |
| Numbers, costs, percentages and durations | **Exact values**, **Custom ranges** or **Equal intervals**, plus **Negative / zero / positive** for float and variance fields |

- **Equal intervals**: set an **Interval width** and an **Origin** (where the first interval starts). Use **And above (optional)** to put everything from that value upward into one final group.
- **Custom ranges**: click **Add range** for each group and fill in **From** and **To**. Leave one end blank for an open-ended range.
- **Units**: for durations, choose **Working hours**, **Working days** or **Working months**.
- **Treat as**: for user defined fields, Logic+ detects the type of data automatically. If it guesses wrong, change it here (Text, Number, Cost, Percentage, Date, Duration or Yes / No).

If a rule is incomplete, a message shows under it, and **Apply settings** stays unavailable until you fix it.

### Display Options

| Setting | Default | What it does |
|---|---|---|
| Exclude summary activities | Off | Hides WBS summary activities |
| Exclude level-of-effort activities | Off | Hides level-of-effort activities |
| Collapse Groups | Off | Opens the chart with every group folded (only with **Apply grouping**) |
| Show activity count in group bar | On | Shows how many activities are in each group |

Click **Apply settings** to apply your changes and close the dialog. **Reset to default** puts everything back to the defaults above; click **Apply settings** afterwards to use them. **Cancel** closes without changing anything.

Your grouping settings are remembered in this browser and apply to every project you open. While a traceback is still running, its provisional results always show as a flat, ungrouped list.

## Column picker

The **Configure columns** button in the toolbar opens a two-panel picker:

- **Available columns**, grouped into categories. Tick a column to add it.
- **Selected columns**. Drag to reorder. A few, such as ID, are fixed in place.

It also has a date format menu, a duration format menu and a reset button.

Column categories, with examples of what's in each:

| Category | Examples |
|---|---|
| General | ID, Activity Name, Activity Status, Activity Type, Calendar, Critical, Longest Path, WBS, WBS Name |
| Dates | Start, Finish, Early/Late Start & Finish, Actual Start/Finish, Baseline Start/Finish, Constraint dates |
| Durations | At Completion Duration, Original Duration, Remaining Duration, Baseline Duration |
| Float | Total Float, Free Float |
| Percent Completes | Activity, Duration, Physical, Work, and Schedule % Complete |
| Lists | Predecessors, Successors, Resources |
| Variance | Start/Finish/Duration variance against baseline (in calendar days) |
| Coding | Activity code fields defined in the schedule |
| User Defined Fields | Any UDFs present in the schedule |
| Costs | Expected Costs |

The default columns in the activity list are ID, Activity Name, Start, Finish, Baseline Start, Baseline Finish and At Completion Duration. Total Float and Critical are available but hidden by default; add them from the picker to see the critical path at a glance.

The **Predecessors** and **Successors** columns list each linked activity's ID, relationship type and lag, separated by commas, for example `A100: SS (0 days), A200: FS (-0.5 days)`. A negative lag means the two activities overlap.
