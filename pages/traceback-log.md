---
page-id: traceback-log
route: /traceback/log/:tracebackId
title: Traceback Log
audience: external
status: draft
version: 1.2.2
last-reviewed: 2026-09-25
blocked-reason: Fixed stale "Delay Analysis mode"/"mode bar"/"Gantt Viewer" language caught in the 2026-09-25 sitewide drift audit – this page was reviewed earlier the same day but the language slipped through that pass. Verified against current component and handler source, but not yet checked against a live traceback run's actual output – confirm on-screen wording before promoting to complete. New "Filter audit step" dropdown and "Error loading traceback log" state (both from the 2026-09-21 traceback-document versioning change, LUSB-1269) added from source. Separately worth flagging to engineering (not a docs issue): opening an old saved traceback from the Gantt page's own "Load Traceback" dropdown (as opposed to this Log page) currently fails silently with no on-screen message at all — see workspace/GAPS.md.
---

At each step, traceback picks a "driving task" by scoring candidate predecessor activities against a set of weighted criteria, after excluding some tasks before scoring starts. The Traceback Setup view doesn't show that working, so when you need to know *why* traceback chose one activity over another, or why an activity never appeared as a candidate, use the Traceback Log.

The log shows the full detail behind one traceback run: the schedule files used, the scoring weights and exclusion settings applied, every candidate's score at every step, and every task filtered out along the way, with the reason.

## How you get here

In the **Delay Analysis** tab of the Traceback method, once a traceback has finished, a **"View Traceback Log"** button becomes available. It opens the log for that traceback run in a new browser tab.

## What you will see

**Packets**: the schedule files in this traceback run (packet ID, project, filename, data date).

**Weights**: the scoring weights used, one row per score type, for example `closest-link`, `implied-link`, `finish-variance`, `outline-number` and `sequence-number`. These are the internal score-type identifiers, not the labels in the Traceback Setup settings modal ("Closest Link", "WBS Structure" and so on), which helps when you compare a log with what someone saw in Traceback Setup. A Calibrated run also has a `sequence-dictionary` row, although that factor has no weight field in the Traceback Setup modal because its weight can't be changed.

**Settings**: the exclusion criteria applied to the run (exclude newer or older than N days, exclude contained activities, exclude duplicate critical activities, exclude by duration or finish date, and exclude LOE and WBS summary tasks). Exclude duplicate critical activities always appears here, although it isn't a toggle in the Traceback Setup modal, because it's a fixed rule that's always on.

**Traceback Results**: one block per step along the traceback path. Each shows the driving task chosen at that step, and a table of every candidate considered, with its score broken down by weight type and a total score as a percentage.

**Project-wide Filtered Tasks**: every task excluded from consideration across the whole run, with the reason.

**Filtered Tasks**: the same, broken down by target or step, so you can see what was filtered out at each point in the path. If the traceback was restarted, a **"Filter audit step"** dropdown above this section lets you choose which restart step's list to view.

**View Raw JSON**: the complete underlying data for the run, which you can expand at the bottom of the page.

## Note

The log opens in its own browser tab, separate from the main app. Close the tab or switch back to return to your traceback.

**If you see "Error loading traceback log"** instead, the saved traceback you're opening predates a change on 2026-09-21 to how traceback results are stored, and older saved results can't be read. Run the traceback again to get a result that loads.
