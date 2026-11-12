---
page-id: schedule-analysis
route: /schedule-viewer
title: Schedule Viewer Detail
audience: external
status: draft
version: 1.5.0
last-reviewed: 2026-09-30
blocked-reason: 2026-09-30 – Group Settings rewritten for LUSB-1125 (v1.2.15–1.2.17): dialog is now "Activity grouping and display settings" – Apply grouping / No grouping, unlimited ordered rules (Group by / Group using / Level / Units / Treat as, interval + custom ranges), activity codes, resources, UDFs groupable; old Mode radio (Group by WBS / custom / none), Outline Level 1–4 and Start Year/Month/Week are gone. Summary/LOE exclusions now default OFF (were On). Verified against GanttSettingsForm.tsx, GanttSettingsContent.tsx, GroupingEditor.tsx, groupingRules.ts, groupingFields.ts, settings.ts, useGroupingSettings.ts (localStorage, not per project). Not yet screenshot-verified. Retitled and reframed 2026-09-25 – was "Gantt Viewer – Schedule Viewer mode", describing Schedule Viewer as "the first of three mode tabs." That framing was corrected sitewide 2026-09-24 (Schedule Viewer and Delay Analysis are two separate modules with separate journeys, not shared modes of one workspace) but this page was missed in that pass despite being linked from 7+ other pages as the canonical Gantt/activity-tray reference – caught in the 2026-09-25 sitewide drift audit. Updated following the LUSB-1231 Schedule Viewer redesign (2026-09-17/18) – "Jump to" relabel, default active tab change, merged zoom behaviour, toolbar button wording. Further updated same day for feature/gantt-smooth-zoom (LUSB-1063) – zoom-in ceiling, Ctrl/Cmd+scroll smooth zoom, and the new synced scrollbar strip. Exact drag/resize feel (column drag-reorder, panel/column/tray resize handles) verified from component structure only, not tested live. Group Settings "Start"/"Outline Level" labels are a known dev ticket to relabel — see workspace/GAPS.md.
---

## About this page

This page is the detailed reference for [Schedule Viewer](pages/gantt-viewer.md)'s Gantt chart and activity tray – how to read the chart, resize and configure it, and what each activity-tray tab shows. Delay Analysis reuses this same Gantt-viewing screen for its own results (see [Delay Analysis](pages/delay-analysis.md)), simply because it also needs to display schedule files visually – that's a shared building block, not a shared journey, so most of what's on this page applies whichever module you're using it from.

## The Gantt chart

The chart is split into two panels:

- **Left panel** – the activity list with configurable columns. Drag the divider on the right edge of the panel to resize it, or drag the edge of an individual column header to resize just that column.
- **Right panel** – the Gantt bars drawn against a timeline. Use the zoom controls (toolbar) to zoom in, or reset back to a 1x fit-to-width view. Zoom out is limited to that same 1x fit-to-width level; zoom in is capped once the timeline reaches its finest detail level – both zoom buttons grey out once you hit their limit. Hold **Ctrl** (or **Cmd**) and scroll the mouse wheel over the chart to zoom smoothly, centred on your cursor, instead of using the toolbar buttons.

A scrollbar strip below the chart keeps the activity list and timeline panels in sync – drag either half to scroll both, or use your keyboard once it's focused.

WBS summary rows can be expanded or collapsed using the arrow next to the WBS code. Activities are shown with their start and finish dates as horizontal bars. Milestones are shown as diamonds.

When a **Baseline** schedule is selected, each activity shows a thinner bar within the same row, positioned below its current bar, representing the baseline dates. (The activity list's Baseline Start / Baseline Finish columns are visible by default even with no baseline chosen – they just stay blank until you select one.)

**Click any activity** to select it and open the activity tray at the bottom of the page.

## Activity tray

The tray is collapsed by default. Click the **Expand** control in the tray's header to open it (it relabels to **Minimize** while open – this is the only open/close control for the tray). When an activity is selected, the tray shows four tabs – **Activity relationships** is the default active tab – each with its own column picker (top right of the tabs) so you can customise which fields that tab displays. The active activity ID is shown in the tray header. Drag the top edge of the open tray to resize its height. Every table's column header stays fixed at the top as you scroll its rows.

### Activity relationships

Shows all predecessors and successors of the selected activity.

| Column | Description |
|--------|-------------|
| ID | Activity ID of the related activity |
| Activity Name | Name of the related activity |
| Relationship Type | How the activities are linked – FS (Finish-to-Start), SS (Start-to-Start), FF (Finish-to-Finish), SF (Start-to-Finish) |
| Critical | Whether the related activity sits on Logic+'s calculated critical path |
| Driving Task | Predecessor rows only – whether the schedule's own logic and dates say this predecessor is actually driving the selected activity, independent of any traceback. A red flag means it's the direct driver; an amber flag means it drives indirectly further back along the chain (hover it to see the chain); no icon means it isn't on the driving path; an em-dash means the driving path couldn't be calculated. See [How the Traceback method works](pages/traceback-engine.md#driving-path) for what "driving" means here. |
| Jump to | Button – navigates the Gantt chart to the related activity. (This same action is labelled **Go To** in the Delay Analysis tray's Activity Scoring tab – see [Traceback Results](pages/gantt-delay-analysis.md#activity-scoring).) |

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

The **Chart settings** button in the toolbar opens **Activity grouping and display settings**. At the top, choose **Apply grouping** to group activities using the rules below, or **No grouping** for a flat activity list.

### Grouping Options

Grouping is built from a list of rules. The first rule makes the top-level groups, the second groups activities within each of those, and so on. By default there's one rule: **WBS**, to **Level 3**.

- **Add grouping** adds a rule. Drag a rule's handle to reorder it, or click its bin button to remove it.
- The same field can be used more than once – for example, WBS to Level 1, then an activity code, then WBS again to a deeper level.

For each rule, choose:

**Group by** – the field to group on. The list is searchable and organised by column category:

- Standard activity fields – dates, durations, float, status, activity type, calendar, critical, variances, costs and more
- **WBS**, **WBS Name** and **WBS Path**
- **Resources** – follows your schedule's resource hierarchy
- **Coding** – every activity code type in the schedule, following each code's own hierarchy
- **User Defined Fields** – your schedule's custom fields
- Traceback fields (Score, Defined Link, Low Confidence, Traced Position) – these show as *Available in Traceback Setup* and can only be used there

**Group using** – the options depend on the kind of field:

| Field | Group using |
|---|---|
| WBS, Resources, activity codes | **Hierarchy level** – then choose a **Level**. Levels deeper than the current schedule goes are marked *(not in this schedule)* |
| Dates | **Day**, **Week**, **Month** (default), **Quarter**, **Year**, or **Custom ranges** |
| Text and Yes / No fields | **Exact values** |
| Numbers, costs, percentages and durations | **Exact values**, **Custom ranges** or **Equal intervals** – plus **Negative / zero / positive** for float and variance fields |

- **Equal intervals** – set an **Interval width** and an **Origin** (where the first interval starts). Use **And above (optional)** to put everything from that value upwards into one final group.
- **Custom ranges** – click **Add range** for each group and fill in **From** and **To**. Leave one end blank for an open-ended range.
- **Units** – for durations, choose **Working hours**, **Working days** or **Working months**.
- **Treat as** – for user defined fields, Logic+ detects the type of data automatically. If it guesses wrong, change it here (Text, Number, Cost, Percentage, Date, Duration, or Yes / No).

If a rule isn't complete, a message shows under it and **Apply settings** stays unavailable until it's fixed.

### Display Options

| Setting | Default | What it does |
|---|---|---|
| Exclude summary activities | Off | Hides WBS summary activities |
| Exclude level-of-effort activities | Off | Hides level-of-effort activities |
| Collapse Groups | Off | Opens the chart with every group folded (only with **Apply grouping**) |
| Show activity count in group bar | On | Shows how many activities are in each group |

Click **Apply settings** to apply your changes and close the dialog. **Reset to default** puts everything back to the defaults above – click **Apply settings** afterwards to use them. **Cancel** closes without changing anything.

Your grouping settings are remembered in this browser and apply to every project you open. While a traceback is still running, its provisional results are always shown as a flat, ungrouped list.

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
