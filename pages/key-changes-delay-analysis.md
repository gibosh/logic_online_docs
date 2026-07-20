---
page-id: key-changes-delay-analysis
title: Delay Analysis — Key Changes from Desktop
audience: external
status: draft
version: 1.0.0
last-reviewed: 2026-06-27
---

## Delay Analysis Key Changes

Records significant differences between the Logic+ Desktop delay/traceback workflow and Logic+ Online. 

---

### Traceback Setup

**Desktop:** Mode switching is handled by selecting start activity in gantt chart view oe top menu.

**Online:** Traceback Setup is a full mode to handle a streamlined traceback setup. This includes the ability to view excluded activities with the ability to bulk update activity inclusion/exclusion. Future changes will save a default traceback start activity to allow single click traceback start. 

### Algorithm Profile

**Desktop:** Fixed scoring algorithm.

**Online:** A profile selector in the Traceback Setup mode bar:

| Profile | Description |
|---------|-------------|
| Desktop (Legacy) | Original rank-normalised desktop scoring. Default. |
| Replica v2.1 | Ground-truth parity profile using the validated rule set. |
| Calibrated v1 | Replica rule set with calibrated score weights. |

### Algorithm Logic

The Logic+ traceback selection engine and delay attribution layer have been updated to improve how activities are scored and selected when constructing the traceback critical path.

| Rule | Change | Reason |
|---|---|---|
| **T1** | Fixes a bug where only one relationship type (Finish-to-Start) is properly exempted from an exclusion rule. Other valid link types (Start-to-Start, Finish-to-Finish, Start-to-Finish) get wrongly excluded. | This is the direct cause of the +68 wd CIV-610 error — the correct linked predecessor was excluded, forcing the engine onto an unrelated activity. |
| **T2** | Scores the gap between activities in *working days* instead of *calendar days*. | Currently a Friday-finish → Monday-start gets penalised as if it were real slack, when in fact zero working days were lost. |
| **T3** | Introduces a new "gap" classification separate from "artefact." If the engine's chosen link has almost no real supporting evidence, it's flagged as an unattributable gap rather than reported as if it were a genuine scheduling defect. | Early-project activities (procurement, mobilisation) often legitimately have sparse formal logic — that shouldn't be reported the same way as an actual data error. |
| **T4** | Improves name-matching between activities so distinctive words (asset tags, location codes) count for more than generic words like "install" or "works." | Currently a common word like "install" carries the same weight as a unique identifier, which drowns out the useful signal. |
| **T5** | Codifies which programme file "owns" a link when the same relationship appears in more than one loaded file. | Prevents the engine reading a stale link record against current-day dates, which produces misleading scores. |
| **T6** | Fixes the float-scoring formula so very negative float values (e.g. −2,640 hours) can't produce runaway scores. | A genuine calculation bug — scores were meant to stay within a fixed band and don't. |
| **T7** | Every traceback step now writes a full audit record: all candidate scores, winner, runner-up, and a "low confidence" flag when the winner only just beat the runner-up. | Gives planners (and anyone challenging a result) the actual numbers behind every decision, not just the final answer. |
| **T8** | Adds a new scoring factor based on a construction-sequence "dictionary" (e.g. "excavate before pour") built from real project data and reviewed contractor documents. | Gives the engine real construction-process knowledge instead of relying only on dates and names. |
| **T9** | Recalibrates how much weight each scoring factor gets, based on statistical analysis of real data rather than judgement calls. | Demonstration testing shows some factors (like name/structure match) are currently under-weighted and others (like matching calendars) may actually be hurting accuracy. |
| **A1/A2** | Changes how "added scope" delay is charged: one charge for the whole inserted group of activities, instead of splitting it artificially across each activity by duration. Retires a legacy figure (+28 wd) that was a mathematical artefact, not a real number. | The old per-activity split implied a precision the evidence doesn't support and wouldn't survive a real challenge/dispute. |
| **A3** | Sets the order in which flags apply: gap → artefact → real. | Makes sure the new "gap" classification (T3) actually takes effect instead of being overridden. |
| **A4** | Flags when an activity's own calendar gives a materially different delay figure than the shared reference calendar, and reports both. | Improves transparency where calendar choice meaningfully changes the answer. |


### Delay Overlay

**Desktop:** Delay visualisation is a separate panel or view.

**Online:** After a traceback completes, the Gantt chart highlights the traceback path directly on the chart bars. The **Cumulative Delay** tab in the tray shows a chart of cumulative delay along that path over time.

### Traceback Restart

**Desktop:** Restart from an alternate candidate only.

**Online:** Two restart options available directly from the Gantt chart in Delay Analysis mode:

- **Restart from shortlisted candidate** — at any step on the traceback path, click the activity in the Gantt chart and select an alternative candidate from the ranked list in the tray, then restart from that point.
- **Restart from other activity** — search for any activity in the schedule (by ID, name, or WBS outline number) and restart the traceback from that activity.

Both options restart the traceback from the selected point, not from the beginning.

### Re-run Delay Analysis

**Desktop:** Re-running requires re-entering traceback setup.

**Online:** The **Re-run Delay Analysis** button (refresh icon, Delay Analysis mode bar) reloads the current saved traceback result. Available once a traceback is saved. Useful after schedule uploads that affect the delay data.

