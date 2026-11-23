---
page-id: traceback-engine
route: /delay-analysis
title: How the Traceback Method Works
audience: external
status: draft
version: 1.8.1
last-reviewed: 2026-11-16
blocked-reason: 2026-11-16 – Link renamed to Delay Attribution Scenarios. 2026-11-13 – 2026-11-13 – Added the cumulative-delay-across-calendars note (LUSB-1368, v1.2.36; changelog: "Corrected traceback cumulative delay when passing between activities with different working calendars" and "Corrected task-calendar fragnet transfers to use the owning activity's working calendar"). Verified against CHANGELOG.md and getCumulativeAssignedDelayByActivityId in delayAnalysis.utils.ts. 2026-10-13 – 2026-10-13 – "Driving Task" section renamed "Driving Path" and rewritten: it is a selectable Algorithm Profile again (LUSB-1299, v1.2.18+), as well as powering the Driving Task column / Driving path tab. Anchor changed #driving-task → #driving-path; sidebar and inbound links updated. 2026-09-30 – Stage two table rewritten and reordered to match the app's own order (Traceback Settings modal and Activity Scoring tab), with on-screen names used directly instead of a separate name-mapping note; descriptions verified against `@lware/traceback/src/measure/` and `profiles/` and aligned word-for-word with traceback-setup.md. Corrected: Author (creator only, not last editor), Same File (nearby updates score partially), Max Float (candidate's own total float under Default/Calibrated, not a path total), Sequence Number (internal ID). Stage one Indirect Evidence now mentions the LUSB-1288 adjustments. Removed the Calibrated "16th factor" paragraph – `sequenceDictionary: false` in both profiles (commit 27babf324). Added a line on secondary scores being relative (rank-normalised). Max Work/Complexity worded neutrally pending engineering – see workspace/GAPS.md 2026-09-30. LUSB-1288 (merged 2026-09-25, "implied link experiment" despite the branch name – it's a real, live, unflagged change) rewrote the candidate-scoring logic behind the Implied Logic flag to weigh recency, task-completion status, and historical linkage patterns across prior schedule updates, on top of date proximity. Flag definition unchanged; candidate selection results can differ from before this date. Fixed two dangling anchors to gantt-delay-analysis.md's now-renamed "Activity Scoring" tab (was "Activity Candidates," merged with the former "Candidate Scoring" tab same day, LUSB-1234). Restructured 2026-09-24 as part of the Delay Analysis consolidation – retitled from "How Traceback and Delay Attribution Work" and reframed as documentation for the Traceback method specifically (Delay Analysis now supports multiple methods; this page has never covered Time-Impact Analysis and was previously mistitled as if it were the whole module). Added worked examples to the "From chain to delay" flag table, adapted from the shipped-category scenarios in pages/delay-attribution-2.md (S01–S19) – Calendar has no equivalent base scenario in that set, so its example here is written fresh rather than adapted, and isn't independently diagrammed yet. "Driving Task" section updated 2026-09-23 – it's no longer a selectable Algorithm Profile (removed from Traceback Setup's dropdown), only the calculation behind the Driving Task column/Driving path tab now. The flag table itself was fully rewritten 2026-09-18 (LUSB-1171, "Delay Attribution 2.0" categories now shipped) — Lag Change, Start Change, No Link and New are gone, replaced by a Constraint flag plus renamed/expanded Relationship, Progress, Implied Logic and New fragnet flags. Verified directly against `frontend/src/analytics/delay/delay.types.ts` and tooltip strings in `delayAnalysis.utils.ts`, not yet screenshot-verified live. Also corrected the Stage One confidence flag name from "Start Change" to "Low Confidence" (`activityColumns.tsx` column id `lowConfidence`) – "Start Change" no longer exists anywhere in the codebase; it's unrelated to the delay-attribution flag rename above, just a coincidentally-timed second correction. Added cross-reference notes 2026-09-23 reconciling this page's longer criterion names against Traceback Setup's shorter on-screen labels (Closest Link/Implied Link, Max Work/Max Float/Complexity) — same signals, different names, previously undocumented as the same thing.
---

This page is about the **Traceback** method in Delay Analysis: *why* it picks the activities it does, and *how* a completed traceback path becomes a day-by-day delay figure for each activity. Two other pages cover the rest of the Traceback method:

- For running a traceback and adjusting its settings, see [Traceback Setup](pages/traceback-setup.md).
- For the tray, the log and restarting a traceback, see [Traceback Results](pages/gantt-delay-analysis.md).
- Delay Analysis's other method, [Time-Impact Analysis](pages/time-impact-analysis.md), doesn't use anything on this page.

## What traceback is actually doing

Traceback works backward through your schedule one step at a time, asking the same question at each activity: **which activity most likely drove this one to move?**

Starting from the activity you select, Logic+ looks at every other activity that could reasonably have caused it to move, scores each one and picks the strongest candidate. That candidate becomes the new starting point, and the question repeats. Run back toward the start of the project, this builds a **chain**: the sequence of activities that, step by step, drove your selected activity to where it is now.

At every step, Logic+ scores a shortlist of real candidates against a fixed set of criteria, and you can see exactly how each one scored in the [Activity Scoring](pages/gantt-delay-analysis.md#activity-scoring) tab.

Most of this page describes the **Default** and **Calibrated** algorithm profiles (formerly "Replica v2.1" and "Calibrated v1", with the same rule sets), which score candidates. The third profile, **Driving Path**, doesn't score at all; the next section covers it.

## Driving Path

**Driving Path** is an algorithm profile you can select in Traceback Setup. It's also the calculation behind two features that work without running a traceback: the **Driving Task** column in [Activity relationships](pages/schedule-analysis.md#activity-relationships), a flag on each predecessor row, and the **Driving path** tab, a chart of the chain. Both are available throughout the Schedule Viewer and Delay Analysis.

Instead of scoring candidates against weighted criteria, it looks only at the schedule's own logic links and dates, and follows whichever predecessor is driving the target activity's date. This is the same calculation Logic+ uses to work out the critical path, so there are no weights or exclusion criteria to tune. In the Driving Task column, a red flag means an activity directly drives the one you're looking at, and an amber flag means it drives it indirectly, further back along the chain.

Driving Path traces through completed activities as well as remaining work, without stopping at an activity that has finished. It uses each activity's imported dates, as they came from the schedule file, not dates Logic+ has recalculated.

## How the candidate field gets narrowed down

Before scoring, Logic+ narrows the field to activities that could reasonably be a driving predecessor, which keeps implausible matches out of the comparison. At a given step, an activity is left out if, for example, it finishes long after the current activity starts, its whole date range sits inside the current activity's dates, it finished long before the current activity starts, or it's a summary or Level-of-Effort row instead of real, dated work.

You can see and adjust several of these rules in [Activity Exclusion Criteria](pages/traceback-setup.md#step-3--adjust-settings-optional).

If a step finds no eligible candidate, Logic+ widens the search automatically and tries again, so the chain doesn't stop dead on a schedule with sparse or missing logic.

## The two-stage scoring process

Logic+ has 15 scoring criteria for judging a candidate. If all 15 counted at once, weak or coincidental signals would carry the same weight as strong, direct evidence. Two activities sharing the same author, for example, could help outscore a logic link deliberately built into the schedule.

So scoring runs in two stages:

| Stage | What happens | Purpose |
|---|---|---|
| **1 – Priority scoring** | Every eligible candidate is scored on 2 criteria, the strongest and most direct forms of evidence | Narrows the candidate pool to a shortlist (10, by default) of plausible drivers |
| **2 – Full scoring** | Only the shortlisted candidates are scored on the remaining 13 criteria, added to their priority scores | Finds the single best match among the plausible candidates, using every available signal |

A candidate has to reach the shortlist on strong evidence before the finer supporting factors help decide which strong candidate wins.

## Stage one – the two priority signals

*[Traceback Setup](pages/traceback-setup.md#step-3--adjust-settings-optional)'s Candidate Score Weights calls these two signals "Closest Link" and "Implied Link".*

**Direct Evidence: Defined Link.** Is there a formal logic link between the candidate and the current activity (Finish-Start, Start-Start, Finish-Finish or Start-Finish), as entered in the schedule? This is the strongest evidence available. If the schedule says one activity must finish before another starts, and the dates support that, the dependency was planned on purpose. Link types don't carry equal weight: Finish-Start counts as by far the strongest evidence, because it's how most real construction sequences work.

**Indirect Evidence: Date Proximity.** Even without a formal link, do the candidate's dates line up closely with the current activity's, in a way that fits a driving relationship? Schedules are rarely fully linked, so date proximity is the next best evidence: an activity finishing right when the current one started strongly suggests cause and effect, even with no link recorded. Logic+ measures this in working days, not calendar days, so an ordinary Friday finish and Monday start isn't counted as lost time. The score is then adjusted for how recent the candidate's schedule update is, whether the candidate had finished by its data date, and whether the two activities were linked in earlier schedule updates.

### Why these two come first, and what happens when neither is strong

Direct Evidence is as close to proof as schedule data gets, and Indirect Evidence is the best substitute when direct evidence is missing. Everything scored in Stage Two only supports the case. It helps decide between two similar candidates, but shouldn't overrule a formal link or a clear date match on its own.

When neither Direct nor Indirect Evidence strongly supports the candidate Logic+ had to pick at a step, the step is flagged **Low Confidence** instead of being reported as a scheduling defect. The chain still continues through that step, but treat the pick as the best available answer on weak evidence, not a confirmed cause. The flag measures how much evidence backed the selection, not how many working days separate the two activities. A wide gap in time doesn't automatically produce a Low Confidence flag, and a Low Confidence flag doesn't necessarily mean the dates were far apart.

*Confidence flagging is part of the Default and Calibrated algorithm profiles. See [Traceback Setup](pages/traceback-setup.md#step-2--select-an-algorithm-profile-optional) to check which profile you're using.*

## Stage two – full scoring

Once the shortlist is built, each candidate on it is also scored on the criteria below, added to its Stage One score. The candidate with the highest total is selected as the driver, added to the chain, and becomes the starting point for the next step back.

Unlike the two priority signals, these scores are **relative**: on each criterion, the best-matching shortlisted candidate gets the full weight and the others get smaller shares in rank order.

The criteria are listed in the same order and under the same names as Traceback Setup's Candidate Score Weights and the Activity Scoring tab.

| Criterion | What it measures | Why it matters |
|---|---|---|
| Finish Variance | Whether the candidate has slipped from its baseline finish by almost exactly the same amount as the current activity | Activities that have drifted in the same way were often affected by the same underlying cause |
| WBS Structure | How many levels of the Work Breakdown Structure the two activities share, from the top down | Activities in the same WBS branch share scope, management and resourcing |
| Activity Name | Words the two activity names share, with distinctive words (an area, asset or system reference) counting far more than common ones | Confirms two activities relate to the same physical scope of work |
| Primary Resource | How much of the current activity's resource work is done by resources also assigned to the candidate | Work done by the same crew is usually sequenced together |
| Created Date | How close together the two activities were first added to the schedule | Activities created around the same time often belong to the same planning session |
| Sequence Number | How close together the two activities' internal ID numbers are | A weak fallback signal, off by default, but useful if your schedule's build order is meaningful |
| Same File | Whether the two activities come from the same schedule update, or how far apart their updates are | Relevant when a traceback spans several schedule updates |
| Project ID | Whether the candidate and current activity belong to the same project | Prevents false connections between unrelated projects loaded together |
| Author | Whether the same person created both activities | Work planned by the same person or team is more likely to be logically related |
| Max Work | The total work built up along the candidate's chain of linked predecessors, compared with the current activity's | Shows how much work sits behind each activity |
| Max Float | The candidate's own total float; the least float scores highest | Activities with little or no float are more likely to be driving the schedule |
| Complexity | How many activities deep the candidate's chain of linked predecessors runs, compared with the current activity's | Shows how deeply each activity is embedded in the schedule's logic |
| Activity Coding | How many of the current activity's activity code values the candidate shares | Shared coding, defined by the planner, strongly suggests two activities belong to the same work |

That's 13 secondary criteria plus the 2 priority signals from Stage One, 15 in total. See [Candidate Score Weights](pages/traceback-setup.md#step-3--adjust-settings-optional) to view or adjust how much each one counts.

## How calendars affect the result

Every day-based comparison above (gaps, durations, lags) uses the right working calendar for the activities involved, not one calendar for everything. If part of your schedule runs on a different calendar, such as a 7-day working week for one trade against a standard Monday–Friday programme calendar, Logic+ measures gaps and durations against each activity's own calendar. If your traceback spans several schedule files with different calendar setups, Logic+ also switches calendars correctly between them.

The running **Cumulative Delay** follows the same rule. When delay passes from an activity on one calendar to the next activity on another, it's counted again in the receiving activity's working days from that point on, instead of being carried over as a raw number. Delay carried by a fragnet that uses a task calendar is counted in the working calendar of the activity that owns it. To see unconverted elapsed days instead, switch the calendar icon in the [Delay Attribution panel](pages/gantt-delay-analysis.md#delay-attribution-panel) to calendar days.

## From chain to delay – how days get charged to each activity

Once the chain is built, Logic+ walks it in schedule order and works out how many days of delay to charge to each activity by comparing it with your baseline schedule. Four rules govern the charge:

- **Every activity has a window of influence.** It opens where the activity is first driven and closes when its own logic hands dates on to its successor. Whatever happens to the activity inside that window is charged to it, and whatever happens outside is charged zero, however dramatic it looks on the bar chart.
- **Each activity is charged only for the delay it added, not its total variance.** If the chain is already 10 days late when it reaches an activity, and that activity pushes things out a further 2 days, it's charged 2 days, not 12.
- **Plan edits and real-world performance are kept separate.** A duration, logic or constraint edit is always kept apart from actual dates moving, even when the two look the same on the chart.
- **The charge lands on the activity that owns the change**, never on a predecessor that happened to be nearby.

Each charge also carries a flag that explains why it was charged:

| Flag | What it means |
|---|---|
| Duration | The activity's own planned (or remaining) duration was edited from what was baselined |
| Relationship | A logic link to a predecessor was added, deleted, or had its type or lag changed vs baseline. It's charged on the successor, naming only the links that changed. |
| Constraint | A date constraint was added, removed or moved, measured by recalculating with and without the constraint |
| Calendar | The activity's, its predecessor's or the project's calendar differs from its baseline counterpart, moving dates with no change to duration, logic, constraints or progress |
| Implied Logic | Traceback proposed a driving predecessor with no recorded relationship in the baseline or current schedule. It's a candidate explanation, not a proven cause, because date correlation is evidence, not proof. |
| Progress | Actual or forecast dates moved and no other flag explains it. This covers real-world performance, such as a late start against otherwise-satisfied logic or work that ran slower than planned. |
| New fragnet | New, unbaselined scope was inserted into the network (see below) |

**New activities.** Where a traceback chain runs through activities that don't exist in the baseline schedule (new scope added since the baseline), Logic+ groups the run of new activities together. It charges the group's *effect* on the finish date, never the new work's own length, to the group's last member, flagged **New fragnet**.

**Resource levelling isn't yet a flag Logic+ can charge.** Delay caused by resource-levelling conflicts is attributed to whichever other flag best fits the date movement, because Logic+ doesn't yet model levelling as a cause of its own. This is a known limitation.

The result is a running delay total, activity by activity, with each charge flagged with its reason. It's shown in the [Delay Attribution panel](pages/gantt-delay-analysis.md#delay-attribution-panel) and the [Cumulative Delay](pages/gantt-delay-analysis.md#cumulative-delay) chart.

## Worked examples

Each example follows the same pattern: something changed (or simply happened), and it's charged to the one category that explains it. All figures are in working days.

**Duration.** Activity A's planned duration grows from 4 to 9 days. Its finish-to-start successor B is pushed the same 5 days, which flow through to Practical Completion. All 5 days are charged to **Duration**, against A. A's window of influence closes when it drives B's start, so B only carries the date forward. A shorter duration works the same way in reverse, charged as a negative figure.

**Relationship.** A new finish-to-start link is added from an existing activity C to B, and C finishes 4 days later than the logic B used to depend on. **Relationship** is charged against the new C→B link, measured as the difference between B's date under the old logic and under the new logic, with everything else held the same. C itself is charged nothing, because C didn't move; the link changed. A deleted or re-lagged link is charged the same way.

**Constraint.** A Start-No-Earlier-Than constraint is applied to B, holding it 5 days after where its logic would place it. **Constraint** is charged against B: Logic+ removes the constraint, recalculates, and charges the difference between the constrained and unconstrained dates. A removed constraint is charged the same way, as a negative figure.

**Calendar.** B's calendar changes from a 5-day working week to a 7-day one, with no other change to its logic, duration, constraints or progress. Because the calendar counts more days per week, B's dates move earlier although the activity itself hasn't changed. **Calendar** is charged against B, kept apart from every other flag because nothing else about the activity changed.

**Implied Logic.** B's start has moved 3 days and now tracks activity D's finish exactly, but there's no relationship between D and B in the baseline or the current schedule. Logic+ proposes D as the apparent driver from the date pattern alone and charges **Implied Logic** against B, explicitly as a candidate explanation, not a proven cause. The date correlation is evidence of something real, but a person has to confirm it.

*Logic+ chooses the candidate for this flag on more than date proximity. It also considers how current the candidate's data is, whether the candidate had finished by its own data date, and whether the same link has appeared in the schedule's past updates. The flag means the same thing as before, but tracebacks run before 25 September 2026 may propose a different candidate, and so flag different activities as Implied Logic, on the same schedule.*

**Progress.** A finishes exactly on its baseline date and its finish-to-start link to B is fully satisfied, yet B's actual start is 5 days later than planned, with nothing in the logic, constraints or durations to explain the gap. **Progress** is charged against B. It's the category for actual dates moving when nothing in the schedule's mechanics explains it.

**New fragnet.** A new, unbaselined 3-day activity is inserted into the A→B chain, pushing B by 3 days. It's charged to **New fragnet** by its actual effect on Practical Completion, not by its own length. If there had been float downstream to absorb it, the charge would be smaller than 3 days, or zero.

**Resource Levelling isn't in this list because it isn't a flag yet** (see the note above). See [Delay Attribution Scenarios](pages/delay-attribution-2.md) for every scenario Logic+ applies today, and for the wider catalogue this method is expected to grow into, including Resource Levelling and many more situations than the seven flags above cover.
