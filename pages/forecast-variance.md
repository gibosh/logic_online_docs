---
page-id: forecast-variance
route: TBD — Analytics Dashboard (module ID not yet allocated)
title: Forecast Variance
audience: external
status: stub
version: 1.0.0
last-reviewed: 2026-07-20
blocked-reason: Module not yet built (confirmed against current codebase — no matching component exists on main). Business-problem layer documented here; technical detail and UI description to follow once the module is live.
---

## Forecast Variance — Overview

When a project's finish date moves, there are two very different reasons it could have done so — and they have very different implications for who is accountable and what needs to happen next.

- **Duration growth** — activities are literally taking longer than planned. This is a productivity problem: the work itself is harder, slower, or less resourced than the plan assumed.
- **Logic change** — the sequence of work has been rearranged. This could be genuine re-planning in response to site conditions, a scope change, or something less transparent.

Showing only that the finish date moved does not tell you which of these is responsible, or where in the project it is happening. Forecast Variance separates the two causes and shows, by work area, which parts of the project are reliably tracking to plan, which are being actively reworked, and which are genuinely volatile — changing significantly from update to update for no clear reason.

This moves the conversation from "the schedule slipped" to "here is specifically why, and where."

## Report details

*(Detail pending module build and live-app validation. Content will cover: how to read the variance breakdown, how to interpret duration growth vs logic change by work area, and how to use this alongside Forecast Confidence.)*

## Note

The split between duration growth and logic change only applies to activities that exist in both the schedules being compared. Activities that are new to the comparison schedule — added scope — are reported separately and not attributed to either cause.
