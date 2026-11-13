---
page-id: gantt-delay-analysis
route: /delay-analysis/schedule-viewer
title: Traceback Results
audience: external
status: draft
version: 1.6.0
last-reviewed: 2026-11-13
blocked-reason: 2026-11-13 – Delay Attribution panel rewritten for v1.2.37–1.2.40 (LUSB-1374, LUSB-1384, LUSB-1386): opens on two columns with expand/collapse chevrons, keyboard controls, calendar toggle now has two modes (the "Using project calendar" option was removed in v1.2.40 and task working calendars is the default), "Relationship" column is headed By Relation, empty cells are blank, and the row info icon is now a chart icon that opens a Delay explanation modal with scenario mini-Gantts. Verified against GanttDelayAnalysisPanel.tsx, GanttDelayAnalysisHeader.tsx, delayAnalysis.utils.ts, ReasonInfo.tsx, delayScenarios.ts at v1.2.40; not screenshot-verified, and the panel/modal screenshots in this page (if any) predate it. 2026-10-13 – 2026-10-13 – Tray can always be reopened once a traceback exists (LUSB-1298, GanttChartTray.tsx hasTraceback). Driving Path anchor updated. 2026-09-30 – Activity Scoring: noted score column headers are raw score-type ids (OUTLINE-NUMBER = WBS Structure), confirmed from a PM production screenshot; replaced stale "without the full score breakdown" line (LUSB-1234 merged full scores into this tab). LUSB-1234 (merged 2026-09-25) removed the separate "Candidate Scoring" tab entirely, folding its Go To navigation into the renamed "Activity Candidates" tab (now called "Activity Scoring") – tray is down from five tabs to four. The "Schedule revision" column survived but is now labelled "File no." on screen despite showing the same order-of-first-appearance number, not an actual file number (confirmed intentional via the app's own test assertions, not a bug to silently work around). LUSB-1279 (same day) gave every tray/panel table a sticky column header, and moved the bold "weights" summary row into that sticky header area so it now scrolls with the header instead of the data. Restructured 2026-09-24 – this page is now framed as the Traceback method's results screen (part of Delay Analysis), not "the third mode of the Gantt Viewer"; see pages/gantt-viewer.md and pages/delay-analysis.md for the corrected module framing. Mode bar controls section rewritten to match the current app: Save Traceback and Re-run Delay Analysis buttons no longer exist (verified against GanttModeSelector.tsx – Delay Analysis tab now only ever renders the Saved Tracebacks dropdown and View Traceback Log button); saving only happens through the exit-tab confirmation dialog, documented below with its exact wording. Added the new Traceback progress panel, the Schedule revision column (Activity Candidates/Candidate Scoring), and the Delay Attribution panel's resize handle and third synced scrollbar – all verified against source 2026-09-24, not yet screenshot-verified live. Older items still pending screenshot verification: "Driving path" tab (added 2026-09-17); three-way calendar toggle and LUSB-1171 flag set (see traceback-engine.md's blocked-reason for the full flag rename).
---

## About this page

This is where Traceback method results are shown, inside [Delay Analysis](pages/delay-analysis.md). It's the second of the Traceback method's two tabs – **Traceback Setup**, then this one – and becomes active automatically once you [start a traceback](pages/traceback-setup.md#step-4--start-traceback). Use it to explore the traceback results, understand which activities are driving project delay, and review how delay has accumulated over time.

For why the algorithm chose each activity on the path, and how delay is charged to it, see [How the Traceback method works](pages/traceback-engine.md).

### While a traceback is running

A progress panel shows the current stage (Starting traceback → Loading schedules → Tracing driving activities → Saving result → Loading result → Calculating delay analysis), the elapsed time, and a running count of activities found so far. Until the final "Calculating delay analysis" stage, it's also labelled **Provisional path** – the chain shown at that point may still change before the run finishes. There's no cancel or retry control on this panel; if a run fails, it shows **Traceback interrupted** with an error message in place of the activity count.

## The Gantt chart in the Delay Analysis tab

The Gantt chart highlights the traceback path – the chain of driving activities identified by the traceback algorithm. Click any highlighted activity to select it and load its data into the tray below.

## Delay Attribution panel

Separate from the tray, a panel sits alongside the Gantt chart's activity list showing the delay attributed to each activity on the traceback path, row by row. It opens showing its first two columns – **Cumulative Delay** and **Task Delay**. Three buttons in the panel's corner control it:

- **Calendar icon** – switches how the delay figures are measured. Its tooltip reads **"Use calendar days: on"** or **"Use calendar days: off"**, and the icon is faded when calendar days is off.
  - **Using task working calendars** (the default, calendar days off) – each activity's delay is counted in its own working calendar, and cumulative delay is re-measured whenever it passes between activities on different calendars. The Cumulative Delay header's tooltip then reads *"Cumulative delay measured in this activity's current working calendar; inherited delay is remeasured at each handover."*
  - **Using calendar days** (calendar days on) – every figure is plain elapsed calendar days, summed in path order.
- **Left chevrons** (**Show all delay columns**) – widen the panel to show every column. If the panel has been collapsed, this button (**Expand delay analysis panel**) reopens it to the first two columns.
- **Right chevrons** – step back: from all columns to the first two (**Show first two delay columns**), and from the first two to fully hidden (**Collapse delay analysis panel**).

The controls work from the keyboard: Tab into the group, move between buttons with the up and down arrow keys, and press Space or Enter to use one.

**Resize.** Drag the handle on the panel's edge to widen or narrow it against the Gantt chart – it won't shrink below a minimum width. The panel has its own horizontal scrollbar, synced with the Gantt chart's and the activity list's, so scrolling any one of the three keeps all three lined up on the same point in time.

Every table in this panel and in the tray below has a **sticky column header** – it stays fixed at the top as you scroll the rows underneath it.

Each row shows:

| Column | Description |
|--------|-------------|
| Cumulative Delay | Running total of assigned delay up to and including this activity |
| Task Delay | Delay charged to this activity, with a coloured dot showing its share of the overall path delay (green = none, yellow = up to 5%, red = more than 5%) |
| Flag columns | One column per reason the delay was charged, shown as a signed number of days. The columns are **New fragnet**, **CONSTRNT** (constraint), **By Relation**, **Implied Logic**, **Calendar**, **Duration** and **Progress**. A cell with nothing charged is left blank. Hover a column header for its definition. See [How delay gets charged to each activity](pages/traceback-engine.md#from-chain-to-delay--how-days-get-charged-to-each-activity) for what each flag means |

**Delay explanation.** A row that has charges carries a small chart icon. Hover it to see **Show delay explanation**; click it to open a **Delay explanation · [activity ID]** window. It lists every flag raised for that activity (e.g. *Relationship — …*), then draws a small Gantt diagram for each rule that was applied – the same scenarios S01–S19 described in [Delay Attribution 2.0](pages/delay-attribution-2.md), plus K-01 when delay is handed between different calendars. Each diagram has a title, a short explanation and a line showing how the charge arises. Bars are shown as **Current** and **Baseline**, with markers for **New activity**, **Non-driving link**, **Deleted link** and **PC: Practical Completion**.

The diagrams are fixed worked examples of the rule, not drawings of your schedule – their dates and figures illustrate the logic, not this activity's actual numbers. If none of the scenarios applies to the evidence available, the window says *"No supported diagram applies to the available attribution evidence."* Click **Dismiss** to close it.

A row belonging to a group of new (unbaselined) activities carries its own icon, tooltip **"Task group added to traceback (no baseline)"** – see [New activities](pages/traceback-engine.md#from-chain-to-delay--how-days-get-charged-to-each-activity) for why every activity in that group shows the same number rather than a split.

## Delay Analysis tray

The tray is collapsed by default. Click **Expand** to open it – once a traceback has run, the tray stays available to reopen even when no activity is selected. The tray shows four tabs. The active activity ID is shown in the tray header.

### Activity Scoring

Shows the ranked list of candidate activities evaluated at the selected driving task's step in the traceback (this tab was called "Activity Candidates" until a 2026-09-25 update folded a separate, more limited "Candidate Scoring" tab into it – if you're looking for that tab, this is where its **Go To** navigation moved to).

| Column | Description |
|--------|-------------|
| Rank | Position in the scoring – shown as "Rank *N*"; Rank 1 is the selected driving activity for this step |
| File no. | Which schedule update the candidate's data came from, numbered 1 upward in the order that update was first used in this traceback – despite the label, this is not the file's actual filename/file number, its own data date, or upload order |
| Data Date | The data date of the schedule used for this step |
| Activity ID | Candidate activity ID |
| Activity Name | Candidate activity name |
| Score columns | One column per scoring measure, shown as a percentage, in the same order as [Candidate Score Weights](pages/traceback-setup.md#step-3--adjust-settings-optional). Headers use short internal names – e.g. CLOSEST-LINK, IMPLIED-LINK – and **OUTLINE-NUMBER** is the score called **WBS Structure** in Traceback Settings. See Candidate Score Weights for what each one measures |
| Total | Combined weighted score as a percentage |

The header row shows the **maximum weights** applied to each scoring measure, and stays fixed at the top as you scroll. These reflect the settings from Traceback Setup. A **Go To** button on each row navigates the Gantt chart to that candidate.

Select a different driving task in the Gantt chart to update the tray with candidates for that step.

Use this tab to compare every candidate's full score breakdown and jump between them in the Gantt chart.

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

### Driving path

Shows a Gantt chart of the driving-path chain leading to whichever activity is currently selected – this is Logic+'s own driving-logic calculation (the same one behind the [Driving Task](pages/traceback-engine.md#driving-path) algorithm profile), independent of the traceback result shown elsewhere in this tray, so you can compare the two. Three checkboxes above the chart, all on by default, control what's traced and drawn:

- **Predecessors** – trace backward from the selected activity
- **Successors** – trace forward from the selected activity
- **Relationship arrows** – draw the links between activities on the path

Switch off Predecessors or Successors to trace in one direction only; switch off both to show just the selected activity with no chain. This tab has its own column picker (default columns: ID, Start, Finish). Select an activity to populate it – with nothing selected, or a WBS summary row selected, it shows a prompt instead of a chart. On larger schedules, a brief "Calculating driving path…" message appears while the chain is worked out.

## Delay Analysis tab controls

### Load Traceback

The dropdown at the top of the Delay Analysis tab lists all saved tracebacks for the current project. Select one to load it. The **default** traceback (marked with a star) loads automatically when you enter this tab for the project. To set a different default, select a traceback and use the star option in the dropdown. This dropdown only appears once at least one traceback has been saved for the project.

Loading a traceback restores the analysis as it was at the time of saving. If the underlying schedule has changed since the save, the loaded traceback reflects the original data, not the updated schedule.

### View Traceback Log

The **View Traceback Log** button (log icon) opens the full log for the current traceback at a separate page. The log shows the complete sequence of driving activities and scoring decisions made during the traceback run.

Disabled until a traceback has been completed.

[Full detail – Traceback Log](pages/traceback-log.md)

### Saving a traceback

**There's no standalone Save button on this tab.** A traceback is saved when you leave it – switch to the **Traceback Setup** tab (or navigate away) while the current result hasn't already been saved, and Logic+ asks first:

> **Exit Delay Analysis?** Are you sure you want to exit Delay Analysis and switch to the selected tab?

Tick **Save traceback?** to keep it – this reveals a **Name** field (defaults to the start activity's name, or "Default traceback" if none) and a **Set as default** checkbox. Confirming saves and switches (**Save and exit**); leaving the checkbox off and confirming discards the result and switches anyway (**Exit without saving**); **Resume analysis** cancels and stays on this tab. If the current result is already saved, switching tabs skips this prompt entirely.

There's no dedicated "re-run" control either – to refresh a delay analysis against a newer schedule upload, return to **Traceback Setup** and start the traceback again.

---

## Restarting a traceback

At any step in the traceback, you can change which activity the algorithm selected as the next driver and restart the analysis from that point. Two options are available from the Gantt chart in the Delay Analysis tab. While a restart is running, a status indicator shows it's processing.

### Restart from shortlisted candidate

Click a highlighted activity on the traceback path in the Gantt chart to select it. In the **Activity Scoring** tab in the tray, the ranked list of candidates for that step is shown. Select an alternative candidate from the list and restart from that activity. The traceback reruns from that point forward.

### Restart from other activity

Search for any activity in the schedule by ID, name, or WBS outline number using the restart combobox. Select an activity and restart the traceback from that point. Use this to explore alternative delay paths not in the ranked shortlist.
