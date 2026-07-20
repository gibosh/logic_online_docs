---
page-id: traceback-setup
route: /schedule-viewer/gantt
title: Gantt Viewer — Traceback Setup
audience: external
status: draft
version: 1.0.3
last-reviewed: 2026-06-08
---

## About this mode

Traceback Setup is the second mode in the [Gantt Viewer](pages/gantt-viewer.md). Use it to configure and run a *traceback analysis* — an algorithm that traces the chain of activities most responsible for project delay, working backward from a selected end activity.

You must have a **Baseline** schedule selected before the traceback will produce meaningful results. The traceback compares the baseline against the **Comparison** schedule to measure delay.

## Running a traceback

### Step 1 — Select a start activity

The start activity is the delayed end point you want to trace from — typically the project completion milestone or another critical endpoint.

Use the activity search box in the mode bar to find the activity by ID or name. The selected activity is highlighted in the Gantt chart. You can also click an activity in the chart and it will populate the search box.

The **Start Traceback** button is disabled until an activity is selected.

### Step 2 — Select an algorithm profile (optional)

The **Algorithm Profile** dropdown lets you choose how the traceback scores and selects candidate activities at each step.

| Profile | Description |
|---------|-------------|
| Desktop (Legacy) | Original scoring from Logic+ Desktop. Default. |
| Replica v2.1 | Ground-truth parity profile using the validated rule set. |
| Calibrated v1 | Replica rule set with calibrated score weights. |

Start with **Desktop (Legacy)** unless you have a specific reason to use another profile.

### Step 3 — Adjust settings (optional)

Click the **settings gear icon** to open **Traceback Settings**. The modal has two tabs.

**Candidate Score Weights**

Controls how much influence each scoring measure has on which activity is selected as the next driver at each step. Weights are set as percentages — higher weight means more influence on the result.

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
| Same File | 3.75% |
| Project ID | 3.75% |
| Author | 2.5% |
| Max Work | 3.75% |
| Max Float | 3.75% |
| Complexity | 3.75% |
| Activity Coding | 3.75% |

The defaults represent the recommended starting point. Adjust them if the standard traceback is not correctly identifying the delay drivers for your schedule type.

**Activity Exclusion Criteria**

Narrows the pool of candidate activities before scoring. Five filters are available:

| Filter | What it excludes |
|--------|-----------------|
| Finish after start of target by (days) | Candidates finishing more than N days after the target activity starts |
| Finish before start of target by (days) | Candidates finishing more than N days before the target activity starts |
| Duration longer than (days) | Candidates with duration exceeding N days |
| Finish later than target finish by (days) | Candidates finishing more than N days past the target finish; set to Unlimited to include all |
| Contained within target date range | Candidates that start and finish entirely within the target activity's time span |
| LOE or WBS Summary activities | Level of Effort and WBS Summary activity types |

Click **Save** to apply settings and close the modal. Changes take effect on the next traceback run. Click **Cancel** to discard changes.

### Step 4 — Start Traceback

Click **Start Traceback**. Logic+ runs the traceback algorithm against the comparison and baseline schedules. When complete, the mode automatically switches to **Delay Analysis**.

If a traceback has already been run and you change the start activity or weights, click **Start Traceback** again to re-run.

Changing the Comparison or Baseline schedule selector resets the traceback.
