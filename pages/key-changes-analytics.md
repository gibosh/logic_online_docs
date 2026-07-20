---
page-id: key-changes-analytics
title: Analytics — Key Changes from Desktop
audience: external
status: draft
version: 1.0.0
last-reviewed: 2026-07-20
---

## Schedule Analytics Key Changes

Most of the analytics for Logic+ Online are new. The section below outlines an overview of the 9 new analytics. Build status confirmed directly against the `logic_online` codebase (`main` branch) on 2026-07-20 — 6 of the 9 are live and wired to real project data; 3 have no code yet.

| Analytic | Change | Reason | Status |
|---|---|---|---|
| [**1 — Forecast Confidence**](forecast-confidence.md) | Green/amber/red trust signal on the reported finish date, built from the schedule's own history — stability, whether completed work actually backs the plan, remaining slack, how heavily reworked the plan has been, and whether past "recoveries" actually stuck. | PMs have no way to tell if a reported finish date is trustworthy — delay can hide in work that hasn't started yet while everything looks fine on paper. | **Built** — module 1, live data |
| [**2 — Forecast Variance**](forecast-variance.md) | Splits finish-date movement into two causes — activities taking longer (duration growth) vs. the plan's sequencing changing (logic change) — and shows which work areas are on track, being reworked, or genuinely volatile. | "The schedule slipped" isn't actionable. Knowing *why* (productivity vs. re-planning) has very different accountability implications. | Not yet built |
| [**3 — DCMA 14-Point Check**](schedule-quality.md) | Automates the industry-standard 14-point schedule health check (plus CIOB PP21 and Additional Best Practice profiles), refreshed on every schedule update instead of done once by hand. | Manual checks in P6 are slow and get skipped. A client independently catching a quality issue Logic+'s own user missed is a credibility hit in a meeting. | **Built** — module 3, live data |
| [**4 — Critical Path Evolution**](critical-path-evolution.md) | Surfaces the Traceback/Delay Attribution engine as a per-period view: which activity was driving the finish date each period, and when the driving path changed hands. | Gives a systematic, repeatable answer to "who's actually driving delay right now" using the same engine already validated for Delay Analysis. | Not yet built |
| [**5 — Bow-Wave Compression**](bow-wave-compression.md) | Tracks activity volume and concurrency across the timeline and between schedule updates, with panels for comparing updates and tracking float and the critical path alongside it. | The finish date can look protected right up until a late-project crunch is unavoidable. Seeing the volume shape early leaves time to act. | **Built** — module 5, live data |
| [**6 — Causes of Change**](causes-of-change.md) | Detects clusters of activities that shifted later together between two updates and names the most plausible upstream trigger — with mandated "links to N of M" wording, never "caused." | Planners often have a hunch about what triggered a shift but no systematic way to test it, or to separate a real event from routine re-baselining noise. | Not yet built |
| [**7 — Completion Forecast**](completion-forecast.md) | Places the reported finish date next to a data-driven, rules-adjusted projection built from the schedule's own update history. | Reported and data-driven dates can disagree by months. Showing only the reported date hides that gap from PMs. | **Built** — module 7, live data |
| [**8 — Schedule Risk (Tornado / P10/P50/P90)**](schedule-risk.md) | Runs a Monte Carlo schedule-risk simulation against your uploaded schedule and an optional pasted risk register, entirely in-browser, and ranks risk drivers by how much they swing the finish date. | Puts quantitative risk analysis alongside the rest of the reliability picture instead of in a disconnected spreadsheet. | **Built** — module 8, live data |
| [**9 — Float Burn-down + Earned Schedule cross-check**](float-burndown-earned-schedule.md) | Shows two independent finish-date estimates — float consumption vs. earned progress — deliberately as an optimistic bound and a pessimistic bound, not averaged. | Each method alone can mislead (float can be gamed by re-sequencing; earned schedule can look healthy while delay hides in unstarted work). Where they disagree is the real finding. | **Built** — module 9, live data |

**Correction from the previous version of this page:** Forecast Confidence and Float Burn-down + Earned Schedule were previously listed as not yet built — both are live in the current codebase. Schedule Risk was previously described as visualising a Monte Carlo file you upload yourself; the current implementation runs its own Monte Carlo simulation against your schedule and a pasted risk register, not an uploaded pre-computed result — see the corrected [Schedule Risk](schedule-risk.md) page.

