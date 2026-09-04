---
page-id: traceback-log
route: /traceback/log/:tracebackId
title: Traceback Log
audience: external
status: draft
version: 1.1.0
last-reviewed: 2026-09-04
blocked-reason: Verified against current component and handler source, but not yet checked against a live traceback run's actual output – confirm on-screen wording before promoting to complete.
---

## Why this page exists

Traceback picks a "driving task" at each step by scoring candidate predecessor activities against a set of weighted criteria, then excludes some tasks from consideration entirely before scoring even starts. When you need to understand *why* traceback chose one activity over another – or why an activity didn't show up as a candidate at all – the normal Traceback Setup view doesn't show that working. This page does.

It shows the complete detail behind one traceback run: the schedule files used, the scoring weights and exclusion settings applied, every candidate's score at every step, and every task that got filtered out along the way, with the reason why.

## How you get here

From **Delay Analysis mode** inside the Schedule Viewer (Gantt Viewer), once a traceback has completed, a **"View Traceback Log"** button becomes available in the mode bar. Clicking it opens this page in a new browser tab, scoped to that specific traceback run.

## What you will see

**Packets** – the schedule files (packet ID, project, filename, data date) that were part of this traceback run.

**Weights** – the scoring weights used, one row per score type, e.g. `closest-link`, `implied-link`, `finish-variance`, `outline-number`, `sequence-number`. These are the raw internal score-type identifiers (kebab-case), not the friendly labels shown in the Traceback Setup settings modal ("Closest Link", "WBS Structure", and so on) – worth knowing if you're cross-checking a log against what a tester saw in Traceback Setup. For a Calibrated v1 run, an extra `sequence-dictionary` row appears here even though it has no corresponding weight field in the Traceback Setup modal (that factor's weight isn't user-adjustable).

**Settings** – the exclusion criteria applied to this run (exclude newer/older than N days, exclude contained activities, exclude duplicate critical activities, exclude by duration or finish date, exclude LOE/WBS summary tasks). Exclude duplicate critical activities always shows here even though it isn't exposed as a toggle in the Traceback Setup settings modal – it's a fixed, always-on rule.

**Traceback Results** – one block per step along the traceback path. Each shows the driving task chosen at that step, and a table of every candidate considered with its score broken down by weight type, plus a total score as a percentage.

**Project-wide Filtered Tasks** – every task excluded from consideration across the whole run, with the reason it was filtered.

**Filtered Tasks** – the same, but broken down per target/step, so you can see what was filtered out at each specific point in the path.

**View Raw JSON** – the complete underlying data for the run, expandable at the bottom of the page.

## Note

This page opens in its own browser tab, separate from the main app – close the tab or switch back to return to your traceback.
