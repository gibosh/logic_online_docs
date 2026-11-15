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

## About this page

Delay Analysis supports more than one method – this page is about **Traceback** specifically: *why* it picks the activities it does, and *how* a completed traceback path turns into a day-by-day delay figure for each activity. It's a companion to two other Traceback-method pages:

- For the mechanics of running a traceback and adjusting its settings, see [Traceback Setup](pages/traceback-setup.md).
- For the tray, the audit log, and restarting a traceback, see [Traceback Results](pages/gantt-delay-analysis.md).
- For Delay Analysis's other method, see [Time-Impact Analysis](pages/time-impact-analysis.md) – it doesn't use anything on this page.

## What traceback is actually doing

Traceback works backwards through your schedule, one step at a time, asking the same question at each activity: **which activity most likely drove this one to move?**

Starting from the activity you select, Logic+ looks at every other activity that could plausibly have caused it to move, scores each one, and picks the strongest candidate. That candidate becomes the new starting point, and the question repeats. Run all the way back towards the start of the project, this builds a **chain** – the sequence of activities that, step by step, drove your selected activity to where it now sits.

Nothing here is a guess. At every step Logic+ scores a shortlist of real candidates against a fixed set of criteria, and can show you exactly how each one scored – see [Activity Scoring](pages/gantt-delay-analysis.md#activity-scoring) for where to view that.

Most of this page describes the **Default** and **Calibrated** algorithm profiles (renamed from "Replica v2.1"/"Calibrated v1" – same rule sets), which score candidates. The third profile, **Driving Path**, doesn't score at all – it's covered in the next section.

## Driving Path

**Driving Path** is a selectable algorithm profile in Traceback Setup. It's also the calculation behind two features that work without running a traceback at all: the **Driving Task** column in [Activity relationships](pages/schedule-analysis.md#activity-relationships) (a flag on each predecessor row) and the **Driving path** tab (a standalone chart of the chain) – both available throughout the Schedule Viewer and Delay Analysis.

Instead of scoring candidates against weighted criteria, it looks only at the schedule's own logic links and dates and follows whichever predecessor is actually driving the target activity's date – the same calculation Logic+ uses to work out critical path. There's nothing to tune: no weights, no exclusion criteria. In the Driving Task column, a red flag means an activity is directly driving the one you're looking at; an amber flag means it drives indirectly, further back along the chain.

Driving Path traces through completed activities as well as remaining work – it doesn't stop once it reaches an activity that's already finished – and uses each activity's imported dates (as they came from the schedule file) rather than dates Logic+ has recalculated.

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

**Indirect Evidence – Date Proximity.** Even without a formal link, do the candidate's dates line up closely with the current activity's, in a way consistent with a driving relationship? Schedules are rarely fully linked in practice, so date proximity is the next-best evidence – an activity finishing right when the current one started is a strong hint of cause and effect, even with no formal link recording it. Logic+ measures this closeness in working days, not plain calendar days, so a completely ordinary Friday-finish-to-Monday-start transition isn't mistaken for lost time. The score is then adjusted for how recent the candidate's schedule update is, whether the candidate had actually finished by its data date, and whether the two activities were linked in earlier schedule updates.

### Why these two come first – and what happens when neither is strong

Direct Evidence is as close to proof as schedule data gets, and Indirect Evidence is the best substitute when direct evidence is missing. Everything scored in Stage Two is corroborating at best – useful for deciding between two similar candidates, but not something that should overrule a genuine formal link or an obvious date match on its own.

When neither Direct nor Indirect Evidence gives strong support for the candidate Logic+ had to pick at a step, that step is flagged **Low Confidence** rather than reported the same way as a genuine scheduling defect. The chain still continues through that step – it just means the pick should be read as the best available answer given weak evidence, not a confirmed cause. This flag is about how much evidence backed the selection, not about how many working days separate the two activities' dates – a wide separation in time doesn't automatically produce a Low Confidence flag, and a Low Confidence flag doesn't necessarily mean the two activities' dates were far apart.

*This confidence flagging is part of the Default and Calibrated algorithm profiles – see [Traceback Setup](pages/traceback-setup.md#step-2--select-an-algorithm-profile-optional) for which profile you're using.*

## Stage two – full scoring

Once the shortlist is built, every candidate on it is also scored on the criteria below, added to their Stage One score. The candidate with the highest total is selected as the driver, added to the chain, and becomes the new starting point for the next backward step.

Unlike the two priority signals, these scores are **relative**: on each criterion, the best-matching shortlisted candidate gets the full weight and the others get stepped-down shares in rank order.

The criteria are listed in the same order, and under the same names, as Traceback Setup's Candidate Score Weights and the Activity Scoring tab.

| Criterion | What it measures | Why it matters |
|---|---|---|
| Finish Variance | Whether the candidate has slipped from its baseline finish by almost exactly the same amount as the current activity | Activities that have drifted in the same way were often affected by the same underlying cause |
| WBS Structure | How many levels of the Work Breakdown Structure the two activities share, from the top down | Activities in the same WBS branch share scope, management, and resourcing |
| Activity Name | Words the two activity names share, with distinctive words (an area, asset or system reference) counting far more than common ones | Confirms two activities relate to the same physical scope of work |
| Primary Resource | How much of the current activity's resource work is done by resources also assigned to the candidate | Work performed by the same crew is naturally sequenced together |
| Created Date | How close together the two activities were first added to the schedule | Activities created around the same time often belong to the same planning session |
| Sequence Number | How close together the two activities' internal ID numbers are | A weak fallback signal – off by default, but useful if your schedule's build order is meaningful |
| Same File | Whether the two activities come from the same schedule update, or how far apart their updates are | Relevant when a traceback spans multiple schedule updates |
| Project ID | Whether the candidate and current activity belong to the same project | Prevents false connections between unrelated projects loaded together |
| Author | Whether the same person created both activities | Work planned by the same person or team is more likely to be logically related |
| Max Work | The total work built up along the candidate's chain of linked predecessors, compared with the current activity's | Gives a sense of how much work sits behind each activity |
| Max Float | The candidate's own total float – the least float scores highest | Activities with little or no float are more likely to be genuinely driving the schedule |
| Complexity | How many activities deep the candidate's chain of linked predecessors runs, compared with the current activity's | Gives a sense of how embedded each activity is in the schedule's logic |
| Activity Coding | How many of the current activity's activity code values the candidate shares | Shared coding is a strong, planner-defined indicator that two activities belong to the same work |

That's 13 secondary criteria plus the 2 priority signals from Stage One – 15 in total. See [Candidate Score Weights](pages/traceback-setup.md#step-3--adjust-settings-optional) to view or adjust how much each of these counts.

## How calendars affect the result

Every day-based comparison above – gaps, durations, lags – is measured using the correct working calendar for the activities involved, not one single calendar applied to everything. If part of your schedule runs on a different calendar (a 7-day working week for a particular trade, say, against a standard Monday–Friday programme calendar), Logic+ measures gaps and durations against each activity's own calendar. If your traceback spans multiple schedule files with different calendar setups, Logic+ also switches calendars correctly as it crosses between them. The running **Cumulative Delay** follows the same rule: when delay passes from an activity on one calendar to the next activity on another, it is re-counted in the receiving activity's working days from that point on, rather than carried over as a raw number (this was corrected in v1.2.36). Delay carried by a fragnet that uses a task calendar is likewise counted in the working calendar of the activity that owns it. Switch the calendar icon in the [Delay Attribution panel](pages/gantt-delay-analysis.md#delay-attribution-panel) to plain calendar days if you'd rather see unconverted elapsed days.

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

The end result is a running, activity-by-activity delay total, each one tagged with the reasoning behind it – shown in the [Delay Attribution panel](pages/gantt-delay-analysis.md#delay-attribution-panel) and the [Cumulative Delay](pages/gantt-delay-analysis.md#cumulative-delay) chart.

## Worked examples

Each flag below is the same shape in practice: something changed (or simply happened), and it's charged to the one category that actually explains it. All figures are in working days.

**Duration.** Activity A's planned duration grows from 4 to 9 days. Its finish-to-start successor B is pushed the same 5 days, flowing straight through to Practical Completion. The whole 5 days is charged to **Duration**, against A – A's window of influence closes the moment it drives B's start, so B just carries the date forward unchanged. A duration cut works the same way in reverse, charged as a negative figure.

**Relationship.** A new finish-to-start link is added from an existing activity C to B, and C finishes 4 days later than the logic B used to run on. **Relationship** is charged against the new C→B link, measured as the difference between B's date under the old logic and the new logic with everything else held constant – C itself is charged nothing, since C didn't move, the link did. A deleted or re-lagged link is charged the same way.

**Constraint.** A Start-No-Earlier-Than constraint is applied to B, holding it 5 days beyond where its logic would otherwise place it. **Constraint** is charged against B – the test is to remove the constraint, recalculate, and charge the difference between the constrained and unconstrained date. A removed constraint is charged as a negative figure the same way.

**Calendar.** B's calendar is swapped from a 5-day working week to a 7-day one, with no other change to its logic, duration, constraints, or progress. Purely as a result of counting more days per week, B's dates move earlier without the activity itself changing at all. **Calendar** is charged against B, isolated from every other flag precisely because nothing else about the activity changed.

**Implied Logic.** B's start has moved 3 days and now tracks activity D's finish exactly – but no relationship between D and B exists in the baseline or the current schedule. Logic+ surfaces D as the apparent driver from the date pattern alone, and charges **Implied Logic** against B – explicitly a candidate explanation, not a proven cause. The date correlation is evidence of something real, but confirming it is a human step.

*Updated 2026-09-25: candidate selection for this flag was enhanced to weigh more than date proximity alone – it now also considers how current the candidate's data is, whether the candidate task had actually finished by its own data date, and whether that same link has shown up historically across the schedule's own past updates. The flag's meaning is unchanged, but which candidate gets proposed – and therefore which activities get flagged as Implied Logic – can differ from a traceback run before this date on the same schedule.*

**Progress.** A finishes exactly on its baseline date and its finish-to-start link to B is fully satisfied – yet B's actual start is 5 days later than planned anyway, with nothing in the logic, constraints, or durations explaining the gap. **Progress** is charged against B: the residual category for when actual dates move and nothing in the schedule mechanics explains it.

**New fragnet.** A new, unbaselined 3-day activity is inserted into the A→B chain, pushing B 3 days. It's charged to **New fragnet** at the actual before/after effect on Practical Completion, not the new work's own length at face value – if there had been float downstream to absorb it, the charge would be smaller than 3 days, or zero.

**Resource Levelling isn't in this list because it isn't a flag yet** – see the note above. See [Delay Attribution Scenarios](pages/delay-attribution-2.md) for every scenario Logic+ applies today, and the wider catalogue this method is expected to grow into, including Resource Levelling and a much wider range of situations than the seven flags above cover today.
