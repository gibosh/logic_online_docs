---
page-id: gantt-viewer
route: /schedule-viewer/gantt
title: Schedule Viewer
audience: external
status: draft
version: 1.1.0
last-reviewed: 2026-07-14
---

## About the Schedule Viewer

The Schedule Viewer is the central workspace for exploring and comparing construction project schedules. Open it from **Gantt Viewer** in the top navigation bar.

Use the Schedule Viewer to look at individual schedules, compare changes between updates, understand the logic connecting activities, and run delay analysis.

---

## File selection

Choose which schedule files to work with using the two selectors at the top of the page.

**Comparison** — the schedule you are analysing (typically the most recent update). Select from all schedule files uploaded for the current project. Files are labelled by data date.

**Baseline** — an earlier schedule to compare against. When set, each activity in the Gantt chart shows a second bar representing the baseline dates. Select **None** to remove the baseline comparison.

Changing either selector resets any active traceback.

*Full detail coming soon.*

---

## Three modes

The Schedule Viewer operates in one of three modes, selectable from the tabs in the toolbar:

| Mode | Use it to |
|------|-----------|
| **Schedule Analysis** | View the Gantt chart, explore activity relationships, and compare activity dates across schedule versions |
| **Traceback Setup** | Select a start activity, configure the algorithm, and run a traceback analysis |
| **Delay Analysis** | Review traceback results, understand delay attribution, save and load tracebacks |

The mode advances automatically to Delay Analysis when a traceback completes.

For full detail on each mode see:
- [Schedule Analysis mode](pages/schedule-analysis.md) — Gantt chart, activity tray, group settings
- [Traceback Setup mode](pages/traceback-setup.md) — start activity, profile, weights, exclusion criteria
- [Delay Analysis mode](pages/gantt-delay-analysis.md) — traceback results, save/load, restart options

---

## Column selector

The column picker (top right of the toolbar) controls which columns appear in the activity list. Columns can be shown, hidden, and reordered by dragging. The columns available change depending on the active mode.

Default columns include: ID, Activity Name, Start, Finish, Total Float, Critical.

*Full column list detail coming soon.*

---

## Group and Sort

The **Group Settings** panel (gear icon, top right of the toolbar) controls how activities are grouped and filtered in the Gantt chart. Options include outline level (1–4), planned start period (year, month, or week), and filters to exclude summary tasks and level of effort activities.

[Full detail — Group Settings](pages/schedule-analysis.md#group-settings)

---

## Go To

The **Go To** button appears next to related activities in the activity tray. Clicking it scrolls and highlights the Gantt chart to that activity without losing your current selection.

Go To is available in:
- The **Relationships** tab of the activity tray (in Schedule Analysis mode)
- The **Candidate Scoring** tab of the traceback tray (in Delay Analysis mode)

[Full detail — Activity tray](pages/schedule-analysis.md#activity-tray)

---

## Schedule History

The **Changes over time** tab in the activity tray shows the selected activity's key values across every schedule upload for the project — start, finish, total float, and duration — in a single table. Use it to see at a glance whether an activity has been delayed, accelerated, or had its float eroded between updates without opening multiple files.

[Full detail — Activity tray](pages/schedule-analysis.md#activity-tray)

---

## Activity Detail

The **Activity detail** tab in the activity tray shows all available fields for the selected activity in a key-value table. Use it for a full read-out of all properties for a specific activity.

*Full detail coming soon.*
