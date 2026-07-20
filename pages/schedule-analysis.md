---
page-id: schedule-analysis
route: /schedule-viewer/gantt
title: Gantt Viewer – Schedule Analysis
audience: external
status: draft
version: 1.0.3
last-reviewed: 2026-06-08
---

## About this mode

Schedule Analysis is the default mode in the [Gantt Viewer](pages/gantt-viewer.md). Use it to explore activities, inspect relationships, and compare how activities have changed across schedule versions.

## The Gantt chart

The chart is split into two panels:

- **Left panel** – the activity list with configurable columns. Drag the divider to resize.
- **Right panel** – the Gantt bars drawn against a timeline

WBS summary rows can be expanded or collapsed using the arrow next to the WBS code. Activities are shown with their start and finish dates as horizontal bars. Milestones are shown as diamonds.

When a **Baseline** schedule is selected, each activity shows a thinner bar below its current bar representing the baseline dates.

**Click any activity** to select it and open the activity tray at the bottom of the page.

## Activity tray

The tray is collapsed by default. Click **Expand** to open it. When an activity is selected, the tray shows three tabs. The active activity ID is shown in the tray header.

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

Shows all available fields for the selected activity in a two-column key-value table. Useful for a full read-out of all properties without filtering.

## Group Settings

The **Group Settings** panel (gear icon, top right of the toolbar) controls how activities are grouped and filtered in the Gantt chart.

| Setting | Options | Default |
|---------|---------|---------|
| Outline Level | 1 / 2 / 3 / 4 | 3 |
| Planned Start | Year / Month / Week | Month |
| Exclude Summary Tasks | On / Off | On |
| Exclude Level of Effort Tasks | On / Off | On |
| Show Task Count | On / Off | On |

**Outline Level** sets how many levels of WBS hierarchy are shown. Level 1 shows only the top-level groupings; Level 4 shows a deeper breakdown.

**Planned Start** groups activities by when they are planned to start – by year, month, or week. Use this to see work clustered by reporting period.

Click **Apply Settings** to apply your selection. Click **Use Default Settings** to reset all options to their defaults.

---

## Column picker

The column picker in the top right controls the columns in the activity list. Available columns:

| Column | Description |
|--------|-------------|
| ID | Activity ID (or WBS code for summary rows) |
| Activity Name | Full activity name |
| Start | Planned start date |
| Finish | Planned finish date |
| Type | Activity type – task dependent, level of effort, or milestone |
| Category | Activity category (if assigned) |
| Total Float | Total float in days |
| Total Slack | Total slack value |
| Critical | Whether the activity is on the critical path (Yes / No) |

Drag columns in the picker to reorder them in the list.
