---
page-id: gantt-delay-analysis
route: /schedule-viewer/gantt
title: Gantt Viewer – Delay Analysis
audience: external
status: draft
version: 1.0.3
last-reviewed: 2026-06-08
---

## About this mode

Delay Analysis is the third mode in the [Gantt Viewer](pages/gantt-viewer.md). It becomes active automatically when a [traceback](pages/traceback-setup.md) completes. Use it to explore the traceback results, understand which activities are driving project delay, and review how delay has accumulated over time.

For why the algorithm chose each activity on the path, and how delay is charged to it, see [How Traceback and Delay Attribution Work](pages/traceback-engine.md).

## The Gantt chart in Delay Analysis mode

The Gantt chart highlights the traceback path – the chain of driving activities identified by the traceback algorithm. Click any highlighted activity to select it and load its data into the tray below.

## Delay Analysis tray

The tray is collapsed by default. Click **Expand** to open it. The tray shows four tabs. The active activity ID is shown in the tray header.

### Activity Candidates

Shows the ranked list of candidate activities evaluated at the selected driving task's step in the traceback.

| Column | Description |
|--------|-------------|
| Rank | Position in the scoring – Rank 1 is the selected driving activity for this step |
| Data Date | The data date of the schedule used for this step |
| Activity ID | Candidate activity ID |
| Activity Name | Candidate activity name |
| Score columns | One column per scoring measure, shown as a percentage |
| Total | Combined weighted score as a percentage |

The first row shows the **maximum weights** applied to each scoring measure. These reflect the settings from Traceback Setup.

Select a different driving task in the Gantt chart to update the tray with candidates for that step.

### Candidate Scoring

A simplified view of the candidates for the selected step. Shows rank, activity ID, and a **Go To** button to navigate the Gantt chart to that candidate.

Use this tab to quickly navigate between candidates without the full score breakdown.

### Changes over time

Shows the selected candidate activity's key values across all schedules uploaded for the project. Each row is one schedule version identified by data date.

Columns shown by default: Start, Finish, Total Float (days), Duration. Use the column picker to adjust.

### Cumulative Delay

A line chart showing how delay has accumulated along the traceback path.

- **X-axis** – project timeline from the earliest driving activity to the latest
- **Y-axis** – cumulative delay in days at each step along the traceback

A rising line means delay is accumulating. A flat or falling line means the project is holding or recovering at that point.

**Hover any point** to see the activity ID, cumulative delay at that date, and the baseline delta (how much delay was added at this step).

**Click any point** to navigate to and select the driving activity at that step in the Gantt chart and update all tray tabs to show that activity's data.

## Mode bar controls

### Save Traceback

When you exit Delay Analysis mode with an unsaved traceback, Logic+ prompts you to save it. Enter a name for the save when prompted. Multiple saves can exist for the same project.

Saved tracebacks are stored inside the Logic+ project database – no external file is created.

### Load Traceback

The dropdown in the Delay Analysis mode bar lists all saved tracebacks for the current project. Select one to load it. The **default** traceback (marked with a star) loads automatically when you enter Delay Analysis mode for the project. To set a different default, select a traceback and use the star option in the dropdown.

Loading a traceback restores the analysis as it was at the time of saving. If the underlying schedule has changed since the save, the loaded traceback reflects the original data, not the updated schedule.

### Re-run Delay Analysis

The **Re-run Delay Analysis** button (refresh icon) reloads the current saved traceback result. Use this after a new schedule upload to refresh the delay data against the updated schedule. Disabled until a traceback has been saved.

### View Traceback Log

The **View Traceback Log** button (log icon) opens the full log for the current traceback at a separate page. The log shows the complete sequence of driving activities and scoring decisions made during the traceback run.

Disabled until a traceback has been completed.

### Exit Traceback

The **Exit Traceback** button (X icon) switches back to Traceback Setup mode. If the current traceback has not been saved, Logic+ prompts to save or discard before switching.

---

## Restarting a traceback

At any step in the traceback, you can change which activity the algorithm selected as the next driver and restart the analysis from that point. Two options are available from the Gantt chart in Delay Analysis mode:

### Restart from shortlisted candidate

Click a highlighted activity on the traceback path in the Gantt chart to select it. In the **Activity Candidates** tab in the tray, the ranked list of candidates for that step is shown. Select an alternative candidate from the list and restart from that activity. The traceback reruns from that point forward.

### Restart from other activity

Search for any activity in the schedule by ID, name, or WBS outline number using the restart combobox. Select an activity and restart the traceback from that point. Use this to explore alternative delay paths not in the ranked shortlist.
