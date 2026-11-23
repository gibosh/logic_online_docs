---
page-id: delay-analysis
route: /delay-analysis
title: Delay Analysis
audience: external
status: complete
version: 1.4.0
last-reviewed: 2026-11-23
blocked-reason: 2026-11-23 – LUSB-1392: No data date notice for undated schedules; added the Delay Attribution Scenarios link; removed 'more methods may be added' (style guide: no coming-soon). Humanizer rewrite of the prose. Consolidated 2026-09-24 – this page previously covered "Delay Analysis" but its body was almost entirely Traceback-specific content duplicated from traceback-setup.md/gantt-delay-analysis.md/traceback-engine.md. Trimmed to a thin, method-neutral hub (matching how pages/analytics-overview.md links out to module pages rather than embedding them) with a single Traceback method section. "Delay Attribute" flag list correction from 2026-09-23 carried forward (now lives on traceback-engine.md, not duplicated here).
---

Delay Analysis is the module for analysing schedule delay. Open it from **Delay Analysis** in the left-hand navigation, then choose a method. Each method has its own steps: pick it, work through its setup, then review its results.

| Method | Use it to |
|---|---|
| **[Traceback](#traceback-method)** | Trace the chain of driving activities backward from a selected endpoint, attribute delay to each link in that chain, and see how the delay picture has built up over time |
| **[Time-Impact Analysis](pages/time-impact-analysis.md)** | Build a hypothetical delay event, insert it into the schedule, and see its calculated impact on Practical Completion |

The two methods have separate setup steps and separate results screens.

Both methods need schedules with a data date. If you select a schedule without one, a **No data date** notice replaces the analysis until you set the date in [Schedule Manager](pages/schedule-manager.md#schedules-without-a-data-date).

## Traceback method

Traceback identifies which activities in a project schedule pushed the completion date later, and by how much. Selecting it opens the [Schedule Viewer](pages/gantt-viewer.md)'s **Traceback Setup** tab, where you select a start activity, choose an algorithm profile, and optionally adjust the candidate scoring weights and activity exclusion criteria. Running the analysis switches to the **Delay Analysis** tab, which shows the traceback path over the Gantt chart, ranked candidate scoring, a breakdown of the delay attributed to each activity, and options to restart, save or load a traceback.

- [Traceback Setup](pages/traceback-setup.md): start activity, algorithm profile, scoring weights, exclusion criteria
- [Delay Analysis results](pages/gantt-delay-analysis.md): traceback path, candidate scoring, delay attribution, restart, save and load
- [Delay Attribution Scenarios](pages/delay-attribution-2.md): every scenario Logic+ uses to decide what caused the delay, with diagrams and worked examples
- [How the Traceback method works](pages/traceback-engine.md): the scoring logic behind each step, and how delay is attributed to each activity
- [Traceback Log](pages/traceback-log.md): the full diagnostic log for a completed traceback
