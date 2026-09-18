---
page-id: schedule-analysis
route: /schedule-viewer
title: Gantt Viewer – Schedule Viewer mode
audience: external
status: draft
version: 1.3.1
last-reviewed: 2026-09-18
blocked-reason: Updated following the LUSB-1231 Schedule Viewer redesign (2026-09-17/18) – "Jump to" relabel, default active tab change, merged zoom behaviour, toolbar button wording. Further updated same day for feature/gantt-smooth-zoom (LUSB-1063) – zoom-in ceiling, Ctrl/Cmd+scroll smooth zoom, and the new synced scrollbar strip. Exact drag/resize feel (column drag-reorder, panel/column/tray resize handles) verified from component structure only, not tested live. Group Settings "Start"/"Outline Level" labels are a known dev ticket to relabel — see workspace/GAPS.md.
---

## About this mode

**Schedule Viewer** is the default mode in the [Gantt Viewer](pages/gantt-viewer.md) — note it shares its name with the Gantt Viewer page itself; this refers specifically to the first of the three mode tabs. Use it to explore activities, inspect relationships, and compare how activities have changed across schedule versions.

## The Gantt chart

The chart is split into two panels:

- **Left panel** – the activity list with configurable columns. Drag the divider on the right edge of the panel to resize it, or drag the edge of an individual column header to resize just that column.
- **Right panel** – the Gantt bars drawn against a timeline. Use the zoom controls (toolbar) to zoom in, or reset back to a 1x fit-to-width view. Zoom out is limited to that same 1x fit-to-width level; zoom in is capped once the timeline reaches its finest detail level – both zoom buttons grey out once you hit their limit. Hold **Ctrl** (or **Cmd**) and scroll the mouse wheel over the chart to zoom smoothly, centred on your cursor, instead of using the toolbar buttons.

A scrollbar strip below the chart keeps the activity list and timeline panels in sync – drag either half to scroll both, or use your keyboard once it's focused.

WBS summary rows can be expanded or collapsed using the arrow next to the WBS code. Activities are shown with their start and finish dates as horizontal bars. Milestones are shown as diamonds.

When a **Baseline** schedule is selected, each activity shows a thinner bar within the same row, positioned below its current bar, representing the baseline dates. (The activity list's Baseline Start / Baseline Finish columns are visible by default even with no baseline chosen – they just stay blank until you select one.)

**Click any activity** to select it and open the activity tray at the bottom of the page.

## Activity tray

The tray is collapsed by default. Click the **Expand** control in the tray's header to open it (it relabels to **Minimize** while open – this is the only open/close control for the tray). When an activity is selected, the tray shows four tabs – **Activity relationships** is the default active tab – each with its own column picker (top right of the tabs) so you can customise which fields that tab displays. The active activity ID is shown in the tray header. Drag the top edge of the open tray to resize its height.

### Activity relationships

Shows all predecessors and successors of the selected activity.

| Column | Description |
|--------|-------------|
| ID | Activity ID of the related activity |
| Activity Name | Name of the related activity |
| Relationship Type | How the activities are linked – FS (Finish-to-Start), SS (Start-to-Start), FF (Finish-to-Finish), SF (Start-to-Finish) |
| Critical | Whether the related activity sits on Logic+'s calculated critical path |
| Driving Task | Predecessor rows only – whether the schedule's own logic and dates say this predecessor is actually driving the selected activity, independent of any traceback. A red flag means it's the direct driver; an amber flag means it drives indirectly further back along the chain (hover it to see the chain); no icon means it isn't on the driving path; an em-dash means the driving path couldn't be calculated. See [How Traceback and Delay Attribution Work](pages/traceback-engine.md#driving-task) for what "driving" means here. |
| Jump to | Button – navigates the Gantt chart to the related activity. (This same action is labelled **Go To** in the Delay Analysis tray's Candidate Scoring tab – see [Gantt Viewer – Delay Analysis](pages/gantt-delay-analysis.md#candidate-scoring).) |

If no relationships are found for the selected activity, a message is shown.

### Driving path

Shows a Gantt chart of the driving-path chain leading to the selected activity – the same underlying logic as the Driving Task column above, but as a standalone chart rather than a flag on each row. Three checkboxes above the chart, all on by default, control what's traced and drawn:

- **Predecessors** – trace backward from the selected activity
- **Successors** – trace forward from the selected activity
- **Relationship arrows** – draw the links between activities on the path

Switch off Predecessors or Successors to trace in one direction only; switch off both to show just the selected activity with no chain. This tab has its own column picker (default columns: ID, Start, Finish). Select an activity to populate it – with nothing selected, or a WBS summary row selected, it shows a prompt instead of a chart. On larger schedules, a brief "Calculating driving path…" message appears while the chain is worked out.

### Changes over time

Shows the selected activity's key values across all schedules uploaded for the project. Each row is one schedule version (identified by data date).

Columns shown by default: Start, Finish, Total Float (days), Duration. Use the column picker to adjust.

Use this tab to see whether an activity has been delayed, accelerated, or had its float eroded between schedule updates.

### Activity detail

Shows the selected activity's fields in a two-column key-value table. By default this includes every available column except ID; use this tab's column picker to narrow it down. Useful for a full read-out of a specific activity's properties.

## Group Settings

The **Group Settings** dialog (**Chart settings** button, toolbar) controls how activities are grouped and filtered in the Gantt chart. It opens as a pop-up dialog with a **Mode** choice at the top:

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

**Start** groups activities into a Year, Month, or Week bucket. Despite the name, this isn't about an activity's start date specifically – any activity shorter than the bucket size (a year, when grouping by year) lands in the same bucket regardless of which of its dates you'd use to place it. Use this to see work clustered by reporting period.

Click **Apply Settings** to apply your selection and close the dialog. Click **Use Default Settings** to reset all options to their defaults and close the dialog.

---

## Column picker

The **Configure columns** button (toolbar) opens a two-panel picker:

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

The **Predecessors** and **Successors** columns list every linked activity's ID, relationship type, and lag, comma-separated – e.g. `A100: SS (0 days), A200: FS (-0.5 days)`. A negative lag means the two activities overlap rather than one starting after a gap.
