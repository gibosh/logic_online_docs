---
page-id: analytics-overview
route: /module/:moduleId/project/:projectId
title: Analytics
audience: external
status: draft
version: 1.4.0
last-reviewed: 2026-11-23
blocked-reason: 2026-11-23 – LUSB-1392: analytics leave out undated schedules and show a No data date notice when none has one (ModulePage.tsx, ProjectPreparation.ts). Corrected 'no random element' - SRA is a random simulation. Humanizer rewrite of the prose. Fixed internal hyphenation drift 2026-09-23 – "Bow Wave Compression" and "Float Burn-down and Earned Schedule" corrected to match the canonical hyphenated names used everywhere else on the site.
---

Logic+ Analytics runs a fixed set of calculations on your uploaded schedule files. Apart from Schedule Risk Analysis, which is a random simulation, there's no random element, so the same schedules always produce the same result and you can explain exactly how each number was calculated.

The analytics are in three groups: **Schedule Quality** (is the schedule built well enough to trust?), **Schedule Insights** (what is the current state of delay and risk?) and **Schedule Predictions** (where is the project likely to end up?).

Analytics need schedules with a data date. Logic+ leaves out any schedule without one, and if none of the project's schedules has a data date, the analytics show a **No data date** notice. See [Schedules without a data date](pages/schedule-manager.md#schedules-without-a-data-date).

<div class="grid-group-label">Schedule Quality</div>
<div class="module-grid cols-3">
  <a class="module-tile" href="#/pages/forecast-confidence">
    <div class="tile-label">01</div>
    <h3>Forecast Confidence</h3>
    <p>A plain green, amber or red trust signal on the reported finish date.</p>
  </a>
  <a class="module-tile" href="#/pages/forecast-variance">
    <div class="tile-label">02</div>
    <h3>Forecast Variance</h3>
    <p>Splits finish-date movement into duration growth and timing shift, by work area.</p>
  </a>
  <a class="module-tile" href="#/pages/schedule-quality">
    <div class="tile-label">03</div>
    <h3>Schedule Quality</h3>
    <p>Automated health check against DCMA, CIOB or Additional Best Practice standards.</p>
  </a>
</div>

<div class="grid-group-label">Schedule Insights</div>
<div class="module-grid cols-3">
  <a class="module-tile" href="#/pages/critical-path-evolution">
    <div class="tile-label">04</div>
    <h3>Critical Path Evolution</h3>
    <p>Which activity drove the finish date in each period, and when that changed.</p>
  </a>
  <a class="module-tile" href="#/pages/bow-wave-compression">
    <div class="tile-label">05</div>
    <h3>Bow-Wave Compression</h3>
    <p>Work volume across the timeline, and how it has shifted between schedule updates.</p>
  </a>
  <a class="module-tile" href="#/pages/causes-of-change">
    <div class="tile-label">06</div>
    <h3>Causes of Change</h3>
    <p>Groups of activities that moved together, with a candidate root cause.</p>
  </a>
</div>

<div class="grid-group-label">Schedule Predictions</div>
<div class="module-grid cols-3">
  <a class="module-tile" href="#/pages/completion-forecast">
    <div class="tile-label">07</div>
    <h3>Completion Forecast</h3>
    <p>The reported finish date alongside a rules-based projection from the schedule's history.</p>
  </a>
  <a class="module-tile" href="#/pages/schedule-risk">
    <div class="tile-label">08</div>
    <h3>Schedule Risk Analysis (SRA)</h3>
    <p>A Monte Carlo schedule-risk simulation that runs in your browser.</p>
  </a>
  <a class="module-tile" href="#/pages/float-burndown-earned-schedule">
    <div class="tile-label">09</div>
    <h3>Float Burn-down</h3>
    <p>Two independent finish-date estimates: an optimistic and a pessimistic bound.</p>
  </a>
</div>

## Schedule Quality

### Forecast Confidence

A plain green, amber or red trust signal on the reported finish date, built from the schedule's own update history. It answers "should I believe this date?" without the reader having to interpret raw metrics.

[About Forecast Confidence](pages/forecast-confidence.md)

### Forecast Variance

Splits finish-date movement into two causes: activities taking longer than planned (a productivity problem), and activities starting on a different date than planned (re-sequencing, a knock-on effect or a manual override). It shows which cause dominates and where in the project it's happening.

[About Forecast Variance](pages/forecast-variance.md)

### Schedule Quality: DCMA, CIOB and best practice

Runs an automated health check on your schedule and scores it against an industry quality profile: the DCMA 14-Point Assessment, CIOB PP21 Standard, CIOB PP21 Major, or the Additional Industry Best Practice checks. Results are shown per check, with a pass or fail badge and, for each failed check, the list of activities that fail it.

[Open Schedule Quality](pages/schedule-quality.md)

## Schedule Insights

### Critical Path Evolution

Shows which activity drove the project finish date in each reporting period, when that changed, and how much delay each step introduced compared with delay carried forward from earlier in the path. It uses the same delay-attribution maths as Delay Analysis, but builds its own driving-path history from critical-flagged activities instead of running a full traceback.

[About Critical Path Evolution](pages/critical-path-evolution.md)

### Bow-Wave Compression

Shows how planned work volume is spread across the project timeline and how that spread has changed between schedule updates, in a Schedule Summary view and a Schedule Forensics view with a Criticality filter.

[Open Bow-Wave Compression](pages/bow-wave-compression.md)

### Causes of Change

Finds the group of activities that moved later together between two schedule updates and classifies the movement as an identified root cause, an ambiguous one or a logic cycle. It also looks for constraint evidence, to tell a real event apart from routine re-baselining.

[About Causes of Change](pages/causes-of-change.md)

## Schedule Predictions

### Completion Forecast

Puts the reported finish date next to a projection based on the schedule's own track record, so you can see the gap between the claimed and the likely finish. The projection comes from the schedule's update history, not a simulation, so the same inputs always give the same answer.

[About Completion Forecast](pages/completion-forecast.md)

### Schedule Risk Analysis (SRA)

Runs a Monte Carlo schedule-risk simulation on your uploaded schedule and an optional pasted risk register, entirely in your browser, and ranks the risk drivers by how far they move the finish date.

[About Schedule Risk Analysis (SRA)](pages/schedule-risk.md)

### Float Burn-down & Earned Schedule

Shows two independent estimates of the real finish date. One is based on how fast the schedule's buffer (float) is being used up, and the other on how much work has been completed against plan. They're shown as an optimistic and a pessimistic bound, and a large gap between them is itself a finding.

[About Float Burn-down & Earned Schedule](pages/float-burndown-earned-schedule.md)
