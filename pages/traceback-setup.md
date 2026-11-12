---
page-id: traceback-setup
route: /delay-analysis/schedule-viewer
title: Traceback Setup
audience: external
status: draft
version: 1.5.0
last-reviewed: 2026-10-13
blocked-reason: 2026-10-13 – "Driving Path" is a selectable Algorithm Profile again (LUSB-1299, e26b6af20, v1.2.18+; profiles.ts value driving-task-v1, label "Driving Path"). Replaced the "no longer a Driving Task option" paragraph. Search placeholder "Select Driving Path target" (ScheduleViewerPage.tsx). Weights/exclusions remain editable in the modal but are ignored – traceback_runner compute.ts bypasses candidate scoring for this profile (flagged in GAPS). 2026-09-30 – Candidate Score Weights expanded with a plain-language description of every score, verified against `@lware/traceback/src/measure/`, `scores/` and `profiles/` (Default = `replica-v2.1` rules: IDF activity-name matching, candidate's own total float for Max Float). Table order kept to match the app (settings modal `scoring.ts` and the Activity Scoring tab use the same order); traceback-engine.md reordered to match. Removed the Calibrated "extra construction-sequence factor" claim – `sequenceDictionary: false` in both Default and Calibrated (`profiles.ts`, commit 27babf324 "Turn off sequence dictionary"). Max Work/Complexity worded neutrally pending an engineering answer on scoring direction – see workspace/GAPS.md 2026-09-30. Swept two leftover "mode" references caught in the 2026-09-25 sitewide drift audit. Reframed 2026-09-24 – this is the Traceback method's setup step within Delay Analysis, not "the second mode of the Gantt Viewer"; see pages/gantt-viewer.md and pages/delay-analysis.md for the corrected module framing. Algorithm Profile names shipped 2026-09-2x as "Default" and "Calibrated" (was Replica v2.1 / Calibrated v1) – confirmed against `frontend/src/page/GanttViewerPage/GanttModeSelector/profiles.ts`, table updated below. This is a different rename from the still-pending "Weighted Scoring" rename tracked in workspace/GAPS.md, which has NOT shipped – don't confuse the two. "Driving Task" removed as a selectable profile 2026-09-23 – the underlying calculation still exists and now only powers the Driving Task column/Driving path tab, documented at traceback-engine.md#driving-path. New calibration status/retry UX and the Traceback Settings modal's own profile switcher documented from source, not yet screenshot-verified live. Step 1's search box updated for the LUSB-1231 redesign (now the shared toolbar search box, not a mode-specific one).
---

## About this page

Traceback Setup is where you configure and run a *traceback analysis* for the Traceback method, one of the methods available under [Delay Analysis](pages/delay-analysis.md) – an algorithm that traces the chain of activities most responsible for project delay, working backward from a selected end activity. Running it switches automatically to [Traceback Results](pages/gantt-delay-analysis.md).

You must have a **Baseline** schedule selected before the traceback will produce meaningful results. The traceback compares the baseline against the **Comparison** schedule to measure delay.

For how the algorithm actually chooses each activity, see [How the Traceback method works](pages/traceback-engine.md).

## Running a traceback

### Step 1 – Select a start activity

The start activity is the delayed end point you want to trace from – typically the project completion milestone or another critical endpoint.

Use the activity search box in the toolbar (shared with Schedule Viewer's own activity search) to find the activity by ID or name. The selected activity is highlighted in the Gantt chart. You can also click an activity in the chart and it will populate the search box.

The **Start Traceback** button is disabled until an activity is selected.

### Step 2 – Select an algorithm profile (optional)

The **Algorithm Profile** dropdown, in the toolbar next to **Start Traceback**, lets you choose how the traceback scores and selects candidate activities at each step. Changing it here applies straight away.

| Profile | Description |
|---------|-------------|
| Default | The validated, corrected rule set – see [How the Traceback method works](pages/traceback-engine.md). **Default.** (Renamed from "Replica v2.1" – same rule set, same underlying value `replica-v2.1`.) |
| Calibrated | The Default rule set, with scoring weights statistically recalibrated against real project data (see note below). (Renamed from "Calibrated v1".) |
| Driving Path | Follows the schedule's own driving relationships back from the activity you select, in the current schedule – no scoring. See [Driving Path](pages/traceback-engine.md#driving-path). |

Start with **Default** unless you have a specific reason to use another profile.

**Calibrated needs to be "trained" on your project before you can use it.** Logic+ automatically calibrates it in the background once your project has at least two schedule updates with confirmed data dates – you don't need to start this yourself. Open the **Algorithm Profile** dropdown to see a status line under Calibrated telling you where things stand, for example *"Calibration in progress…"*, *"Calibration ready"*, or a message explaining what's missing (like needing a second schedule update). While training is running you'll see a spinning icon, and the **Start Traceback** button stays disabled for Calibrated until the status says ready. If calibration fails or falls out of date (for example, after you upload a newer schedule), a small refresh icon appears next to the status message – click it to calibrate again.

**The same profile switcher also appears at the top of the Traceback Settings modal** (Step 3, below) – it's the same control, just reachable from a second place. The difference is when it takes effect: changing the profile here in the toolbar applies immediately, while changing it inside Traceback Settings only applies when you click **Save** there (or is discarded on **Cancel**), alongside any weight or exclusion changes made in the same modal.

**Driving Path** works differently from the other two. Instead of scoring candidates, it follows whichever predecessor the schedule's own logic and dates say is actually driving each activity – the same calculation behind the **Driving Task** column and **Driving path** tab elsewhere in Logic+. With Driving Path selected, the toolbar search box asks you to **Select Driving Path target** – the activity to trace back from. Candidate Score Weights and Activity Exclusion Criteria (Step 3) don't apply to it.

### Step 3 – Adjust settings (optional)

Click the **settings gear icon** to open **Traceback Settings**. The modal has two tabs.

**Candidate Score Weights**

At each step of a traceback, Logic+ scores every candidate activity against the measures below and picks the one with the highest total as the next driver. Each weight is the **most points that score can add** to a candidate's total – the defaults add up to 100, so Closest Link can add up to 35 points and Author up to 2.5. Setting a weight to 0% switches that score off.

Scoring happens in two stages:

1. **Primary scores** are worked out for every eligible candidate, and the top 10 go through to a shortlist.
2. **Secondary scores** are then added for the shortlisted candidates only, and the highest total wins.

So secondary scores can decide between strong candidates, but can't lift a weak one onto the shortlist. Secondary scores are also **relative**: on each measure, the best-matching candidate at that step gets the full weight and the others get stepped-down shares in rank order – which is why you'll often see several candidates on the same value in the [Activity Scoring](pages/gantt-delay-analysis.md#activity-scoring) tab.

*Primary scores – direct evidence that one activity drove another:*

| Score | What it does | Default |
|---|---|---|
| Closest Link | Checks whether the schedule has a logic link between the candidate and the current activity, and how tightly the dates follow it. Finish-Start links count the most and Start-Finish the least; the score tapers off as the working-day gap between the two widens. No link means this score is zero. | 35% |
| Implied Link | Measures how well the candidate's dates line up with the current activity's in a way that looks like a driving relationship, whether or not they're linked. Gaps are counted in working days, so an ordinary Friday finish and Monday start isn't a gap, and a candidate that overlaps instead of finishing first is marked down. The score is then adjusted for how recent the candidate's schedule update is, whether the candidate had actually finished by its data date, and whether the two activities were linked in earlier schedule updates. | 18.75% |

*Secondary scores – supporting evidence used to choose between shortlisted candidates:*

| Score | What it does | Default |
|---|---|---|
| Finish Variance | Compares how far each activity has slipped from its baseline finish. Candidates that have slipped by almost exactly the same amount as the current activity score; others score nothing on this measure. | 3.75% |
| WBS Structure | Compares where the two activities sit in the Work Breakdown Structure, level by level from the top. The more levels they share, the higher the score. | 3.75% |
| Activity Name | Looks for words the two activity names share. Distinctive words – an area, asset or system reference like "SP0913" – count far more than common ones, and very generic words such as "install" and "works" are ignored. | 3.75% |
| Primary Resource | Checks how much of the current activity's resource work is done by resources that are also assigned to the candidate. Work done by the same crew is usually sequenced together. | 6.25% |
| Created Date | Compares when the two activities were first added to the schedule. The closer together, the higher the score – activities created together often belong to the same package of work. | 3.75% |
| Sequence Number | Compares how close together the two activities' internal ID numbers are. A weak signal, so it's off by default – raise it only if your schedule was built in the same order as the work. | 0% |
| Same File | Compares which schedule update each activity comes from. Activities from the same update score highest, and the score drops the further apart the updates are. Only matters when a traceback spans several updates. | 3.75% |
| Project ID | Scores when both activities belong to the same project, so Logic+ doesn't link unrelated projects loaded together. | 3.75% |
| Author | Scores when the same person created both activities. | 2.5% |
| Max Work | Compares the total work built up along the candidate's chain of linked predecessors with the current activity's. | 3.75% |
| Max Float | Looks at the candidate's own total float. The candidate with the least float at that step – the most critical – scores highest. | 3.75% |
| Complexity | Compares how many activities deep the candidate's chain of linked predecessors runs with the current activity's. | 3.75% |
| Activity Coding | Checks how many of the current activity's activity code values (such as discipline, area or phase) the candidate shares. | 3.75% |

**When to adjust the weights.** Start with the defaults, and only change them if the traceback keeps picking activities you know aren't the real drivers. For example:

- If your schedule is poorly linked, the dates and supporting evidence need to do more of the work, so consider raising Implied Link, WBS Structure or Activity Name.
- If your schedule uses activity codes consistently, raise Activity Coding.
- If a score is misleading on your project – say one person authored the whole schedule – set it to 0%.

To see how every candidate scored at a step, use the [Activity Scoring](pages/gantt-delay-analysis.md#activity-scoring) tab or the [Traceback Log](pages/traceback-log.md). For the reasoning behind the two-stage approach, see [How the Traceback method works](pages/traceback-engine.md#the-two-stage-scoring-process).

**With Calibrated selected (here or in the toolbar), every weight field above becomes read-only**, showing the weights Logic+ has actually learned for your project instead of the editable defaults – to change them, recalibrate the profile rather than typing new numbers in.

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

Click **Start Traceback**. Logic+ runs the traceback algorithm against the comparison and baseline schedules, honouring the exclusion criteria and any manual exclusions above. When complete, the screen automatically switches to the **Delay Analysis** tab.

If a traceback has already been run and you change the start activity or weights, click **Start Traceback** again to re-run.

Changing the Comparison or Baseline schedule selector resets the traceback.
