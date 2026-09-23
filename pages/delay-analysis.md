---
page-id: delay-analysis
route: /delay-analysis
title: Delay Analysis
audience: external
status: complete
version: 1.2.1
last-reviewed: 2026-09-23
blocked-reason: "Delay Attribute" flag list was still describing the pre-LUSB-1171 flag names (relationship/duration/start-date/lag/no-link/calendar/new) — corrected 2026-09-23 to match the shipped 7-flag set documented in traceback-engine.md.
---

## About Delay Analysis

Delay Analysis is where you access Logic+'s different delay-analysis methods. Open it from **Delay Analysis** in the left-hand navigation, and choose which method you want:

| Method | Use it to |
|---|---|
| **Traceback** (this page) | Trace the chain of driving activities backward from a selected endpoint, attribute delay to each link in that chain, and see how the delay picture has built up over time |
| **[Time-Impact Analysis](pages/time-impact-analysis.md)** | Build a hypothetical delay event, insert it into the schedule, and see its calculated impact on Practical Completion |

More methods may be added here over time. The rest of this page covers the Traceback method.

Traceback identifies which activities in a project schedule are responsible for pushing the completion date later, and by how much. Selecting it opens the [Schedule Viewer](pages/gantt-viewer.md) on its **Traceback Setup** mode – there are two stages, setting up and running the traceback in Traceback Setup mode, then reviewing the results in **Delay Analysis** mode. (You can also reach these same two modes directly from inside the Schedule Viewer itself, using its own mode switcher – both paths lead to the same screen.)

---

## Traceback Setup

Before running a delay analysis, select a start activity (the endpoint you are tracing from), choose an algorithm profile, and optionally adjust the candidate scoring weights and activity exclusion criteria.

[Full detail – Traceback Setup mode](pages/traceback-setup.md)

### Traceback Settings

The settings modal (gear icon in the Traceback Setup toolbar) has two tabs:

**Candidate Score Weights** – adjust how much influence each measure has on which activity is selected as the next driver at each step. Primary scores (Closest Link, Implied Link) drive most of the decision. Thirteen secondary scores refine the ranking when primary scores are close – this only applies to the Replica v2.1 and Calibrated v1 algorithm profiles; the Driving Task profile ignores these weights entirely.

**Activity Exclusion Criteria** – six filters that narrow the candidate pool before scoring: by date proximity to the target activity (two filters), by duration, by finish date, by date containment, and by activity type. You can also manually exclude or include individual activities (or a whole WBS branch at once) directly in the activity list.

[Full detail – Traceback Settings](pages/traceback-setup.md#step-3--adjust-settings-optional)

---

## Run Traceback

Click **Start Traceback** in the Traceback Setup toolbar to run the analysis. Logic+ traces the delay path backward from the selected activity, comparing the current schedule against the baseline. When complete, the view switches automatically to Delay Analysis mode.

At each step, the traceback engine scores every eligible candidate activity against a fixed set of criteria and picks the strongest one – a two-stage process that weighs a real logical link and date proximity most heavily, then uses supporting factors like WBS position, resourcing, and naming to settle close calls.

[Full detail – How Traceback and Delay Attribution Work](pages/traceback-engine.md)

---

## Delay Overlay

When Delay Analysis mode is active, the Gantt chart highlights the traceback path – the chain of activities identified as driving the delay. Click any highlighted activity to load its candidate and scoring data in the tray below.

For why a given activity was chosen as part of this path, see [How Traceback and Delay Attribution Work](pages/traceback-engine.md).

---

## Candidate Scores

The **Activity Candidates** tab in the Delay Analysis tray shows the full ranked list of candidate activities evaluated at the selected driving task's step. One row per candidate, with a column for each scoring measure and the combined weighted total. The first row shows the maximum weights applied.

[Full detail – Activity Candidates tab](pages/gantt-delay-analysis.md#activity-candidates)

---

## Candidate Details

The **Candidate Scoring** tab shows a simplified view of the ranked candidates for the selected step, with a Go To button to navigate the Gantt chart to any candidate.

[Full detail – Candidate Scoring tab](pages/gantt-delay-analysis.md#candidate-scoring)

---

## Delay Attribute

A per-activity breakdown, shown in a dedicated panel next to the Gantt chart's activity list (separate from the tray below), of how much delay each activity on the traceback path contributed – separating delay that was inherited from earlier in the chain from delay the activity itself introduced, with a flag explaining the reason behind each charge (a duration edit, a relationship change, a constraint change, a calendar difference, an implied/unproven driving link, unexplained real-world progress, or newly added scope).

[Full detail – Delay Attribution panel](pages/gantt-delay-analysis.md#delay-attribution-panel) · [How the charge is calculated](pages/traceback-engine.md#from-chain-to-delay--how-days-get-charged-to-each-activity)

---

## Traceback Log

The **View Traceback Log** button (log icon in the Delay Analysis toolbar) opens the complete log for the current traceback in a separate page. The log shows every driving activity and scoring decision made during the run.

[Full detail – Traceback Log](pages/traceback-log.md)

---

## Traceback Restart

At any step in the traceback path you can change which activity the algorithm selected and restart the analysis from that point. Two options are available:

### From a Shortlisted Candidate

Select an alternative from the ranked candidates list in the tray for the current step and restart from there. Use this when the algorithm's top pick is not the correct driver for that step.

[Full detail](pages/gantt-delay-analysis.md#restart-from-shortlisted-candidate)

### From Any Other Activity

Search for any activity in the schedule by ID, name, or WBS outline number and restart the traceback from that point. Use this to explore alternative delay paths not in the shortlisted candidates.

[Full detail](pages/gantt-delay-analysis.md#restart-from-other-activity)

---

## Save and Load Traceback

Completed tracebacks can be saved by name and reloaded later without re-running the analysis. A default traceback can be set per project – it loads automatically when Delay Analysis mode is entered for that project.

[Full detail – Save and Load](pages/gantt-delay-analysis.md#mode-bar-controls)
