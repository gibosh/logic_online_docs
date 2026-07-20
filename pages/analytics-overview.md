---
page-id: analytics-overview
route: /module/:moduleId/project/:projectId
title: Analytics
audience: external
status: draft
version: 1.0.0
last-reviewed: 2026-07-14
---

## About Analytics

Logic+ Analytics runs a suite of deterministic analyses against your uploaded schedule files. The same schedule always produces the same result – there is no random element. This makes the output defensible: you can explain exactly how each number was calculated.

Analytics are organised in three groups: **schedule quality** (is the schedule well-built enough to trust?), **where we are now** (what is the current state of delay and risk?), and **predictive** (where is the project likely to end up?).

Select a project and schedule from the header to load data into the analytics views.

---

## Schedule Quality

### Schedule Quality – DCMA / CIOB / Best Practice

Runs an automated health check against your schedule and scores it against an industry-standard quality profile. Choose from the DCMA 14-Point Assessment, CIOB PP21 Standard, CIOB PP21 Major, or the Additional Industry Best Practice checks. Results are shown per check, with a pass/fail badge and the list of offending activities for any failed check.

[Open Schedule Quality](pages/schedule-quality.md)

### Forecast Confidence

A plain green, amber, or red trust signal on the reported finish date, built from the schedule's own update history. Answers the question "should I believe this date?" without requiring the reader to interpret raw metrics.

[About Forecast Confidence](pages/forecast-confidence.md)

### Forecast Variance

Separates finish-date movement into two causes: activities taking longer than planned (a productivity problem) and the sequence of work changing (a logic or re-planning event). Shows which cause is dominant, and where in the project it is happening.

[About Forecast Variance](pages/forecast-variance.md) *(module coming soon)*

---

## Schedule Insights

### Critical Path Evolution

Shows which activity was driving the project finish date in each reporting period, when that role changed hands, and how much delay was newly introduced at each step versus carried forward from earlier in the path. Uses the same engine as the Delay Analysis traceback.

[About Critical Path Evolution](pages/critical-path-evolution.md) *(module coming soon)*

### Bow Wave Compression

Shows planned work volume distributed across the project timeline and how that distribution has changed between schedule updates. Includes Peak Concurrent Tasks, Project Progress, and Average Float sub-views, plus panels that identify the single largest re-sequencing event in the update history.

[Open Bow Wave Compression](pages/bow-wave-compression.md)

### Causes of Change

Identifies the group of activities that moved later together between two schedule updates, names the most plausible upstream trigger, and compares the size of the movement against the schedule's own background variation – to distinguish a real event from routine re-baselining noise.

[About Causes of Change](pages/causes-of-change.md) *(module coming soon)*

---

## Schedule Predictions

### Completion Forecast

Places the reported finish date alongside a data-driven projection based on the schedule's own track record, so the gap between "claimed" and "likely" is visible. The projection uses the schedule's update history, not a simulation – it produces the same answer for the same inputs every time.

[About Completion Forecast](pages/completion-forecast.md)

### Schedule Risk Analysis (SRA)

Runs a Monte Carlo schedule-risk simulation against your uploaded schedule and an optional pasted risk register, entirely in-browser, and ranks risk drivers by how much they swing the finish date.

[About Schedule Risk Analysis (SRA)](pages/schedule-risk.md)

### Float Burn-down and Earned Schedule

Shows two independent estimates of the real finish date – one based on how fast schedule buffer (float) is being consumed, one based on how much work has been completed against plan. Shown deliberately as an optimistic and a pessimistic bound. When the two methods disagree, that gap is the finding.

[About Float Burn-down and Earned Schedule](pages/float-burndown-earned-schedule.md)
