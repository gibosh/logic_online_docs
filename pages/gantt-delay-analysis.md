---
page-id: gantt-delay-analysis
route: /delay-analysis/schedule-viewer
title: Traceback Results
audience: external
status: draft
version: 1.8.0
last-reviewed: 2026-11-23
blocked-reason: 2026-11-23 – Driving path tab: profile is Driving Path (was written as Driving Task). Activity Scoring rename note reworded without the date. Humanizer rewrite of the prose; linked headings kept verbatim. 2026-11-16 – Added "How Logic+ decides what caused the delay" with a Go to link to Delay Attribution Scenarios (formerly the Next Release page, now folded into Delay Analysis). 2026-11-13 – 2026-11-13 – Delay Attribution panel rewritten for v1.2.37–1.2.40 (LUSB-1374, LUSB-1384, LUSB-1386): opens on two columns with expand/collapse chevrons, keyboard controls, calendar toggle now has two modes (the "Using project calendar" option was removed in v1.2.40 and task working calendars is the default), "Relationship" column is headed By Relation, empty cells are blank, and the row info icon is now a chart icon that opens a Delay explanation modal with scenario mini-Gantts. Verified against GanttDelayAnalysisPanel.tsx, GanttDelayAnalysisHeader.tsx, delayAnalysis.utils.ts, ReasonInfo.tsx, delayScenarios.ts at v1.2.40; not screenshot-verified, and the panel/modal screenshots in this page (if any) predate it. 2026-10-13 – 2026-10-13 – Tray can always be reopened once a traceback exists (LUSB-1298, GanttChartTray.tsx hasTraceback). Driving Path anchor updated. 2026-09-30 – Activity Scoring: noted score column headers are raw score-type ids (OUTLINE-NUMBER = WBS Structure), confirmed from a PM production screenshot; replaced stale "without the full score breakdown" line (LUSB-1234 merged full scores into this tab). LUSB-1234 (merged 2026-09-25) removed the separate "Candidate Scoring" tab entirely, folding its Go To navigation into the renamed "Activity Candidates" tab (now called "Activity Scoring") – tray is down from five tabs to four. The "Schedule revision" column survived but is now labelled "File no." on screen despite showing the same order-of-first-appearance number, not an actual file number (confirmed intentional via the app's own test assertions, not a bug to silently work around). LUSB-1279 (same day) gave every tray/panel table a sticky column header, and moved the bold "weights" summary row into that sticky header area so it now scrolls with the header instead of the data. Restructured 2026-09-24 – this page is now framed as the Traceback method's results screen (part of Delay Analysis), not "the third mode of the Gantt Viewer"; see pages/gantt-viewer.md and pages/delay-analysis.md for the corrected module framing. Mode bar controls section rewritten to match the current app: Save Traceback and Re-run Delay Analysis buttons no longer exist (verified against GanttModeSelector.tsx – Delay Analysis tab now only ever renders the Saved Tracebacks dropdown and View Traceback Log button); saving only happens through the exit-tab confirmation dialog, documented below with its exact wording. Added the new Traceback progress panel, the Schedule revision column (Activity Candidates/Candidate Scoring), and the Delay Attribution panel's resize handle and third synced scrollbar – all verified against source 2026-09-24, not yet screenshot-verified live. Older items still pending screenshot verification: "Driving path" tab (added 2026-09-17); three-way calendar toggle and LUSB-1171 flag set (see traceback-engine.md's blocked-reason for the full flag rename).
---

## About this page

This page covers the results of the Traceback method in [Delay Analysis](pages/delay-analysis.md). It's the second of the Traceback method's two tabs, after **Traceback Setup**, and opens automatically once you [start a traceback](pages/traceback-setup.md#step-4--start-traceback). Use it to explore the results, see which activities are driving project delay, and review how delay has built up over time.

For why the algorithm chose each activity on the path, and how delay is charged to it, see [How the Traceback method works](pages/traceback-engine.md).

### While a traceback is running

A progress panel shows the current stage (Starting traceback → Loading schedules → Tracing driving activities → Saving result → Loading result → Calculating delay analysis), the elapsed time and a running count of the activities found so far. Until the final "Calculating delay analysis" stage, it's also labelled **Provisional path**, because the chain may still change before the run finishes. The panel has no cancel or retry control. If a run fails, it shows **Traceback interrupted** with an error message in place of the activity count.

## The Gantt chart in the Delay Analysis tab

The Gantt chart highlights the traceback path, the chain of driving activities the traceback found. Click a highlighted activity to select it and load its data into the tray below.

## Delay Attribution panel

A panel beside the Gantt chart's activity list, separate from the tray, shows the delay attributed to each activity on the traceback path, row by row. It opens with its first two columns, **Cumulative Delay** and **Task Delay**. Three buttons in the panel's corner control it:

- **Calendar icon**: switches how the delay figures are measured. Its tooltip reads **"Use calendar days: on"** or **"Use calendar days: off"**, and the icon is faded when calendar days is off.
  - **Using task working calendars** (the default, with calendar days off): each activity's delay is counted in its own working calendar, and cumulative delay is measured again whenever it passes between activities on different calendars. The Cumulative Delay header's tooltip then reads *"Cumulative delay measured in this activity's current working calendar; inherited delay is remeasured at each handover."*
  - **Using calendar days** (calendar days on): every figure is plain elapsed calendar days, added up in path order.
- **Left chevrons** (**Show all delay columns**): widen the panel to show every column. If the panel is collapsed, this button (**Expand delay analysis panel**) reopens it with the first two columns.
- **Right chevrons**: step back, from all columns to the first two (**Show first two delay columns**), and from the first two to fully hidden (**Collapse delay analysis panel**).

The controls work from the keyboard: Tab into the group, move between buttons with the up and down arrow keys, and press Space or Enter to use one.

**Resize.** Drag the handle on the panel's edge to widen or narrow it against the Gantt chart, down to a minimum width. The panel has its own horizontal scrollbar, kept in step with the Gantt chart's and the activity list's, so scrolling any of the three keeps all three on the same point in time.

Every table in this panel and in the tray below has a **sticky column header** that stays at the top as you scroll the rows.

Each row shows:

| Column | Description |
|--------|-------------|
| Cumulative Delay | Running total of assigned delay up to and including this activity |
| Task Delay | Delay charged to this activity, with a coloured dot showing its share of the overall path delay (green = none, yellow = up to 5%, red = more than 5%) |
| Flag columns | One column per reason the delay was charged, shown as a signed number of days. The columns are **New fragnet**, **CONSTRNT** (constraint), **By Relation**, **Implied Logic**, **Calendar**, **Duration** and **Progress**. A cell with nothing charged is left blank. Hover a column header for its definition. See [How delay gets charged to each activity](pages/traceback-engine.md#from-chain-to-delay--how-days-get-charged-to-each-activity) for what each flag means. |

**Delay explanation.** A row with charges has a small chart icon. Hovering it shows **Show delay explanation**, and clicking it opens a **Delay explanation · [activity ID]** window. The window lists every flag raised for that activity (for example *Relationship — …*), then draws a small Gantt diagram for each rule applied: the scenarios S01–S19 described in [Delay Attribution Scenarios](pages/delay-attribution-2.md), plus K-01 when delay passes between different calendars. Each diagram has a title, a short explanation and a line showing how the charge arises. Bars are labelled **Current** and **Baseline**, with markers for **New activity**, **Non-driving link**, **Deleted link** and **PC: Practical Completion**.

The diagrams are fixed worked examples of each rule, not drawings of your schedule, so their dates and figures illustrate the logic, not this activity's numbers. If none of the scenarios fits the evidence available, the window says *"No supported diagram applies to the available attribution evidence."* Click **Dismiss** to close it.

A row in a group of new (unbaselined) activities has its own icon, with the tooltip **"Task group added to traceback (no baseline)"**. See [New activities](pages/traceback-engine.md#from-chain-to-delay--how-days-get-charged-to-each-activity) for why every activity in that group shows the same number instead of a split.

### How Logic+ decides what caused the delay

Each day of movement at Practical Completion is charged to one of seven categories, the flag columns above. Logic+ works this out by comparing the baseline and current schedules and matching what changed against a set of standard situations: scenarios **S01–S19**, plus **K-01** for delay passing between activities on different calendars. Each scenario has its own rule for which activity is charged and by how much. For example, new scope is charged by its actual effect on the finish date, never by how long the new work is.

> **Go to [Delay Attribution Scenarios](pages/delay-attribution-2.md)** for every scenario with a diagram, the rule behind it and a worked charge table.

## Delay Analysis tray

The tray is collapsed by default. Click **Expand** to open it. Once a traceback has run, you can reopen the tray even when no activity is selected. The tray has four tabs, and its header shows the selected activity's ID.

### Activity Scoring

Shows the ranked list of candidate activities considered at the selected driving task's step of the traceback. This tab was previously called "Activity Candidates", and it now includes the **Go To** navigation from the old "Candidate Scoring" tab.

| Column | Description |
|--------|-------------|
| Rank | Position in the scoring, shown as "Rank *N*". Rank 1 is the selected driving activity for this step. |
| File no. | Which schedule update the candidate's data came from, numbered from 1 in the order the traceback first used each update. Despite the label, it isn't the file's name, data date or upload order. |
| Data Date | The data date of the schedule used for this step |
| Activity ID | Candidate activity ID |
| Activity Name | Candidate activity name |
| Score columns | One column per scoring measure, shown as a percentage, in the same order as [Candidate Score Weights](pages/traceback-setup.md#step-3--adjust-settings-optional). Headers use short internal names such as CLOSEST-LINK and IMPLIED-LINK, and **OUTLINE-NUMBER** is the score called **WBS Structure** in Traceback Settings. See Candidate Score Weights for what each one measures. |
| Total | Combined weighted score as a percentage |

The header row shows the **maximum weight** of each scoring measure, taken from the Traceback Setup settings, and stays at the top as you scroll. A **Go To** button on each row moves the Gantt chart to that candidate.

Select a different driving task in the Gantt chart to show the candidates for that step.

Use this tab to compare every candidate's full score breakdown and move between them in the Gantt chart.

### Changes over time

Shows the selected candidate activity's key values in every schedule uploaded to the project, one row per schedule version, identified by data date.

The default columns are Start, Finish, Total Float (days) and Duration, and the column picker adds others.

### Cumulative Delay

A line chart of how delay has built up along the traceback path.

- **X-axis**: the project timeline, from the earliest driving activity to the latest
- **Y-axis**: cumulative delay in days at each step of the traceback

A rising line means delay is building up. A flat or falling line means the project is holding steady or recovering at that point.

**Hover a point** to see the activity ID, the cumulative delay at that date and the baseline delta (how much delay this step added).

**Click a point** to select the driving activity at that step in the Gantt chart and show its data in every tray tab.

### Driving path

Shows a Gantt chart of the driving-path chain leading to the selected activity. It uses Logic+'s own driving-logic calculation (the same one behind the [Driving Path](pages/traceback-engine.md#driving-path) algorithm profile), independent of the traceback result in the rest of the tray, so you can compare the two. Three checkboxes above the chart, all on by default, control what's traced and drawn:

- **Predecessors** traces backward from the selected activity.
- **Successors** traces forward from the selected activity.
- **Relationship arrows** draws the links between activities on the path.

Switch off Predecessors or Successors to trace in one direction only, or both to show just the selected activity. This tab has its own column picker (default columns: ID, Start, Finish). Select an activity to fill it; with nothing selected, or with a WBS summary row selected, it shows a prompt instead of a chart. On larger schedules, "Calculating driving path…" shows briefly while Logic+ works out the chain.

## Delay Analysis tab controls

### Load Traceback

The dropdown at the top of the Delay Analysis tab lists the saved tracebacks for the current project. Select one to load it. The **default** traceback, marked with a star, loads automatically when you open this tab for the project. To make another traceback the default, select it and use the star option in the dropdown. The dropdown appears only once at least one traceback has been saved for the project.

A loaded traceback is restored as it was when saved. If the schedule has changed since then, the loaded traceback still shows the original data.

### View Traceback Log

The **View Traceback Log** button (log icon) opens the full log for the current traceback on a separate page. The log shows the complete sequence of driving activities and scoring decisions in the traceback run.

It's disabled until a traceback has finished.

[Full detail: Traceback Log](pages/traceback-log.md)

### Saving a traceback

**This tab has no separate Save button.** You save a traceback when you leave it. If the current result hasn't been saved and you switch to the **Traceback Setup** tab or navigate away, Logic+ asks first:

> **Exit Delay Analysis?** Are you sure you want to exit Delay Analysis and switch to the selected tab?

Tick **Save traceback?** to keep it. This shows a **Name** field, which defaults to the start activity's name (or "Default traceback" if it has none), and a **Set as default** checkbox. Confirming saves the result and switches tab (**Save and exit**). If you leave the checkbox off and confirm, the result is discarded and the tab switches anyway (**Exit without saving**). **Resume analysis** cancels and keeps you on this tab. If the current result is already saved, switching tabs skips the prompt.

There's also no re-run control. To refresh a delay analysis against a newer schedule upload, go back to **Traceback Setup** and start the traceback again.

## Restarting a traceback

At any step, you can change which activity the algorithm chose as the next driver and restart the analysis from that point. The Gantt chart in the Delay Analysis tab has two ways to do this, and a status indicator shows while a restart is running.

### Restart from shortlisted candidate

Click a highlighted activity on the traceback path in the Gantt chart to select it. The **Activity Scoring** tab in the tray shows the ranked candidates for that step. Select a different candidate from the list and restart from that activity. The traceback runs again from that point forward.

### Restart from other activity

Use the restart combobox to search for any activity in the schedule by ID, name or WBS outline number. Select an activity and restart the traceback from that point. Use this to explore delay paths that aren't in the ranked shortlist.
