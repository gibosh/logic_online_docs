---
page-id: traceback-log
route: /traceback/log/:tracebackId
title: Traceback Log
audience: internal
status: draft
version: 1.0.0
last-reviewed: 2026-07-20
blocked-reason: Written from component source, not yet checked against a live traceback run. Internal audience — this is a raw diagnostic view, not written for end users. Do not publish externally without a decision to change that.
---

## Why this page exists

Traceback picks a "driving task" at each step by scoring candidate predecessor activities against a set of weighted criteria, then excludes some tasks from consideration entirely before scoring even starts. When a tester or support person needs to understand *why* traceback chose one activity over another — or why an activity didn't show up as a candidate at all — the normal Traceback Setup view doesn't show that working. This page does.

It is a raw, unstyled dump of everything that went into one traceback run: the schedule packets used, the scoring weights, the exclusion settings, every candidate's score at every step, and every task that got filtered out along the way (with the reason why).

## How you get here

From **Traceback Setup mode** inside the Schedule Viewer (Gantt Viewer), once a traceback has been run, a **"View Traceback Log"** button becomes available in the toolbar. Clicking it opens this page in a new browser tab, scoped to that specific traceback run (`tracebackId` in the URL).

## What you will see

**Packets** — the schedule files (packet ID, project, filename, data date) that were part of this traceback run.

**Weights** — the scoring weights used, by score type (e.g. Defined Link, ClosestLink, ImpliedLink — matches the weighting set up in Traceback Setup).

**Settings** — the exclusion criteria applied to this run (exclude newer/older than N days, exclude contained activities, exclude duplicate critical activities, exclude by duration or finish date, exclude LOE/WBS summary tasks).

**Traceback Results** — one block per step along the traceback path. Each shows the driving task chosen at that step, and a table of every candidate considered with its score broken down by weight type, plus a total score as a percentage.

**Project-wide Filtered Tasks** — every task excluded from consideration across the whole run, with the reason it was filtered.

**Filtered Tasks** — the same, but broken down per target/step, so you can see what was filtered out at each specific point in the path.

**View Raw JSON** — the complete underlying data for the run, expandable at the bottom of the page.

## Notes for whoever validates this against the live app

- Confirm the weight names shown match current Traceback Setup labels (ClosestLink / ImpliedLink / Defined Link, or whatever the current scoring set is called).
- This page has no navigation back into the app — it's a standalone diagnostic tab. Worth confirming that's intentional before beta testers start using it, since a support person might click through to it without an easy way back.
- Distinct from `pages/traceback.md` (the older Desktop-derived candidate-scoring reference) and `pages/traceback-setup.md` (the external, business-facing Traceback Setup mode doc) — this page is the "show your working" raw log, not a replacement for either.
