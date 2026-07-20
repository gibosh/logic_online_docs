---
page-id: delay-analysis
route: /schedule-viewer/gantt
title: Delay Analysis
audience: external
status: draft
version: 1.1.0
last-reviewed: 2026-07-14
---

## About Delay Analysis

Delay Analysis identifies which activities in a project schedule are responsible for pushing the completion date later, and by how much. It traces the chain of driving activities backward from a selected endpoint, attributes delay to each link in that chain, and shows how the delay picture has built up over time.

The Delay Analysis workflow lives inside the [Schedule Viewer](pages/gantt-viewer.md). There are two stages — setting up and running the traceback in **Traceback Setup** mode, then reviewing the results in **Delay Analysis** mode.

---

## Traceback Setup

Before running a delay analysis, select a start activity (the endpoint you are tracing from), choose an algorithm profile, and optionally adjust the candidate scoring weights and activity exclusion criteria.

[Full detail — Traceback Setup mode](pages/traceback-setup.md)

### Traceback Settings

The settings modal (gear icon in the Traceback Setup toolbar) has two sections:

**Scoring** — adjust how much influence each measure has on which activity is selected as the next driver at each step. Primary scores (Closest Link, Implied Link) drive most of the decision. Thirteen secondary scores refine the ranking when primary scores are close.

**Excluded activities** — five filters that narrow the candidate pool before scoring: by date proximity to the target activity, by duration, by finish date, by date containment, and by activity type.

[Full detail — Traceback Settings](pages/traceback-setup.md#step-3--adjust-settings-optional)

---

## Run Traceback

Click **Start Traceback** in the Traceback Setup toolbar to run the analysis. Logic+ traces the delay path backward from the selected activity, comparing the current schedule against the baseline. When complete, the view switches automatically to Delay Analysis mode.

---

## Delay Overlay

When Delay Analysis mode is active, the Gantt chart highlights the traceback path — the chain of activities identified as driving the delay. Click any highlighted activity to load its candidate and scoring data in the tray below.

---

## Candidate Scores

The **Activity Candidates** tab in the Delay Analysis tray shows the full ranked list of candidate activities evaluated at the selected driving task's step. One row per candidate, with a column for each scoring measure and the combined weighted total. The first row shows the maximum weights applied.

[Full detail — Activity Candidates tab](pages/gantt-delay-analysis.md#activity-candidates)

---

## Candidate Details

The **Candidate Scoring** tab shows a simplified view of the ranked candidates for the selected step, with a Go To button to navigate the Gantt chart to any candidate.

[Full detail — Candidate Scoring tab](pages/gantt-delay-analysis.md#candidate-scoring)

---

## Delay Attribute

A summary of how much delay each activity on the traceback path contributed — separating delay that was inherited from earlier in the chain from delay the activity itself introduced.

*Full detail coming soon.*

---

## Traceback Log

The **View Traceback Log** button (log icon in the Delay Analysis toolbar) opens the complete log for the current traceback in a separate page. The log shows every driving activity and scoring decision made during the run.

[Full detail — Traceback Log](pages/gantt-delay-analysis.md#mode-bar-controls)

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

## Traceback Engine

The traceback engine applies a set of correction rules to ensure the traced path reflects what actually happened on the project, not what data-quality gaps or calendar mismatches imply. The engine corrects three known causes of incorrect attribution: calendar mismatches between compared schedules, broken or missing links in the activity network, and a relationship-type recording issue in the underlying data.

*This feature is pending deployment. Detail will be added once the engine update is live.*

---

## Save and Load Traceback

Completed tracebacks can be saved by name and reloaded later without re-running the analysis. A default traceback can be set per project — it loads automatically when Delay Analysis mode is entered for that project.

[Full detail — Save and Load](pages/gantt-delay-analysis.md#mode-bar-controls)
