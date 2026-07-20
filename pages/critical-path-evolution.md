---
page-id: critical-path-evolution
route: TBD — Analytics Dashboard (module ID not yet allocated)
title: Critical Path Evolution
audience: external
status: stub
version: 1.0.0
last-reviewed: 2026-07-20
blocked-reason: Module not yet built (confirmed against current codebase — no matching component exists on main). Business-problem layer documented here; technical detail and UI description to follow once the module is live.
---

## Why this page exists

At any point in a project, one activity (or chain of activities) is actually in the driver's seat — the thing that, if it slips further, directly moves the project finish date. But that driver's seat can change hands between reporting periods, sometimes without anyone explicitly noticing.

Critical Path Evolution shows which activity was driving the project finish date in each reporting period, whether that role passed to a different activity between updates, and how much of the delay shown at each period was newly caused there versus carried forward from an earlier part of the path.

In plain terms: who was in the driver's seat, and when did the wheel change hands.

This uses the same traceback and delay attribution engine as the Gantt Viewer's Delay Analysis mode, surfaced here as a view across all reporting periods rather than a single point-in-time analysis.

## What you will see

*(Detail pending module build and live-app validation. Content will cover: how to read the evolution chart, how to interpret driver changes between periods, and how this relates to the Delay Analysis traceback.)*

## Important

Most delay shown in this view is **forecast** — it represents delay in activities that have not yet started or finished. It has not yet happened in the way that completed-work delay has. The display makes this distinction explicit so that forecast delay is not read with the same weight as delay that is already locked in. This matters when this view is used in a delay-claim or dispute context.
