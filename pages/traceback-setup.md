---
page-id: traceback-setup
route: /delay-analysis/schedule-viewer
title: Gantt Viewer – Traceback Setup
audience: external
status: draft
version: 1.2.0
last-reviewed: 2026-09-23
blocked-reason: Algorithm Profile table is expected to change soon (Replica v2.1 → "Weighted Scoring") — see workspace/GAPS.md "Upcoming release changes to watch for." Not yet updated ahead of that release; reconfirmed 2026-09-23 that this rename still hasn't shipped. "Driving Task" removed as a selectable profile 2026-09-23 (confirmed via `frontend/src/page/GanttViewerPage/GanttModeSelector/profiles.ts` — only Replica v2.1 and Calibrated v1 remain, and the runner now rejects the driving-task-v1 profile server-side) — the underlying calculation still exists and now only powers the Driving Task column/Driving path tab, documented at traceback-engine.md#driving-task. New Calibrated v1 calibration status/retry UX documented from source, not yet screenshot-verified live. Step 1's search box updated for the LUSB-1231 redesign (now the shared toolbar search box, not a mode-specific one).
---

## About this mode

Traceback Setup is the second mode in the [Gantt Viewer](pages/gantt-viewer.md). Use it to configure and run a *traceback analysis* – an algorithm that traces the chain of activities most responsible for project delay, working backward from a selected end activity.

You must have a **Baseline** schedule selected before the traceback will produce meaningful results. The traceback compares the baseline against the **Comparison** schedule to measure delay.

For how the algorithm actually chooses each activity, see [How Traceback and Delay Attribution Work](pages/traceback-engine.md).

## Running a traceback

### Step 1 – Select a start activity

The start activity is the delayed end point you want to trace from – typically the project completion milestone or another critical endpoint.

Use the activity search box in the toolbar (shared with Schedule Viewer mode's activity search) to find the activity by ID or name. The selected activity is highlighted in the Gantt chart. You can also click an activity in the chart and it will populate the search box.

The **Start Traceback** button is disabled until an activity is selected.

### Step 2 – Select an algorithm profile (optional)

The **Algorithm Profile** dropdown lets you choose how the traceback scores and selects candidate activities at each step.

| Profile | Description |
|---------|-------------|
| Replica v2.1 | The validated, corrected rule set – see [How Traceback and Delay Attribution Work](pages/traceback-engine.md). **Default.** |
| Calibrated v1 | The Replica v2.1 rule set, with scoring weights statistically recalibrated against real project data, plus an extra construction-sequence scoring factor (see note below). |

Start with **Replica v2.1** (the default) unless you have a specific reason to use another profile.

**Calibrated v1 needs to be "trained" on your project before you can use it.** Logic+ automatically calibrates it in the background once your project has at least two schedule updates with confirmed data dates – you don't need to start this yourself. Open the **Algorithm Profile** dropdown to see a status line under Calibrated v1 telling you where things stand, for example *"Calibration in progress…"*, *"Calibration ready"*, or a message explaining what's missing (like needing a second schedule update). While training is running you'll see a spinning icon, and the **Start Traceback** button stays disabled for Calibrated v1 until the status says ready. If calibration fails or falls out of date (for example, after you upload a newer schedule), a small refresh icon appears next to the status message – click it to calibrate again.

**There is no longer a "Driving Task" option in this dropdown.** It was removed as a selectable Algorithm Profile on 2026-09-21 or shortly before. The same underlying driving-logic calculation is still very much alive elsewhere – it now powers only the **Driving Task** column in [Activity relationships](pages/schedule-analysis.md#activity-relationships) and the **[Driving path](pages/schedule-analysis.md#driving-path)** tab, both reachable throughout the Schedule Viewer without running a traceback at all. See [Driving Task](pages/traceback-engine.md#driving-task) for what that calculation does.

### Step 3 – Adjust settings (optional)

Click the **settings gear icon** to open **Traceback Settings**. The modal has two tabs.

**Candidate Score Weights**

Controls how much influence each scoring measure has on which activity is selected as the next driver at each step. Weights are set as percentages – higher weight means more influence on the result.

*Primary scores* drive most of the selection decision:

| Score | Default |
|-------|---------|
| Closest Link | 35% |
| Implied Link | 18.75% |

*Secondary scores* refine the ranking when primary scores are close:

| Score | Default |
|-------|---------|
| Finish Variance | 3.75% |
| WBS Structure | 3.75% |
| Activity Name | 3.75% |
| Primary Resource | 6.25% |
| Created Date | 3.75% |
| Sequence Number | 0% |
| Same File | 3.75% |
| Project ID | 3.75% |
| Author | 2.5% |
| Max Work | 3.75% |
| Max Float | 3.75% |
| Complexity | 3.75% |
| Activity Coding | 3.75% |

The defaults represent the recommended starting point. Adjust them if the standard traceback is not correctly identifying the delay drivers for your schedule type. Sequence Number (how close together two activities' internal line numbers are) defaults to 0% – raise it if your schedule's build order is a meaningful signal on its own.

*Calibrated v1 also scores a 16th factor automatically – a match against a construction-sequence dictionary (e.g. "excavate before pour"). It isn't listed above because its weight isn't user-adjustable, but it does appear in the [Traceback Log](pages/traceback-log.md) for a Calibrated v1 run.*

**Activity Exclusion Criteria**

Narrows the pool of candidate activities before scoring. Six filters are available:

| Filter | What it excludes |
|--------|-----------------|
| Finish after start of target by (days) | Candidates finishing more than N days after the target activity starts |
| Finish before start of target by (days) | Candidates finishing more than N days before the target activity starts |
| Duration longer than (days) | Candidates with duration exceeding N days |
| Finish later than target finish by (days) | Candidates finishing more than N days past the target finish; set to Unlimited to include all |
| Contained within target date range | Candidates that start and finish entirely within the target activity's time span |
| LOE or WBS Summary activities | Level of Effort and WBS Summary activity types |

Click **Save** to apply settings and close the modal. Changes take effect on the next traceback run. Click **Cancel** to discard changes.

Logic+ also always excludes a candidate that has already been chosen as a driving activity earlier in the same traceback (**exclude duplicate critical activities**), so the chain can't loop back on itself. This rule isn't adjustable here, but it does show up in the [Traceback Log](pages/traceback-log.md) for a completed run.

### Manually excluding activities

Alongside the automatic filters above, the activity list in Traceback Setup mode has an **Excluded** column with a checkbox on every row. Untick an activity to exclude it from candidate scoring regardless of what the automatic filters decided, or tick it back in to include it. Ticking the checkbox on a WBS summary row bulk-excludes or bulk-includes all of that branch's child activities at once.

LOE and WBS Summary activities are automatically excluded and their checkboxes are disabled while **Exclude LOE or WBS Summary activities** (above) is switched on – turn that filter off first if you need to manually include one of them.

### Step 4 – Start Traceback

Click **Start Traceback**. Logic+ runs the traceback algorithm against the comparison and baseline schedules, honouring the exclusion criteria and any manual exclusions above. When complete, the mode automatically switches to **Delay Analysis**.

If a traceback has already been run and you change the start activity or weights, click **Start Traceback** again to re-run.

Changing the Comparison or Baseline schedule selector resets the traceback.
