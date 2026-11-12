---
page-id: delay-analysis
route: /delay-analysis
title: Delay Analysis
audience: external
status: complete
version: 1.3.0
last-reviewed: 2026-09-24
blocked-reason: Consolidated 2026-09-24 – this page previously covered "Delay Analysis" but its body was almost entirely Traceback-specific content duplicated from traceback-setup.md/gantt-delay-analysis.md/traceback-engine.md. Trimmed to a thin, method-neutral hub (matching how pages/analytics-overview.md links out to module pages rather than embedding them) with a single Traceback method section. "Delay Attribute" flag list correction from 2026-09-23 carried forward (now lives on traceback-engine.md, not duplicated here).
---

## About Delay Analysis

Delay Analysis is the module for analysing schedule delay – open it from **Delay Analysis** in the left-hand navigation, then choose which method to use. Each method is its own journey: pick it, work through that method's own setup, then review its results.

| Method | Use it to |
|---|---|
| **[Traceback](#traceback-method)** | Trace the chain of driving activities backward from a selected endpoint, attribute delay to each link in that chain, and see how the delay picture has built up over time |
| **[Time-Impact Analysis](pages/time-impact-analysis.md)** | Build a hypothetical delay event, insert it into the schedule, and see its calculated impact on Practical Completion |

More methods may be added here over time – Traceback and Time-Impact Analysis don't share a setup flow or a results screen with each other, and a future method wouldn't be expected to either.

---

## Traceback method

Traceback identifies which activities in a project schedule are responsible for pushing the completion date later, and by how much. Selecting it opens the [Schedule Viewer](pages/gantt-viewer.md)'s **Traceback Setup** tab, where you select a start activity, choose an algorithm profile, and optionally adjust candidate scoring weights and activity exclusion criteria. Running the analysis switches automatically to the **Delay Analysis** tab, which shows the traceback path over the Gantt chart, ranked candidate scoring, a per-activity delay attribution breakdown, and options to restart, save, or load a traceback.

- [Traceback Setup](pages/traceback-setup.md) – start activity, algorithm profile, scoring weights, exclusion criteria
- [Delay Analysis results](pages/gantt-delay-analysis.md) – traceback path, candidate scoring, delay attribution, restart, save/load
- [How the Traceback method works](pages/traceback-engine.md) – the scoring logic behind each step, and how delay gets attributed to each activity
- [Traceback Log](pages/traceback-log.md) – the full diagnostic log for a completed traceback
