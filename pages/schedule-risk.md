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

## Schedule Risk Analysis (SRA)

A finish date on its own doesn't tell you how much uncertainty sits behind it, or which risks matter most to it. Schedule Risk runs a quantitative schedule risk analysis – the kind of Monte Carlo simulation planners traditionally run in a separate risk tool – directly against your uploaded schedule, and ranks the risk drivers by how much they actually move the finish date.

Keeping this analysis inside Logic+, against the same schedule data as the rest of your reliability picture, means the risk view doesn't live disconnected in a separate spreadsheet nobody re-reads.

**Note:** the simulation itself runs entirely in your browser – the schedule and risk register are never sent anywhere to compute the result, and saved risk models are kept in your browser's local storage only.

## What you provide

- **A schedule** – selected from the schedules already uploaded to the project; no separate file upload is needed for this module.
- **A risk register** (optional) – paste rows copied from a spreadsheet, with columns for risk ID, the activity or WBS area it targets, risk name, type, likelihood, and a minimum/most likely/maximum duration impact. Column headers are matched automatically.

## Report details

A **Tornado chart** ranking the activities or WBS areas that most affect the finish date – by duration uncertainty, by risk register impact, or both together, depending on the analysis mode selected.

Headline finish-date statistics at **P50, P80, and P90** (the 50th, 80th, and 90th percentile finish dates across the simulation), plus a full distribution you can inspect. You can also drill into individual milestones to see their own P50/P80/P90 distribution.

## Options

Steps 1 and 2 of the screen set up the risk model:

| Setting | What it controls |
|---|---|
| Risk-model level | The WBS level risk ranges are applied at (step 1) – each WBS area at that level becomes one risk block |
| Preset | Conservative, Balanced + opportunity (the default) or Opportunity-rich starting ranges (step 2). You can also type your own optimistic / most likely / pessimistic percentages, or override them per block |

## Running the simulation

Step **3 · Run** has four settings, then **Run simulation**. The note under the button changes to describe the analysis you've picked.

### Analysis

Chooses what is allowed to vary from one simulated run to the next.

| Option | What varies | What you'll see in the results | Use it to |
|---|---|---|---|
| **Uncertainty (tornado)** | Only the duration ranges from step 2. The risk register is ignored. | Forecast completion, Distribution of finish dates, Criticality index and **Duration sensitivity** (the tornado chart) | Find which parts of the schedule the finish date is most sensitive to |
| **Risk register** | Only the risks and opportunities in your register. Every duration stays at plan. | Forecast completion, Distribution of finish dates, Criticality index and **Risk register impact**. There's no Duration sensitivity chart, because durations don't vary. | Assess the effect of your named risks on their own |
| **Combined** *(default)* | Both: every block carries its duration range, and the register fires on top | Every card, including Duration sensitivity and Risk register impact | Run a standard quantitative schedule risk analysis (QSRA) |

> **Two things to watch.** In **Combined**, set the duration ranges as if the listed risks *don't* happen – otherwise the same delay is counted twice, once in the range and again when the risk fires. In **Risk register**, you need at least one risk that matches the schedule: with an empty register, or none of its rows matched, every run is identical and P50, P80 and P90 all equal the plan finish.

### Detail

| Option | How it works | Use it for |
|---|---|---|
| **Summarised (block drivers)** *(default, recommended)* | Every activity in a WBS block shares the same random draw in each run, so the block moves together. Rankings are by WBS block. | Forecasting. It keeps realistic swings in the result, and it's the only setting that can show a genuine chance of finishing early. |
| **Full (every activity)** | Every activity varies on its own. Across thousands of activities the ups and downs cancel out along each path, the spread collapses, and almost every run finishes late. | A contrast only – not a forecast |

### Iterations

How many times the schedule is simulated: **1,000**, **2,000** *(default)* or **5,000**. Each run starts from a new random seed, so the results shift slightly every time you run, even with the same settings. More iterations give smoother percentiles and smaller shifts between runs, but take longer.

### Plan baseline

The date the results are measured against. It **doesn't change the simulation** – the P50, P80 and P90 dates stay the same either way – but it changes:

- the first tile in **Forecast completion**, labelled **Deterministic (logic)** or **Schedule finish**
- the working days ahead of or behind plan shown under P50, P80 and P90
- **Chance of finishing ≤ plan**
- the green "beat plan" area in **Distribution of finish dates**

| Option | The plan date is… | Use it when |
|---|---|---|
| **Forward pass (logic)** *(default)* | The schedule recalculated from its earliest activity start using its own logic and planned durations, with no uncertainty, constraints or progress – the same model the simulation uses | You want a like-for-like comparison: how far the ranges and risks alone move the finish |
| **Schedule finish** | The latest finish date of any activity in the imported schedule – the finish your scheduling tool calculated, including progress and constraints | You want the chance of meeting the date in the programme itself. Because the simulation leaves out progress and constraints (see below), part of the gap is the difference between the two models, not risk. |

The **Forecast completion** card also shows the imported schedule finish and explains any gap to the logic date.

## Calculation and other logic

**Data used:** the selected schedule, plus an optional pasted risk register (risk ID, target activity or WBS area, likelihood, and a minimum/most-likely/maximum day impact per risk). Risk register rows are matched to activities by **exact activity code match** – there is no fuzzy or approximate matching, so a mistyped or reformatted activity code in a pasted row will not match and that risk will be excluded from the run rather than partially matched.

**How it's calculated:** each simulation run draws a random duration multiplier for every activity or WBS block from its three-point range (optimistic/most likely/pessimistic, as a percentage of plan, set by the chosen preset or a per-block override), using a **triangular distribution** – the standard three-point-estimate shape for this kind of model, weighted toward the most-likely value rather than spread evenly between the extremes:

> For optimistic value *a*, most likely value *m*, pessimistic value *b*, and a random draw *u* between 0 and 1: the crossover point *c* = (*m* − *a*) ÷ (*b* − *a*). If *u* < *c*, the drawn multiplier = *a* + √(*u* × (*b* − *a*) × (*m* − *a*)); otherwise the drawn multiplier = *b* − √((1 − *u*) × (*b* − *a*) × (*b* − *m*))
> Simulated duration = planned duration × drawn multiplier

In "Summarised" detail, every activity in the same WBS block shares one drawn multiplier per run, so the block moves together; in "Full" detail, every activity draws its own multiplier independently. The schedule's logic is then re-evaluated forward with these simulated durations, and the resulting project finish date is recorded – repeated for the chosen number of iterations.

If a risk register is included, each risk is tested independently on every run: it "fires" if a separate random draw is below its stated likelihood, and if it fires, its day-impact is drawn the same way, from its own minimum/most-likely/maximum range, using the triangular-distribution formula above (an opportunity's drawn impact is subtracted rather than added). A risk aimed at a specific activity adds its drawn days straight onto that activity's simulated duration; a risk aimed at a WBS block instead stretches that block's duration proportionally:

> Block stretch factor = 1 + (drawn day-impact ÷ the block's own internal critical-path length), never allowed below 0.05 – so a risk can't shrink a block to less than 5% of its length

Across every iteration, two separate statistics are produced. The **Tornado ranking** is a **Pearson correlation** between each activity or WBS area's random draw across all iterations and the resulting project finish date across those same iterations, ranked by the size of that correlation regardless of direction:

> Tornado rank = |Pearson correlation between the area's per-iteration random draw and the per-iteration project finish date|, highest first

The **criticality index** asks a different question – not how much an area's uncertainty moves the finish date, but how often that area actually ends up being the thing the finish date depends on:

> Criticality index (%) = the percentage of iterations in which that activity or WBS area contained the activity that actually determined the finish date in that run (the one with the latest calculated finish after the forward pass)

An area can rank high on one measure and low on the other – for example, a naturally short, low-uncertainty activity that's nonetheless on the driving path in every run scores a high criticality index but a modest Tornado rank, because varying its duration barely moves the outcome.

Date constraints already in the schedule (Start On, Finish On, Finish On or After, Mandatory Start, Mandatory Finish, As Late As Possible, and others) are not applied during the simulation – every constraint type is uniformly set aside ("neutralised"), not selectively removed, so the simulation reflects the schedule's logic and durations alone. This is also why, under the "forward-pass logic" plan-baseline setting, the model's own deterministic (no-uncertainty) finish date can differ from P6's constrained finish date – it is the schedule's real logic, unclamped. Each run uses a fresh random seed taken from the current time – running the simulation again on unchanged inputs and settings produces a new random draw and a slightly different result, not a repeat of the previous run.

**Progress is not modelled.** Every run starts the whole schedule from its earliest activity start and uses each activity's planned duration. Actual dates, remaining durations and the data date are not used, so work that is already complete is simulated too. On a schedule update, the results describe the plan as a whole rather than just the work still to do – bear this in mind when reading the P-dates.

## Note

This model does not account for progress to date, correlation between unrelated activities, cascading risk effects, or resource constraints – it is a schedule-logic and duration-uncertainty model, not a full project risk simulation. Treat the ranking as a guide to where to focus attention, not a complete risk assessment on its own.
