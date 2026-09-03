---
page-id: key-changes-analytics
title: Analytics – Key Changes from Desktop
audience: external
status: draft
version: 1.1.0
last-reviewed: 2026-09-03
---

## Schedule Analytics Key Changes

Most of the analytics for Logic+ Online are new. The section below outlines an overview of the 9 new analytics. 

| Analytic | Change | Reason | Status |
|---|---|---|---|
| [**1 – Forecast Confidence**](pages/forecast-confidence.md) | Green/amber/red trust signal on the reported finish date, built from the schedule's own history – stability, whether completed work actually backs the plan, remaining slack, how heavily reworked the plan has been, and whether past "recoveries" actually stuck. | PMs have no way to tell if a reported finish date is trustworthy – delay can hide in work that hasn't started yet while everything looks fine on paper. | **Built** – module 1, live data |
| [**2 – Forecast Variance**](pages/forecast-variance.md) | Splits finish-date movement into two causes – activities taking longer (duration growth) vs. an activity's start date itself moving (timing shift) – and shows which work areas are on track, being reworked, or genuinely volatile. | "The schedule slipped" isn't actionable. Knowing *why* (productivity vs. re-sequencing) has very different accountability implications. | **Built** – module 2, live data |
| [**3 – DCMA 14-Point Check**](pages/schedule-quality.md) | Automates the industry-standard 14-point schedule health check (plus CIOB PP21 and Additional Best Practice profiles), refreshed from the latest upload, with a full assessment-scope drill-down to audit exactly what fed each check. | Manual checks in P6 are slow and get skipped. A client independently catching a quality issue Logic+'s own user missed is a credibility hit in a meeting. | **Built** – module 3, live data |
| [**4 – Critical Path Evolution**](pages/critical-path-evolution.md) | A per-period view of which activity was driving the finish date, and when the driving path changed hands. Shares its delay-charging maths with Delay Analysis, but builds its own driving-path history rather than running a full traceback. | Gives a systematic, repeatable answer to "who's actually driving delay right now" without needing to set up and run a traceback. | **Built** – module 4, live data |
| [**5 – Bow-Wave Compression**](pages/bow-wave-compression.md) | Tracks activity volume and concurrency across the timeline and between schedule updates, via a Schedule Summary view and a Schedule Forensics view with a Criticality filter. | The finish date can look protected right up until a late-project crunch is unavoidable. Seeing the volume shape early leaves time to act. | **Built** – module 5, live data |
| [**6 – Causes of Change**](pages/causes-of-change.md) | Detects clusters of activities that shifted later together between two updates and classifies the likely root cause – identified, ambiguous, or a logic cycle – with supporting constraint evidence where found. | Planners often have a hunch about what triggered a shift but no systematic way to test it, or to separate a real event from routine re-baselining noise. | **Built** – module 6, live data |
| [**7 – Completion Forecast**](pages/completion-forecast.md) | Places the reported finish date next to a data-driven, rules-adjusted projection built from the schedule's own update history. | Reported and data-driven dates can disagree by months. Showing only the reported date hides that gap from PMs. | **Built** – module 7, live data |
| [**8 – Schedule Risk Analysis (SRA)**](pages/schedule-risk.md) | Runs a Monte Carlo schedule-risk simulation against your uploaded schedule and an optional pasted risk register, entirely in-browser, and ranks risk drivers by how much they swing the finish date. | Puts quantitative risk analysis alongside the rest of the reliability picture instead of in a disconnected spreadsheet. | **Built** – module 8, live data |
| [**9 – Float Burn-down + Earned Schedule cross-check**](pages/float-burndown-earned-schedule.md) | Shows two independent finish-date estimates – float consumption vs. earned progress – deliberately as an optimistic bound and a pessimistic bound, not averaged. | Each method alone can mislead (float can be gamed by re-sequencing; earned schedule can look healthy while delay hides in unstarted work). Where they disagree is the real finding. | **Built** – module 9, live data |

