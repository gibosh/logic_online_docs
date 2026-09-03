---
page-id: schedule-analysis
route: /schedule-viewer/gantt
title: Gantt Viewer – Schedule Viewer mode
audience: external
status: draft
version: 1.1.1
last-reviewed: 2026-09-03
blocked-reason: Exact drag/resize feel (column drag-reorder, panel/column/tray resize handles) verified from component structure only, not tested live.
---

## About this mode

**Schedule Viewer** is the default mode in the [Gantt Viewer](pages/gantt-viewer.md) — note it shares its name with the Gantt Viewer page itself; this refers specifically to the first of the three mode tabs. Use it to explore activities, inspect relationships, and compare how activities have changed across schedule versions.

## The Gantt chart

The chart is split into two panels:

- **Left panel** – the activity list with configurable columns. Drag the divider on the right edge of the panel to resize it, or drag the edge of an individual column header to resize just that column.
- **Right panel** – the Gantt bars drawn against a timeline. Use the zoom controls (top right of the toolbar) to zoom in, zoom out, reset to 1x, or auto-fit the chart to the current width.

WBS summary rows can be expanded or collapsed using the arrow next to the WBS code. Activities are shown with their start and finish dates as horizontal bars. Milestones are shown as diamonds.

When a **Baseline** schedule is selected, each activity shows a thinner bar within the same row, positioned below its current bar, representing the baseline dates. (The activity list's Baseline Start / Baseline Finish columns are visible by default even with no baseline chosen – they just stay blank until you select one.)

**Click any activity** to select it and open the activity tray at the bottom of the page.

## Activity tray

The tray is collapsed by default. Click **Show Details** to open it (the button relabels to **Hide Details** while open). When an activity is selected, the tray shows three tabs, each with its own column picker (top right of the tabs) so you can customise which fields that tab displays. The active activity ID is shown in the tray header. Drag the top edge of the open tray to resize its height.

### Activity relationships

Shows all predecessors and successors of the selected activity.

| Column | Description |
|--------|-------------|
| ID | Activity ID of the related activity |
| Activity Name | Name of the related activity |
| Relationship Type | How the activities are linked – FS (Finish-to-Start), SS (Start-to-Start), FF (Finish-to-Finish), SF (Start-to-Finish) |
| Go To | Arrow button – navigates the Gantt chart to the related activity |

If no relationships are found for the selected activity, a message is shown.

### Changes over time

Shows the selected activity's key values across all schedules uploaded for the project. Each row is one schedule version (identified by data date).

Columns shown by default: Start, Finish, Total Float (days), Duration. Use the column picker to adjust.

Use this tab to see whether an activity has been delayed, accelerated, or had its float eroded between schedule updates.

### Activity detail

Shows the selected activity's fields in a two-column key-value table. By default this includes every available column except ID; use this tab's column picker to narrow it down. Useful for a full read-out of a specific activity's properties.

## Group Settings

The **Group Settings** dialog (gear icon, top right of the toolbar) controls how activities are grouped and filtered in the Gantt chart. It opens as a pop-up dialog with a **Mode** choice at the top:

| Mode | Effect |
|---|---|
| Group by WBS (default) | Activities are grouped and folded under the schedule's own WBS hierarchy |
| Use custom grouping | Enables the settings below |
| Don't group activities | Flat, ungrouped activity list |

The settings below are only editable when **Use custom grouping** is selected:

| Setting | Options | Default |
|---------|---------|---------|
| Outline Level | 1 / 2 / 3 / 4 | 3 |
| Start | Year / Month / Week | Month |
| Exclude summary activities | On / Off | On |
| Exclude level of effort activities | On / Off | On |
| Group and sort by WBS | On / Off | On |
| Show activity counts | On / Off | On |

**Outline Level** sets how many levels of WBS hierarchy are shown. Level 1 shows only the top-level groupings; Level 4 shows a deeper breakdown.

**Start** groups activities by their current start date – by year, month, or week. Use this to see work clustered by reporting period. (Renamed from "Planned Start" and changed to group by current start rather than baseline planned start – LUSB-1068. If you're comparing against an older baseline schedule, note this follows the schedule's live dates, not the original plan.)

Click **Apply Settings** to apply your selection and close the dialog. Click **Use Default Settings** to reset all options to their defaults and close the dialog.

---

## Column picker

The **Column Selection** button (top right of the toolbar, next to the zoom controls) opens a two-panel picker:

- **Available columns**, grouped into categories – tick a column to add it.
- **Selected columns** – drag to reorder. A few (like ID) are fixed in place.

It also has a date format menu, a duration format menu, and a reset button.

Column categories and examples of what's in each:

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

Default visible columns in the activity list: ID, Activity Name, Start, Finish, Baseline Start, Baseline Finish, At Completion Duration. (Total Float and Critical are available but not shown by default – add them from the picker if you need critical-path visibility at a glance.)
