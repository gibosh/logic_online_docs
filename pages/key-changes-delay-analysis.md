---
page-id: key-changes-delay-analysis
title: Delay Analysis – Key Changes from Desktop
audience: external
status: draft
version: 1.0.0
last-reviewed: 2026-07-21
---

## Delay Analysis Key Changes

Records significant differences between the Logic+ Desktop delay/traceback workflow and Logic+ Online.

---

### Traceback Setup

**Desktop:** Mode switching is handled by selecting a start activity in the Gantt chart view or the top menu.

**Online:** Traceback Setup is a full mode for a streamlined setup, including the ability to view excluded activities and bulk-update activity inclusion/exclusion. A future update will let you save a default traceback start activity, for single-click traceback start.

### Algorithm Profile

**Desktop:** One fixed scoring algorithm.

**Online:** A profile selector in the Traceback Setup mode bar lets you choose which scoring rule set to use:

| Profile | Description |
|---------|-------------|
| Desktop (Legacy) | Reproduces Desktop's original scoring exactly, bugs included — useful for like-for-like comparison while migrating. **Default.** |
| Replica v2.1 | The validated, corrected rule set — see [Algorithm Logic](#algorithm-logic) below for what changes. |
| Calibrated v1 | The Replica v2.1 rule set, with scoring weights statistically recalibrated against real project data. |

### Algorithm Logic

The traceback engine and delay attribution have both been reviewed and improved for Online. A couple of things are worth knowing before reading the table below:

- **Most of these changes only take effect if you select Replica v2.1 or Calibrated v1** in the profile selector above. If you leave the profile on its default (Desktop Legacy), traceback candidate selection behaves exactly as it does in Desktop — that's deliberate, so you have a stable baseline to compare against while migrating.
- **Delay attribution — how a chain gets turned into a day-by-day charge — isn't affected by that profile selector.** Its improvements apply no matter which profile you've selected for traceback selection, because the two are separate stages: the profile only changes how a candidate is picked, not how delay is subsequently charged.

#### More accurate candidate selection

| Change | What it does | Why it matters | Where it applies |
|---|---|---|---|
| **Calendar-aware date-proximity scoring** | When there's no formal link between two activities, the fallback date-proximity score now measures the gap between them in working days on the correct calendar, not raw calendar days. | A completely normal Friday-finish-to-Monday-start transition was previously scored as if real time had been lost, when in fact zero working days had passed. | All profiles |
| **Contained-activity exclusion, fixed** | Previously, an activity that falls entirely inside another activity's dates could still be genuinely linked to it — but only a Finish-to-Start link was correctly recognised as an exception to the exclusion rule. The other three link types (Start-to-Start, Finish-to-Finish, Start-to-Finish) were wrongly excluded even when a real link existed. Now all four link types are recognised correctly. | In one real case, this caused a several-week distortion in the reported delay chain — the correct linked predecessor was excluded, forcing the engine onto an unrelated activity instead. | Replica v2.1 / Calibrated v1 |
| **Distinctive-word name matching** | Comparing activity names now gives more weight to distinctive words (asset tags, location codes) and less to generic ones like "install" or "works." | Previously, a common word carried the same weight as a unique identifier, which drowned out the useful signal when comparing two activity names. | Replica v2.1 / Calibrated v1 |
| **Cross-file link ownership** | When the same relationship appears in more than one loaded schedule file, Logic+ now has clear rules for which file's version of that link is used. | Prevents the engine reading a stale link record against current-day dates, which could previously produce misleading scores. | Replica v2.1 / Calibrated v1 |
| **Float-score formula fix** | Fixed the float-scoring formula so very large negative float values can no longer produce runaway scores outside the intended range. | A genuine calculation bug — scores were meant to stay within a fixed range, and previously didn't always. | Replica v2.1 / Calibrated v1 |

#### Clearer confidence and transparency

| Change | What it does | Why it matters | Where it applies |
|---|---|---|---|
| **"Gap" vs. genuine defect classification** | When the engine's best available choice at a step has almost no real supporting evidence — no strong link, no close dates — that step is now classified as an unattributable **Gap** rather than reported the same way as an actual scheduling defect. This check happens first, so it can't be silently overridden by another flag. | Early-project activities like procurement and mobilisation often legitimately have sparse formal logic. That's a normal, expected pattern — it shouldn't be reported the same way as a genuine data error elsewhere in the schedule. | Replica v2.1 / Calibrated v1 |
| **Full scoring audit trail** | Every traceback step now writes a complete record: every candidate's score, the winner, the runner-up, and a "low confidence" flag when the winner only just beat the runner-up. | Gives you — or anyone reviewing or challenging a result — the actual evidence behind every decision, not just the final answer. | Replica v2.1 / Calibrated v1 |
| **Calendar-sensitivity flag** | Flags when an activity's own calendar produces a noticeably different delay figure than the project's shared reference calendar, and reports both figures. | Improves transparency in cases where calendar choice meaningfully changes the answer. | All profiles — this is part of delay attribution, not candidate selection |

#### New scoring capability (Calibrated v1 only)

| Change | What it does | Why it matters |
|---|---|---|
| **Construction-sequence dictionary** | Adds a new scoring factor based on a construction-sequence "dictionary" (e.g. "excavate before pour"), built from real project data and reviewed contractor documents. | Gives the engine real construction-process knowledge to draw on, instead of relying only on dates and names. |
| **Statistically recalibrated weights** | Recalibrates how much weight each scoring factor gets, based on statistical analysis of real project data rather than judgement calls. | Testing shows some factors (like name/structure match) were under-weighted, and others (like matching calendars) may actually have been hurting accuracy. |

#### Delay attribution (all profiles)

| Change | What it does | Why it matters |
|---|---|---|
| **Grouped charging for added scope** | When new activities are added to a schedule with no equivalent in the baseline, the delay they represent is now charged once, to the activity that introduced them — instead of being split artificially across each new activity by duration. | The old per-activity split implied a level of precision the evidence doesn't support, and wouldn't hold up under a real challenge or dispute. |

---

### Delay Overlay

**Desktop:** Delay visualisation is a separate panel or view.

**Online:** After a traceback completes, the Gantt chart highlights the traceback path directly on the chart bars. The **Cumulative Delay** tab in the tray shows a chart of cumulative delay along that path over time.

### Traceback Restart

**Desktop:** Restart from an alternate candidate only.

**Online:** Two restart options are available directly from the Gantt chart in Delay Analysis mode:

- **Restart from shortlisted candidate** – at any step on the traceback path, click the activity in the Gantt chart and select an alternative candidate from the ranked list in the tray, then restart from that point.
- **Restart from other activity** – search for any activity in the schedule (by ID, name, or WBS outline number) and restart the traceback from that activity.

Both options restart the traceback from the selected point, not from the beginning.

### Re-run Delay Analysis

**Desktop:** Re-running requires re-entering traceback setup.

**Online:** The **Re-run Delay Analysis** button (refresh icon, Delay Analysis mode bar) reloads the current saved traceback result. Available once a traceback is saved. Useful after schedule uploads that affect the delay data.
