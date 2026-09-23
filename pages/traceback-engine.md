---
page-id: traceback-engine
route: /delay-analysis
title: How Traceback and Delay Attribution Work
audience: external
status: draft
version: 1.3.0
last-reviewed: 2026-09-23
blocked-reason: "Driving Task" section updated 2026-09-23 – it's no longer a selectable Algorithm Profile (removed from Traceback Setup's dropdown), only the calculation behind the Driving Task column/Driving path tab now. The "From chain to delay" flag table was fully rewritten 2026-09-18 (LUSB-1171, "Delay Attribution 2.0" categories now shipped) — Lag Change, Start Change, No Link and New are gone, replaced by a Constraint flag plus renamed/expanded Relationship, Progress, Implied Logic and New fragnet flags. Verified directly against `frontend/src/analytics/delay/delay.types.ts` and tooltip strings in `delayAnalysis.utils.ts`, not yet screenshot-verified live. Also corrected the Stage One confidence flag name from "Start Change" to "Low Confidence" (`activityColumns.tsx` column id `lowConfidence`) – "Start Change" no longer exists anywhere in the codebase; it's unrelated to the delay-attribution flag rename above, just a coincidentally-timed second correction. Added cross-reference notes 2026-09-23 reconciling this page's longer criterion names against Traceback Setup's shorter on-screen labels (Closest Link/Implied Link, Max Work/Max Float/Complexity) — same signals, different names, previously undocumented as the same thing. See workspace/GAPS.md for the open question on `pages/delay-attribution-2.md`'s status.
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

Everything on this page describes the **Replica v2.1** and **Calibrated v1** algorithm profiles – the only two selectable in Traceback Setup's Algorithm Profile dropdown today. A separate **Driving Task** calculation, covered below, also exists in Logic+ but isn't reachable from that dropdown any more.

## Driving Task

Driving Task is not a traceback algorithm profile you can select – it used to be, but that option was removed from Traceback Setup's dropdown. It survives as its own calculation, used in two places, independent of any traceback run: the **Driving Task** column in [Activity relationships](pages/schedule-analysis.md#activity-relationships) (a flag on each predecessor row) and the **Driving path** tab (a standalone chart of the chain) – both available throughout the Schedule Viewer and Delay Analysis, whether or not a traceback has been run.

The calculation itself is unchanged: instead of scoring candidates against weighted criteria, it looks only at the schedule's own logic links and dates and follows whichever predecessor is actually driving the target activity's date – the same calculation Logic+ uses to work out critical path. There's nothing to tune: no weights, no exclusion criteria. A red flag means an activity is directly driving the one you're looking at; an amber flag means it drives indirectly, further back along the chain.

Driving Task traces through completed activities as well as remaining work – it doesn't stop once it reaches an activity that's already finished – and uses each activity's imported dates (as they came from the schedule file) rather than dates Logic+ has recalculated.

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

*Shown in [Traceback Setup](pages/traceback-setup.md#step-3--adjust-settings-optional)'s Candidate Score Weights as "Closest Link" and "Implied Link" – same two signals, shorter on-screen names.*

**Direct Evidence – Defined Link.** Does a formal logical relationship exist between the candidate and the current activity – a Finish-Start, Start-Start, Finish-Finish, or Start-Finish link, as entered in the schedule? This is the strongest evidence available: if the schedule says one activity must finish before another starts, and the dates support that, the dependency was deliberately planned. Not every link type carries equal weight, though – Finish-Start is treated as by far the strongest evidence, because it's how most real construction sequences naturally work.

**Indirect Evidence – Date Proximity.** Even without a formal link, do the candidate's dates line up closely with the current activity's, in a way consistent with a driving relationship? Schedules are rarely fully linked in practice, so date proximity is the next-best evidence – an activity finishing right when the current one started is a strong hint of cause and effect, even with no formal link recording it. Logic+ measures this closeness in working days, not plain calendar days, so a completely ordinary Friday-finish-to-Monday-start transition isn't mistaken for lost time.

### Why these two come first – and what happens when neither is strong

Direct Evidence is as close to proof as schedule data gets, and Indirect Evidence is the best substitute when direct evidence is missing. Everything scored in Stage Two is corroborating at best – useful for deciding between two similar candidates, but not something that should overrule a genuine formal link or an obvious date match on its own.

When neither Direct nor Indirect Evidence gives strong support for the candidate Logic+ had to pick at a step, that step is flagged **Low Confidence** rather than reported the same way as a genuine scheduling defect. The chain still continues through that step – it just means the pick should be read as the best available answer given weak evidence, not a confirmed cause. This flag is about how much evidence backed the selection, not about how many working days separate the two activities' dates – a wide separation in time doesn't automatically produce a Low Confidence flag, and a Low Confidence flag doesn't necessarily mean the two activities' dates were far apart.

*This confidence flagging is part of the Replica v2.1 and Calibrated v1 algorithm profiles – see [Key Changes – Delay Analysis](pages/key-changes-delay-analysis.md) for which profile you're using.*

## Stage two – full scoring

Once the shortlist is built, every candidate on it is also scored on the criteria below, added to their Stage One score. The candidate with the highest total is selected as the driver, added to the chain, and becomes the new starting point for the next backward step.

*The first three rows below appear in Traceback Setup's Candidate Score Weights under shorter names: "Max Work", "Max Float", and "Complexity."*

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
| Sequence Number | How close together the two activities' internal line numbers are | A weak fallback signal – off by default, but useful if your schedule's build order is meaningful |

That's 13 secondary criteria plus the 2 priority signals from Stage One – 15 in total. See [Candidate Score Weights](pages/traceback-setup.md#step-3--adjust-settings-optional) to view or adjust how much each of these counts.

The **Calibrated v1** profile scores a 16th factor on top of these – a match against a construction-sequence dictionary (e.g. "excavate before pour"). It isn't user-adjustable, so it doesn't appear in Candidate Score Weights, but it does show up in the [Traceback Log](pages/traceback-log.md) for a Calibrated v1 run.

## How calendars affect the result

Every day-based comparison above – gaps, durations, lags – is measured using the correct working calendar for the activities involved, not one single calendar applied to everything. If part of your schedule runs on a different calendar (a 7-day working week for a particular trade, say, against a standard Monday–Friday programme calendar), Logic+ measures gaps and durations against each activity's own calendar. If your traceback spans multiple schedule files with different calendar setups, Logic+ also switches calendars correctly as it crosses between them.

## From chain to delay – how days get charged to each activity

Once the chain is built, Logic+ walks it in schedule order and works out how many days of delay to charge to each activity, by comparing it against your baseline schedule.

**Every activity has a window of influence** – it opens where the activity is first driven, and closes the moment its own logic hands dates to its successor. Whatever happens to an activity inside that window is its to answer for; whatever happens outside it is charged zero, however dramatic it looks on the bar chart. **Each activity is only charged for the delay it added, not its total variance** – if the chain was already running 10 days late by the time it reaches a given activity, and that activity's own contribution only pushes things a further 2 days out, it's charged 2 days, not 12. **An edit to the plan and a change in real-world performance are never mixed** – a duration, logic, or constraint edit is always kept separate from actual dates simply moving, even when they look identical on the chart. And **the charge always lands on the activity that owns the change**, never on a predecessor that just happened to be nearby.

Each charge also comes with a plain-language flag explaining why it was charged:

| Flag | What it means |
|---|---|
| Duration | The activity's own planned (or remaining) duration was edited from what was baselined |
| Relationship | A logic link to a predecessor was added, deleted, or had its type or lag changed vs baseline – charged on the successor, naming only the links that actually changed |
| Constraint | A date constraint was added, removed, or moved – measured by recalculating with and without the constraint |
| Calendar | The activity's, its predecessor's, or the project's calendar differs from its baseline counterpart, moving dates with no change to duration, logic, constraints, or progress |
| Implied Logic | Traceback proposed a driving predecessor with no recorded relationship in baseline or current – a candidate explanation, not a proven cause; date correlation is evidence, not proof |
| Progress | Actual or forecast dates moved and no other flag explains it – the catch-all for real-world performance (a late start against otherwise-satisfied logic, or work that simply ran slower than planned) |
| New fragnet | New, unbaselined scope was inserted into the network – see below |

**New activities** – where a traceback chain runs through activities that don't exist in the baseline schedule at all (genuinely new scope added since baseline), Logic+ groups the run of new activities together and charges the group's *effect* on the finish date – never the new work's own length at face value – to the group's last member, tagged **New fragnet**.

**Resource Levelling is not yet a flag Logic+ can charge** – delay genuinely caused by resource-levelling conflicts currently gets attributed to whichever other flag above best fits the date movement, since Logic+ doesn't yet model levelling as its own cause. This is a known gap, not a bug.

The end result is a running, activity-by-activity delay total, each one tagged with the reasoning behind it – shown in the [Delay Attribute](pages/delay-analysis.md#delay-attribute) summary and the [Cumulative Delay](pages/gantt-delay-analysis.md#cumulative-delay) chart.
