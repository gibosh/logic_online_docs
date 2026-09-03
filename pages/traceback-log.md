---
page-id: traceback-log
route: /traceback/log/:tracebackId
title: Traceback Log
audience: internal
status: draft
version: 1.0.1
last-reviewed: 2026-09-03
blocked-reason: Verified against current component and handler source (2026-09-03), but still not checked against a live traceback run's actual output. Internal audience – this is a raw diagnostic view, not written for end users. Do not publish externally without a decision to change that. See also the access-control note below – the page is currently reachable by any authenticated user, not just internal staff.
---

## Why this page exists

Traceback picks a "driving task" at each step by scoring candidate predecessor activities against a set of weighted criteria, then excludes some tasks from consideration entirely before scoring even starts. When a tester or support person needs to understand *why* traceback chose one activity over another – or why an activity didn't show up as a candidate at all – the normal Traceback Setup view doesn't show that working. This page does.

It is a raw, unstyled dump of everything that went into one traceback run: the schedule packets used, the scoring weights, the exclusion settings, every candidate's score at every step, and every task that got filtered out along the way (with the reason why).

## How you get here

From **Delay Analysis mode** inside the Schedule Viewer (Gantt Viewer), once a traceback has completed, a **"View Traceback Log"** button becomes available in the mode bar. Clicking it opens this page in a new browser tab, scoped to that specific traceback run (`tracebackId` in the URL). (Corrected 2026-09-03 – this was previously misdescribed as living in Traceback Setup mode; the button is in Delay Analysis mode, and is disabled until a traceback has been run.)

## What you will see

**Packets** – the schedule files (packet ID, project, filename, data date) that were part of this traceback run.

**Weights** – the scoring weights used, one row per score type, e.g. `closest-link`, `implied-link`, `finish-variance`, `outline-number`, `sequence-number`. These are the raw internal score-type identifiers (kebab-case), not the friendly labels shown in the Traceback Setup settings modal ("Closest Link", "WBS Structure", and so on) – worth knowing if you're cross-checking a log against what a tester saw in Traceback Setup. For a Calibrated v1 run, an extra `sequence-dictionary` row appears here even though it has no corresponding weight field in the Traceback Setup modal (that factor's weight isn't user-adjustable).

**Settings** – the exclusion criteria applied to this run (exclude newer/older than N days, exclude contained activities, exclude duplicate critical activities, exclude by duration or finish date, exclude LOE/WBS summary tasks). Exclude duplicate critical activities always shows here even though it isn't exposed as a toggle in the Traceback Setup settings modal – it's a fixed, always-on rule.

**Traceback Results** – one block per step along the traceback path. Each shows the driving task chosen at that step, and a table of every candidate considered with its score broken down by weight type, plus a total score as a percentage.

**Project-wide Filtered Tasks** – every task excluded from consideration across the whole run, with the reason it was filtered.

**Filtered Tasks** – the same, but broken down per target/step, so you can see what was filtered out at each specific point in the path.

**View Raw JSON** – the complete underlying data for the run, expandable at the bottom of the page.

## Notes for whoever validates this against the live app

- **Resolved (2026-09-03):** the weight names shown do *not* match the Traceback Setup labels – see the note under Weights above. Confirmed from `TracebackService.buildLog` (`backend/services/TracebackService.ts`), which passes the raw `ScoreType` strings straight through as the row name.
- **Access control – needs a product decision.** This route (`/traceback/log/:tracebackId`) is registered behind the app's standard `GuardedRoute` (any authenticated user), not the separate `AdminGuardedRoute` used elsewhere in the app for internal-only pages. The "View Traceback Log" button in the Delay Analysis mode bar is likewise available to any user who can reach Delay Analysis mode – there's no role check hiding it from external beta testers. So despite this page being written and reviewed as an internal diagnostic view, any external beta tester who completes a traceback can currently open it today. Someone needs to decide whether that's fine as-is, or whether the route/button should be gated to internal roles.
- This page has no navigation back into the app – it's a standalone diagnostic tab. Given the point above, that's more likely to matter for external testers than originally assumed.
- Still not checked against a live traceback run's actual JSON output – the interface above is read from `TracebackLogView.tsx`'s prop types and `backend/api/traceback/log/[tracebackId]/handler.ts`'s response shape, not from watching a real run.
- The docs folder has no `pages/traceback.md` file (checked 2026-09-03) – the earlier note describing one as "the older Desktop-derived candidate-scoring reference" no longer applies; if that reference material exists it isn't in this docs site under that name. This page is distinct from `pages/traceback-setup.md` (the external, business-facing Traceback Setup mode doc) – it's the "show your working" raw log, not a replacement for it.
