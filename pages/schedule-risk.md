---
page-id: schedule-risk
route: /module/8/project/:projectId
title: Schedule Risk Analysis (SRA)
audience: external
status: review
version: 1.3.0
last-reviewed: 2026-11-18
blocked-reason: 2026-11-18 – Run section (step 3) rewritten in detail against TornadoModule/model.ts and packetModel.ts at v1.2.40: Analysis (what varies, which result cards show, empty-register and double-counting caveats), Detail (the app's own 'contrast, not a forecast' warning for Full), Iterations (default 2,000, new random seed per run), Plan baseline (exactly what it changes; Forward pass = recalculated from the earliest activity start with planned durations, constraints and progress left out; Schedule finish = latest finish of any activity in the imported file). Added 'Progress is not modelled' (packetModel uses task.duration, forwardPass starts every activity at day 0; actuals, remaining duration and data date unused). Not screenshot-verified. The approved v6 restyle (dropdowns, 'Plan finish taken from') is not built yet – update names when it ships.
---

## Overview

Schedule Risk runs a quantitative schedule risk analysis on your uploaded schedule. This is the Monte Carlo simulation planners usually run in a separate risk tool. It estimates how much uncertainty sits behind the finish date and ranks the risk drivers by how far they move it. Because it works on the same schedule data as the rest of Logic+, you don't need to keep a separate risk spreadsheet in step with the programme.

**Note:** the simulation runs entirely in your browser. The schedule and risk register aren't sent anywhere to compute the result, and saved risk models are kept only in your browser's local storage.

## What you provide

- **A schedule**, chosen from the schedules already uploaded to the project. This module needs no separate upload.
- **A risk register** (optional). Paste rows copied from a spreadsheet, with columns for risk ID, the activity or WBS area it targets, risk name, type, likelihood, and a minimum, most likely and maximum duration impact. Logic+ matches the column headers automatically.

## Report details

A **Tornado chart** ranks the activities or WBS areas that most affect the finish date, by duration uncertainty, by risk register impact or by both, depending on the analysis mode.

Headline finish dates at **P50, P80, and P90** (the 50th, 80th and 90th percentile finish dates across the simulation) sit above a full distribution you can inspect. You can also drill into individual milestones to see their own P50, P80 and P90 dates.

## Options

Steps 1 and 2 of the screen set up the risk model:

| Setting | What it controls |
|---|---|
| Risk-model level | The WBS level the risk ranges apply at (step 1). Each WBS area at that level becomes one risk block. |
| Preset | Conservative, Balanced + opportunity (the default) or Opportunity-rich starting ranges (step 2). You can also type your own optimistic, most likely and pessimistic percentages, or override them per block. |

## Running the simulation

Step **3 · Run** has four settings, then **Run simulation**. The note under the button changes to describe the analysis you've picked.

### Analysis

This setting chooses what may vary from one simulated run to the next.

| Option | What varies | What you'll see in the results | Use it to |
|---|---|---|---|
| **Uncertainty (tornado)** | Only the duration ranges from step 2. The risk register is ignored. | Forecast completion, Distribution of finish dates, Criticality index and **Duration sensitivity** (the tornado chart) | Find which parts of the schedule the finish date is most sensitive to |
| **Risk register** | Only the risks and opportunities in your register. Every duration stays at plan. | Forecast completion, Distribution of finish dates, Criticality index and **Risk register impact**. There's no Duration sensitivity chart, because durations don't vary. | Assess the effect of your named risks on their own |
| **Combined** *(default)* | Both: every block carries its duration range, and the register fires on top | Every card, including Duration sensitivity and Risk register impact | Run a standard quantitative schedule risk analysis (QSRA) |

> **Two things to watch.** In **Combined**, set the duration ranges as if the listed risks *don't* happen. Otherwise the same delay is counted twice: once in the range and again when the risk fires. In **Risk register**, you need at least one risk that matches the schedule. With an empty register, or with none of its rows matched, every run is identical and P50, P80 and P90 all equal the plan finish.

### Detail

| Option | How it works | Use it for |
|---|---|---|
| **Summarised (block drivers)** *(default, recommended)* | Every activity in a WBS block shares the same random draw in each run, so the block moves together. Rankings are by WBS block. | Forecasting. It keeps realistic swings in the result, and it's the only setting that can show a genuine chance of finishing early. |
| **Full (every activity)** | Every activity varies on its own. Across thousands of activities the ups and downs cancel out along each path, the spread collapses, and almost every run finishes late. | Comparison only. Don't use it as a forecast. |

### Iterations

This is how many times the schedule is simulated: **1,000**, **2,000** *(default)* or **5,000**. Each run starts from a new random seed, so results shift slightly every time you run, even with the same settings. More iterations give smoother percentiles and smaller shifts between runs, but take longer.

### Plan baseline

This is the date the results are measured against. It **doesn't change the simulation**, so the P50, P80 and P90 dates stay the same either way. It does change:

- the first tile in **Forecast completion**, labelled **Deterministic (logic)** or **Schedule finish**
- the working days ahead of or behind plan shown under P50, P80 and P90
- **Chance of finishing ≤ plan**
- the green "beat plan" area in **Distribution of finish dates**

| Option | The plan date is… | Use it when |
|---|---|---|
| **Forward pass (logic)** *(default)* | The schedule recalculated from its earliest activity start using its own logic and planned durations, with no uncertainty, constraints or progress. This is the same model the simulation uses. | You want a like-for-like comparison: how far the ranges and risks alone move the finish |
| **Schedule finish** | The latest finish date of any activity in the imported schedule: the finish your scheduling tool calculated, including progress and constraints | You want the chance of meeting the date in the programme itself. The simulation leaves out progress and constraints (see below), so part of the gap comes from the difference between the two models rather than from risk. |

The **Forecast completion** card also shows the imported schedule finish and explains any gap to the logic date.

## Calculation and other logic

**Data used:** the selected schedule, plus an optional pasted risk register (risk ID, target activity or WBS area, likelihood, and a minimum, most likely and maximum day impact per risk). Register rows are matched to activities by **exact activity code**, with no fuzzy matching. A row with a mistyped or reformatted activity code doesn't match, and that risk is left out of the run.

**How it's calculated:** each simulation run draws a random duration multiplier for every activity or WBS block from its three-point range (optimistic, most likely and pessimistic, as a percentage of plan, set by the preset or a per-block override). The draw uses a **triangular distribution**, the standard shape for three-point estimates, which weights results toward the most likely value instead of spreading them evenly between the extremes:

> For optimistic value *a*, most likely value *m*, pessimistic value *b*, and a random draw *u* between 0 and 1: the crossover point *c* = (*m* − *a*) ÷ (*b* − *a*). If *u* < *c*, the drawn multiplier = *a* + √(*u* × (*b* − *a*) × (*m* − *a*)); otherwise the drawn multiplier = *b* − √((1 − *u*) × (*b* − *a*) × (*b* − *m*))
> Simulated duration = planned duration × drawn multiplier

In "Summarised" detail, every activity in the same WBS block shares one drawn multiplier per run, so the block moves together. In "Full" detail, every activity draws its own multiplier. Logic+ then runs the schedule's logic forward with these simulated durations and records the project finish date, and repeats this for the chosen number of iterations.

If you include a risk register, each risk is tested independently on every run. It fires when a separate random draw falls below its stated likelihood. When it fires, its day impact is drawn from its own minimum, most likely and maximum range using the same triangular formula, and an opportunity's impact is subtracted instead of added. A risk aimed at a specific activity adds its drawn days to that activity's simulated duration. A risk aimed at a WBS block stretches that block's duration in proportion:

> Block stretch factor = 1 + (drawn day-impact ÷ the block's own internal critical-path length), never allowed below 0.05, so a risk can't shrink a block to less than 5% of its length

The simulation produces two separate statistics. The **Tornado ranking** is the **Pearson correlation** between each activity or WBS area's random draw and the project finish date across all iterations, ranked by the size of the correlation in either direction:

> Tornado rank = |Pearson correlation between the area's per-iteration random draw and the per-iteration project finish date|, highest first

The **criticality index** measures how often an area ends up determining the finish date, instead of how far its uncertainty moves it:

> Criticality index (%) = the percentage of iterations in which that activity or WBS area contained the activity that actually determined the finish date in that run (the one with the latest calculated finish after the forward pass)

An area can rank high on one measure and low on the other. A short, low-uncertainty activity that sits on the driving path in every run scores a high criticality index but a modest Tornado rank, because varying its duration barely moves the finish.

The simulation ignores every date constraint in the schedule (Start On, Finish On, Finish On or After, Mandatory Start, Mandatory Finish, As Late As Possible, and others). All constraint types are set aside ("neutralised") in the same way, so the result reflects the schedule's logic and durations alone. That's why, under the "forward-pass logic" plan baseline, the model's deterministic finish date (with no uncertainty) can differ from P6's constrained finish date: it's the schedule's own logic without the constraints. Each run takes a fresh random seed from the current time, so running again with unchanged inputs gives a slightly different result.

**Progress is not modelled.** Every run starts the whole schedule from its earliest activity start and uses each activity's planned duration. Actual dates, remaining durations and the data date aren't used, so work that is already complete is simulated too. On a schedule update, the results describe the plan as a whole rather than the work still to do, so bear this in mind when reading the P-dates.

## Note

This model doesn't account for progress to date, correlation between unrelated activities, risks that trigger other risks, or resource constraints. It models schedule logic and duration uncertainty, so treat the ranking as a guide to where to focus attention, alongside a full risk assessment.
