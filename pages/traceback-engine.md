---
page-id: traceback-engine
route: /schedule-viewer/gantt
title: How Traceback and Delay Attribution Work
audience: external
status: draft
version: 1.0.0
last-reviewed: 2026-07-21
---

## About this page

This page explains *why* traceback picks the activities it does, and *how* a completed traceback path turns into a day-by-day delay figure for each activity. It's a companion to two other pages:

- For the mechanics of running a traceback and adjusting its settings, see [Traceback Setup](pages/traceback-setup.md).
- For the tray, the audit log, and restarting a traceback, see [Gantt Viewer – Delay Analysis](pages/gantt-delay-analysis.md).
- For how this behaves differently depending on your chosen Algorithm Profile, see [Key Changes – Delay Analysis](pages/key-changes-delay-analysis.md).

## What traceback is actually doing

Traceback works backwards through your schedule, one step at a time, asking the same question at each activity: **which activity most likely drove this one to move?**

Starting from the activity you select, Logic+ looks at every other activity that could plausibly have caused it to move, scores each one, and picks the strongest candidate. That candidate becomes the new starting point, and the question repeats. Run all the way back towards the start of the project, this builds a **chain** – the sequence of activities that, step by step, drove your selected activity to where it now sits.

Nothing here is a guess. At every step Logic+ scores a shortlist of real candidates against a fixed set of criteria, and can show you exactly how each one scored – see [Activity Candidates](pages/gantt-delay-analysis.md#activity-candidates) for where to view that.

## How the candidate field gets narrowed down

Before any scoring happens, Logic+ narrows the field to activities that could sensibly be a driving predecessor at all. This keeps the comparison focused and avoids obviously implausible matches ever getting a look-in. An activity is left out of consideration at a given step if, for example, it finishes long after the current activity starts, its whole date range sits inside the current activity's dates, it finished a long time before the current activity starts, or it's a summary/Level-of-Effort row rather than real, dated work.

You can see and adjust several of these narrowing rules yourself – see [Activity Exclusion Criteria](pages/traceback-setup.md#step-3--adjust-settings-optional).

If a step can't find any eligible candidate at all, Logic+ automatically widens the search and tries again, rather than giving up. This is what stops the chain from dead-ending on a schedule with sparse or missing logic.

## The two-stage scoring process

Logic+ has 15 scoring criteria it can use to judge a candidate. If all 15 voted at once, weak or coincidental signals would carry the same weight as strong, direct evidence – two activities happening to share the same author, say, could help edge out a genuine logic link deliberately built into the schedule.

So scoring runs in two stages:

| Stage | What happens | Purpose |
|---|---|---|
| **1 – Priority scoring** | Every eligible candidate is scored on 2 criteria – the strongest, most direct forms of evidence | Narrows the candidate pool to a shortlist (10, by default) of plausible drivers |
| **2 – Full scoring** | Only the shortlisted candidates are scored on the remaining 13 criteria, added to their priority scores | Finds the single best match among the plausible candidates, using every available signal |

A candidate has to earn its place on strong evidence before the finer-grained, corroborating factors get a vote on which strong candidate wins.

## Stage one – the two priority signals

**Direct Evidence – Defined Link.** Does a formal logical relationship exist between the candidate and the current activity – a Finish-Start, Start-Start, Finish-Finish, or Start-Finish link, as entered in the schedule? This is the strongest evidence available: if the schedule says one activity must finish before another starts, and the dates support that, the dependency was deliberately planned. Not every link type carries equal weight, though – Finish-Start is treated as by far the strongest evidence, because it's how most real construction sequences naturally work.

**Indirect Evidence – Date Proximity.** Even without a formal link, do the candidate's dates line up closely with the current activity's, in a way consistent with a driving relationship? Schedules are rarely fully linked in practice, so date proximity is the next-best evidence – an activity finishing right when the current one started is a strong hint of cause and effect, even with no formal link recording it. Logic+ measures this closeness in working days, not plain calendar days, so a completely ordinary Friday-finish-to-Monday-start transition isn't mistaken for lost time.

### Why these two come first – and what "Gap" really means

Direct Evidence is as close to proof as schedule data gets, and Indirect Evidence is the best substitute when direct evidence is missing. Everything scored in Stage Two is corroborating at best – useful for deciding between two similar candidates, but not something that should overrule a genuine formal link or an obvious date match on its own.

You may hear "gap" used in two different ways – worth keeping them separate. **Plain "gap"** is just elapsed time: the number of working days between one activity's dates and another's – a measurement, not a judgement. Separately, Logic+ can flag a step as a **Gap** when neither Direct nor Indirect Evidence gave strong support for the candidate it had to pick – meaning the chain still continues through that step, but it should be read as "best available answer" rather than a confirmed cause. A Gap-flagged step doesn't necessarily have a large plain-gap in time behind it, and a wide time gap doesn't automatically produce a Gap flag either – the flag is about how much evidence backs the selection, not how far apart the two activities sit in time.

*This confidence flagging is part of the Replica v2.1 and Calibrated v1 algorithm profiles – see [Key Changes – Delay Analysis](pages/key-changes-delay-analysis.md) for which profile you're using.*

## Stage two – full scoring

Once the shortlist is built, every candidate on it is also scored on the criteria below, added to their Stage One score. The candidate with the highest total is selected as the driver, added to the chain, and becomes the new starting point for the next backward step.

| Criterion | What it measures | Why it matters |
|---|---|---|
| Cumulative Work Content | Total effort built up along the candidate's dependency path | Major work packages are more likely to be critical to the outcome than minor, low-effort activities |
| Cumulative Float | How little spare time has built up along the candidate's path | Activities on a tightly constrained path are more likely to be genuinely driving the schedule |
| Network Complexity | How many activities deep the candidate's dependency chain runs | Candidates embedded in a well-linked, deliberately planned sequence are more trustworthy |
| WBS Structure | How closely the candidate's position in the Work Breakdown Structure matches the current activity's | Activities in the same WBS branch share scope, management, and resourcing |
| Activity Coding | Whether the candidate shares the same activity code values | Shared coding is a strong, planner-defined indicator that two activities belong to the same work |
| Primary Resource | Whether the candidate and current activity share resources doing similar amounts of work | Work performed by the same crew is naturally sequenced together |
| Finish Variance | How similarly the candidate's dates have already drifted from plan | Activities that have drifted in a similar way were often affected by the same underlying cause |
| Activity Name | How many meaningful words the two activity names have in common | Confirms two activities relate to the same physical scope of work |
| Created Date | How close together the two activities were originally created in the schedule | Activities created around the same time often belong to the same planning session |
| Author | Whether the candidate and current activity were created or last edited by the same person | Work planned by the same person or team is more likely to be logically related |
| Same File | Whether the candidate and current activity come from the same schedule update | Relevant when comparing across multiple schedule files |
| Project ID | Whether the candidate and current activity belong to the same project | Prevents false connections between unrelated projects loaded together |

See [Candidate Score Weights](pages/traceback-setup.md#step-3--adjust-settings-optional) to view or adjust how much each of these counts.

## How calendars affect the result

Every day-based comparison above – gaps, durations, lags – is measured using the correct working calendar for the activities involved, not one single calendar applied to everything. If part of your schedule runs on a different calendar (a 7-day working week for a particular trade, say, against a standard Monday–Friday programme calendar), Logic+ measures gaps and durations against each activity's own calendar. If your traceback spans multiple schedule files with different calendar setups, Logic+ also switches calendars correctly as it crosses between them.

## From chain to delay – how days get charged to each activity

Once the chain is built, Logic+ walks it in schedule order and works out how many days of delay to charge to each activity, by comparing it against your baseline schedule. For each activity, Logic+ looks at:

- **Finish variance** – actual finish vs. baseline finish
- **Start variance** – actual start vs. baseline start
- **Duration variance** – did the activity itself take longer or shorter than planned
- **Gap variance** – did the time between it and its predecessor grow or shrink, compared to what the baseline relationship implied

**Each activity is only charged for the delay it added, not its total variance.** If the chain was already running 10 days late by the time it reaches a given activity, and that activity's own contribution only pushes things a further 2 days out, it's charged 2 days – not 12. This stops delay being double-counted as it's traced back through the chain.

Each charge also comes with a plain-language flag explaining why it was charged:

| Flag | What it means |
|---|---|
| Relationship changed | The actual link type to its predecessor is different from the baseline schedule, and that change measurably affected the date |
| Duration | The activity's own duration changed from what was planned |
| Gap | The time between it and its predecessor grew or shrank |
| No link | There's no formal relationship to its predecessor, so the step is explained by date proximity rather than a real logic link |
| Calendar | The activity's own calendar behaves meaningfully differently from the project's standard calendar, which can itself explain some of the variance |

**New activities** – where a traceback chain runs through activities that don't exist in the baseline schedule at all (genuinely new scope added since baseline), Logic+ groups them together and charges the delay they represent to the activity judged to have introduced them, once it can see how much extra delay resulted from their addition.

The end result is a running, activity-by-activity delay total, each one tagged with the reasoning behind it – shown in the [Delay Attribute](pages/delay-analysis.md#delay-attribute) summary and the [Cumulative Delay](pages/gantt-delay-analysis.md#cumulative-delay) chart.
