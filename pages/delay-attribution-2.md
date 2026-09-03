---
page-id: delay-attribution-2
title: Delay Attribution 2.0
audience: external
status: draft
version: 0.5.0
last-reviewed: 2026-09-03
blocked-reason: This is a preview of an upcoming release, adapted from the Delay Attribution Scenarios spec (draft v0.5, "for alignment" — not yet finalised). Scenario numbering, wording and treatments may still change before this ships. Do not treat this as documentation of current behaviour.
---

## Delay Attribution 2.0 – Overview

Right now, when Logic+ tells you a chain of activities caused a delay, it's harder than it should be to see exactly *why* — which rule fired, which activity actually carries the charge, and whether the number would survive being challenged in a real delay claim.

Delay Attribution 2.0 is a rebuild of that logic around one question: **for every day added to or saved from Practical Completion, can Logic+ point to the one specific thing that caused it, and defend the number?**

It works scenario by scenario. Instead of one general rule trying to cover every situation, the new engine recognises a fixed catalogue of situations — a duration was edited, a constraint was added, an activity simply ran late with nothing else changing — and applies a specific, tested treatment to each one. This page walks through that catalogue the way the tool will apply it: what the situation looks like, what Logic+ decides, and why.

**This is a preview, not a change you'll see in the app yet.** The content below is adapted from the underlying specification while it's still in draft — treat it as "this is the plan," not "this is what Logic+ does today."

---

## The eight attribution categories

Every day of movement at Practical Completion gets charged to exactly one of eight categories (or, for a handful of genuinely tied cases, to more than one at once — see [Concurrent drivers](#s13-concurrent-drivers) below).

| Category | What it means |
|---|---|
| **Duration Change** | The planned duration of an activity — original or remaining — was edited in the schedule. |
| **Relationship Change** | A logic link was added, deleted, or had its lag or type changed. |
| **Constraint Change** | A date constraint was added, removed, or moved. |
| **Progress Change** | Actual dates moved the activity and nothing else explains it — a late start against logic that was otherwise satisfied, or work that simply ran slower than planned. This is the catch-all for real-world performance, used only when nothing else accounts for the movement. |
| **Implied Logic** | Logic+ spots an activity tracking another one's dates with no formal link behind it. Flagged as a candidate explanation — never reported as a proven cause. |
| **Fragnet / New Activity** | New scope was added to the schedule, whether a single activity or a whole chain. Charged by the actual effect on the finish date, never by how long the new work itself is. |
| **Resource Levelling** | Delay produced by the resource-levelling engine. Only ever attributed once every other category has been ruled out *and* a genuine resource conflict backs it up. |
| **Calendar Change** | An activity's calendar was swapped, or the calendar itself was edited (holidays, working days). Moves dates without touching duration, logic, constraints, or progress. |

---

## How to read the scenario diagrams

Each scenario below is illustrated with a small before/after diagram, read left to right:

| On the diagram | Means |
|---|---|
| Solid blue bar | The activity's current position — actual dates where the work is complete, forecast dates where it's still ahead |
| Thin yellow bar underneath | The baseline position, so you can see the shift directly against it |
| Solid arrow | A driving relationship — the logic actually setting the successor's dates |
| Dotted arrow | A relationship that exists but isn't driving anything right now (satisfied, redundant, or overtaken by something else) |
| Red crossed arrow | Deleted logic |
| Red marker | A constraint |
| A link labelled "new" | Only exists in the current schedule, not the baseline |
| Vertical blue line | The data date |
| Hatched blue bar | An inserted activity or fragnet, with no baseline counterpart |

On the right, the **Effect at PC** panel shows the same story as it lands on Practical Completion: the yellow diamond is baseline PC, the blue diamond is current (or forecast) PC, and the arrow between them is the movement — with the category it's charged to written underneath.

One thing to note throughout: **everything here applies equally to work that's already happened and work that's still ahead.** Where an activity has finished, the blue bar is as-built and the charge is retrospective fact. Where an activity is still ahead of the data date, the blue bar is a forecast — the same rules apply unchanged, just against the projected dates instead of actual ones.

---

## The rules behind every scenario

Eight ideas govern every treatment decision in this catalogue. Once these make sense, most individual scenarios are just this logic being applied to a specific situation.

**Every activity has a window of influence.** It opens where the activity is first driven, and closes at the moment its own logic hands dates to its successor. Under a finish-to-start or start-to-start link, that hand-over happens at the successor's start; under finish-to-finish or start-to-finish, at the successor's finish. Whatever happens to an activity *inside* that window is its to answer for. Whatever happens outside it is charged zero — however dramatic it looks on the bar chart.

**Only movement that's actually driving something is chargeable.** An activity can run late and still charge nothing at all, if the lateness is absorbed by float and never reaches the critical path.

**A relationship is charged for exactly what it proves — no more.** If a link only explains part of a movement, that part goes to the link and the rest goes to Progress Change. No category is ever allowed to silently absorb movement it can't actually account for.

**Insertions are measured by their effect, not their length.** A new fragnet or activity is charged the difference between Practical Completion with it in the schedule and without it — never its own duration at face value.

**An edit to the plan and a change in real-world performance are never mixed.** If someone changed the schedule (duration, logic, a constraint), that's a Duration/Relationship/Constraint Change. If actual dates simply moved, that's Progress Change. The two are always kept apart, even when they look identical on the bar chart.

**Some attributions are candidates, not proven causes.** Implied Logic and Resource Levelling are both correlation-based — Logic+ flags them because the evidence points that way, but it never claims certainty it doesn't have.

**The charge always lands on the activity that owns the change**, never on a predecessor that simply happened to be nearby. A duration edit charges the edited activity; a late start charges the activity that started late; a new link charges the successor whose dates it now governs, with the link itself named. This is what keeps a per-activity total meaningful: it always means "what this activity actually contributed," never "what happened near this activity."

**Everything has to add up.** Across any period, the signed charges across all eight categories sum exactly to the real movement of Practical Completion — with genuinely tied drivers counted once, not double-counted.

---

## Group 1 – One clear cause, one simple chain

The foundation cases: one thing changes, the chain is a simple finish-to-start, and the effect flows straight through to Practical Completion. Every scenario after this group is one of these seven, made more complicated.

### S01 – Duration extension on the driving path

![Duration extension on the driving path](pages/images/delay-attribution-2/S01.png)

**What happens.** Activity A's planned duration grows from 4 to 9 working days. Its finish-to-start successor B is pushed the same 5 days, and the delay flows through unbroken logic all the way to Practical Completion.

**How it's charged.** The whole 5 days goes to **Duration Change**, against A. A's window closes the moment it drives B's start — from there, B just carries the date forward unchanged.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| A +5 | – | – | – | – | – | – | – | **+5** |

### S02 – Duration reduction (mitigation)

![Duration reduction](pages/images/delay-attribution-2/S02.png)

**What happens.** Activity A's duration is cut from 4 to 2 days. B pulls in 2 days earlier, and Practical Completion improves by 2 days.

**How it's charged.** **Duration Change** against A, as a negative value. Recording genuine gains matters just as much as recording delay — over a reporting period, the positives and negatives have to sum to the real net movement, or the model doesn't reconcile.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| A −2 | – | – | – | – | – | – | – | **−2** |

### S03 – Late start with logic satisfied

![Late start with logic satisfied](pages/images/delay-attribution-2/S03.png)

**What happens.** A finishes exactly on its baseline date. Its finish-to-start link to B is fully satisfied — yet B's actual start is 5 days later than planned anyway. Nothing in the logic, constraints, or durations explains the gap; the date is just later.

**How it's charged.** **Progress Change** against B. This is the residual category in action: when actual dates move and nothing in the schedule mechanics explains it, the delay belongs to that activity's own performance.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | – | – | B +5 | – | – | – | – | **+5** |

### S04 – Slow progress after an on-time start

![Slow progress after an on-time start](pages/images/delay-attribution-2/S04.png)

**What happens.** B starts exactly on its baseline date, but takes 9 days against a planned 4. The stretched bar pushes everything downstream by 5 days.

**How it's charged.** **Progress Change** against B — not Duration Change. The distinction that matters: Duration Change is an edit to the *plan* (someone revised the duration in the schedule); Progress Change is what actually happened on site. Same visible symptom on the bar chart, genuinely different cause.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | – | – | B +5 | – | – | – | – | **+5** |

### S05 – Constraint added

![Constraint added](pages/images/delay-attribution-2/S05.png)

**What happens.** A Start-No-Earlier-Than constraint is applied to B, holding it 5 days beyond where its logic would otherwise place it. The constraint, not the logic, now controls B's start.

**How it's charged.** **Constraint Change** against B. The test Logic+ applies: remove the constraint, recalculate, and the difference between the constrained and unconstrained date is the charge. The upstream logic is charged nothing — it was never the binding control.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | – | B +5 | – | – | – | – | – | **+5** |

### S06 – Constraint removed

![Constraint removed](pages/images/delay-attribution-2/S06.png)

**What happens.** The baseline carried a constraint holding B at a later date. That constraint is deleted, and B pulls back 3 days to its logic-driven date.

**How it's charged.** **Constraint Change** against B, negative. The mirror image of S05: constraints are charged for what they add, and credited for what they release.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | – | B −3 | – | – | – | – | – | **−3** |

### S07 – New relationship inserted

![New relationship inserted](pages/images/delay-attribution-2/S07.png)

**What happens.** A new finish-to-start link is added from existing activity C to B. C finishes later than A did, so B is now driven 4 days later than its baseline logic would place it.

**How it's charged.** **Relationship Change**, against the new C→B link. The measure is the difference between B's date under the old logic and under the new logic, with everything else held constant. C itself is charged nothing — C didn't move, the link moved. The charge is carried on B's row (the successor the link now governs), with the link itself named as the mechanism — this is the rule for every relationship charge in this catalogue.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | B +4 | – | – | – | – | – | – | **+4** |

---

## Group 2 – When the relationship type changes the story

Every relationship type has its own hand-over point — the moment an activity's influence ends and the baton passes downstream. This group is about following that hand-over point precisely: splitting a charge when logic only partly explains a movement, applying the driving test when a link isn't actually binding, and flagging concurrency honestly rather than forcing a single winner.

### S08 – Lag increased on an existing link

![Lag increased on an existing link](pages/images/delay-attribution-2/S08.png)

**What happens.** The A→B relationship stays the same type, but its lag is edited from 0 to +5 days. B is pushed 5 days with no movement in A at all.

**How it's charged.** **Relationship Change** against the A→B link. A lag edit is a logic edit — treated exactly like inserting or deleting a relationship, measured as the delta it produces.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | B +5 | – | – | – | – | – | – | **+5** |

### S09 – Relationship deleted

![Relationship deleted](pages/images/delay-attribution-2/S09.png)

**What happens.** The A→B link is removed. B falls back onto its remaining predecessor D and starts 3 days earlier than baseline.

**How it's charged.** **Relationship Change**, negative, against the deleted A→B link — carried on B's row, since A never moved and answers for nothing. (If deletion had left B with no predecessor at all, this escalates instead to Implied Logic — see [S19](#s19-implied-link) — because the schedule would no longer explain B's position at all.)

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | B −3 | – | – | – | – | – | – | **−3** |

### S10 – Start-to-start driving

![Start-to-start driving](pages/images/delay-attribution-2/S10.png)

**What happens.** A drives B through a start-to-start link with 2 days of lag. A's actual start slips 3 days, pushing B's start by 3 days too. A's own finish also blows out by 6 days — but nothing downstream depends on A's finish.

**How it's charged.** **Progress Change** against A, for 3 days only — not 6. Under a start-to-start link, A's sphere of influence is its *start*; once B's start is driven, the baton has passed. A's finish performance sits outside the window and charges zero, however bad it looks on the bar chart. This is the window-of-influence principle in its purest form.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | – | – | A +3 | – | – | – | – | **+3** |

### S11 – Finish-to-finish driving

![Finish-to-finish driving](pages/images/delay-attribution-2/S11.png)

**What happens.** A drives B's finish through a finish-to-finish link with 1 day of lag. A's duration extends 4 days, and B's finish is dragged 4 days with it.

**How it's charged.** **Duration Change** against A, transmitted through the FF link. The mirror image of S10: here A's window closes at its *finish*, and B's own start isn't part of the driven chain at all.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| A +4 | – | – | – | – | – | – | – | **+4** |

### S12 – Split attribution

![Finish-to-finish partially explains the movement](pages/images/delay-attribution-2/S12.png)

**What happens.** A→B is finish-to-finish with 5 days of lag. A's finish slips 2 days, dragging B's required finish 2 days later. But B's actual start is 5 days later than plan — the FF logic only explains 2 of those days. The remaining 3 have no logic explanation at all.

**How it's charged.** Split: 2 days to A's cause (here, Duration Change) transmitted through the link, and 3 days to **Progress Change** against B. This is the general rule for every partial explanation in the catalogue — charge the logic for exactly what it proves, and put the unexplained rest into Progress Change. No category ever absorbs movement it can't mathematically account for.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| A +2 | – | – | B +3 | – | – | – | – | **+5** |

### S13 – Concurrent drivers

![Concurrent drivers](pages/images/delay-attribution-2/S13.png)

**What happens.** A and C both finish 5 days late, and both drive B through finish-to-start links. On the recalculated dates, the two drivers tie exactly.

**How it's charged.** Each concurrent driver carries the **full** charge — A +5 *and* C +5, both flagged concurrent. At Practical Completion the movement is counted once: the two entries are alternative explanations of the same 5 days, not 10 days of delay, so a concurrent set is de-duplicated before the columns are summed. This matters for a real delay claim: recording the full charge against each driver preserves the fact that either one, on its own, fully explains the movement. Splitting the charge in half, or force-picking a winner, would manufacture a certainty the schedule data doesn't actually contain.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | – | – | A +5\* / C +5\* | – | – | – | – | **+5** |

*\* concurrent — the full +5 is recorded against each driver, counted once at PC, never summed.*

### S14 – Absorbed by float

![Slip absorbed by float](pages/images/delay-attribution-2/S14.png)

**What happens.** A slips 2 days, but B is actually driven by C — and A's late finish still lands inside its own float. B doesn't move. PC doesn't move.

**How it's charged.** A is charged **zero**. Only movement on the genuinely driving path is chargeable — the driving test runs at every step, and a relationship that isn't binding transmits nothing. This scenario is the guard rail against the single most common overclaim in delay analysis: treating every activity that ran late as if all of its lateness reached completion.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | – | – | – | – | – | – | – | **0** |

### S15 – Start-to-finish

![Start-to-finish](pages/images/delay-attribution-2/S15.png)

**What happens.** B→C is start-to-finish: C can't finish until B starts. B's start slips 5 days, holding C's finish 5 days later.

**How it's charged.** Charged to whatever caused B's start to move — here, **Progress Change** against B — transmitted through the SF link. Start-to-finish is rare, and often a sign the schedule logic could be modelled more cleanly, but the treatment still has to be defined: the hand-over point is predecessor start to successor finish, and attribution follows it exactly like any other link type.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | – | – | B +5 | – | – | – | – | **+5** |

---

## Group 3 – When the network itself changes

Fragnets, new activities, implied links, and resource levelling — the cases where the network has genuinely changed shape, or where the schedule no longer fully explains its own dates on its own. Insertions are always measured by their before/after effect on Practical Completion; implied links and levelling are always reported as candidates, never as proven causes.

### S16 – Fragnet inserted, net effect zero

![Fragnet inserted, net effect zero](pages/images/delay-attribution-2/S16.png)

**What happens.** An 8-day fragnet F is inserted between A and B. But B is held by a Start-No-Earlier-Than constraint and had never actually moved off its baseline date — the fragnet finishes comfortably inside B's existing wait. Practical Completion before the insertion equals Practical Completion after it.

**How it's charged.** **Zero.** The rule is strictly the before/after delta at Practical Completion: recalculate PC with the fragnet out, recalculate with it in, and charge the difference — never the fragnet's own length. B's successors are now held in place by different logic, but since they hadn't moved off baseline, the fragnet changed the *explanation* of the dates, not the dates themselves.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | – | – | – | – | F 0 | – | – | **0** |

### S17 – Fragnet inserted, partial push

![Fragnet inserted, partial push](pages/images/delay-attribution-2/S17.png)

**What happens.** The same 8-day fragnet is inserted, but this time B was already sitting at day 9, and the fragnet's day-12 finish pushes B by 3 days.

**How it's charged.** **Fragnet / New Activity**, +3 — not +8. Same before/after method as S16: the fragnet absorbs whatever float and waiting time already existed downstream, and only the residual movement at PC is charged to it. Charging fragnets at face-value length would systematically overstate their real effect.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | – | – | – | – | F +3 | – | – | **+3** |

### S18 – Single new activity inserted

![Single new activity inserted](pages/images/delay-attribution-2/S18.png)

**What happens.** One new 3-day activity N is inserted into the A→B chain. B is pushed 3 days.

**How it's charged.** Treated exactly like a fragnet — a fragnet of one. Charged to **Fragnet / New Activity** at the before/after delta at PC, which here happens to equal N's full duration only because there was no float available to absorb it.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | – | – | – | – | N +3 | – | – | **+3** |

### S19 – Implied link

![Implied link — the algorithm finds a driver with no relationship](pages/images/delay-attribution-2/S19.png)

**What happens.** B has moved 3 days, and its new start tracks D's finish exactly — but no relationship between D and B exists in the baseline *or* the current schedule. Logic+ surfaces D as the apparent driver anyway, from the pattern alone.

**How it's charged.** **Implied Logic**, explicitly labelled a candidate initiator — never reported as a proven cause. The date correlation is evidence of something real (a shared crew, shared site access, an instruction that never made it into the schedule) but it's correlation, not proof, and the language used in the tool and in any report has to keep that distinction honest. The charge quantifies what the implied link *would* explain if it turns out to be real; confirming it is a human step.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | – | – | – | B +3 | – | – | – | **+3** |

### S20 – Resource levelling

![Resource levelling — the candidate column](pages/images/delay-attribution-2/S20.png)

**What happens.** B is 5 days later than baseline. Its logic is fully satisfied, no constraint exists, durations are unchanged, and the dates aren't actualised late — every other category comes back empty. Resource levelling is switched on in the schedule, and B's resources were over-allocated in the window it vacated.

**How it's charged.** **Resource Levelling** — but only by elimination plus corroboration: attributed here when (a) no other category explains the movement, (b) levelling is switched on, *and* (c) a genuine resource conflict existed in the window B vacated. Without (b) and (c), the residual stays in Progress Change instead — an empty scorecard across the other seven categories is necessary but never sufficient on its own. Like Implied Logic, this is a candidate attribution pending confirmation against the levelling log, not a settled fact.

| Duration | Relationship | Constraint | Progress | Implied Logic | Fragnet/New | Levelling | Calendar | Δ PC |
|---|---|---|---|---|---|---|---|---|
| – | – | – | – | – | – | B +5 | – | **+5** |

---

## Summary: all 20 scenarios at a glance

| ID | Scenario | Charged to | Δ PC |
|---|---|---|---|
| S01 | Duration extension on the driving path | A | +5 |
| S02 | Duration reduction (mitigation) | A | −2 |
| S03 | Late start with logic satisfied | B | +5 |
| S04 | Slow progress after an on-time start | B | +5 |
| S05 | Constraint added | B | +5 |
| S06 | Constraint removed | B | −3 |
| S07 | New relationship inserted | B | +4 |
| S08 | Lag increased on an existing link | B | +5 |
| S09 | Relationship deleted | B | −3 |
| S10 | Start-to-start driving | A | +3 |
| S11 | Finish-to-finish driving | A | +4 |
| S12 | Split attribution (finish-to-finish, partial) | A / B | +5 |
| S13 | Concurrent drivers | A & C | +5 |
| S14 | Absorbed by float | – | 0 |
| S15 | Start-to-finish | B | +5 |
| S16 | Fragnet inserted, net effect zero | F | 0 |
| S17 | Fragnet inserted, partial push | F | +3 |
| S18 | Single new activity inserted | N | +3 |
| S19 | Implied link | B | +3 |
| S20 | Resource levelling | B | +5 |

---

## The complete catalogue: 58 situations, five families

The 20 scenarios above are the foundation — every later scenario in the full catalogue is one of these seven core ideas, made more specific. The complete specification covers **58 discrete situations** in total, grouped into five families. Twenty-one are already covered above; the rest are set out here for reference, each with the same diagram-plus-explanation treatment, organised by family so you can go as deep as you need to.

Each section below is collapsed by default — expand the ones you need.

<details>
<summary><strong>Family A — What changed (34 situations)</strong></summary>

Every discrete way a schedule can actually differ from its baseline, in both directions — the raw material every other family builds on. Each situation has exactly one home category, and gains are always recorded as a negative charge so the columns reconcile properly to the real movement at PC.

![Family A — what changed](pages/images/delay-attribution-2/A.png)

| ID | Situation | Treatment | Builds on |
|---|---|---|---|
| A-01 | Original duration increased | Full recalculated delta to Duration Change | S01 |
| A-02 | Original duration decreased | Negative Duration Change; gains recorded explicitly | S02 |
| A-03 | Remaining duration increased (work in progress) | Duration Change on the remaining portion only — the actualised portion is untouched | S01 |
| A-04 | Remaining duration decreased | Negative Duration Change on the remaining portion | S02 |
| A-05 | Relationship added, driving | Relationship Change at the recalculated delta | S07 |
| A-06 | Relationship added, non-driving | Recorded against the link, charged zero (driving test) | S14 |
| A-07 | Relationship deleted, successor re-driven earlier | Negative Relationship Change | S09 |
| A-08 | Relationship deleted, no driver left | Escalates to Implied Logic — the schedule no longer explains the successor's position | S09 → S19 |
| A-09 | Lag increased on an existing link | Relationship Change | S08 |
| A-10 | Lag decreased | Negative Relationship Change | S08 (mirror) |
| A-11 | Relationship type changed (e.g. FS → SS) | Treated as delete-plus-add; net recalculated delta | S07 + S09 |
| A-12 | Start-side constraint added or pushed later | Constraint Change | S05 |
| A-13 | Start-side constraint removed or relaxed | Negative Constraint Change | S06 |
| A-14 | Finish-side constraint added or tightened | May create negative float instead of moving dates; only actual PC movement is charged | S05 |
| A-15 | Finish-side constraint removed | Negative Constraint Change where PC moves — mirror of A-14 | S06 |
| A-16 | Mandatory constraint added | Constraint Change; overrides logic in both directions — suppressed logic is flagged, not charged | S05 |
| A-17 | Mandatory constraint removed | Constraint Change at the recalculated delta — mirror of A-16 | S06 |
| A-18 | Late actual/forecast start, logic satisfied | Progress Change | S03 |
| A-19 | Early actual/forecast start | Negative Progress Change | S03 (mirror) |
| A-20 | Slow progress — actual duration longer than planned | Progress Change | S04 |
| A-21 | Fast progress | Negative Progress Change | S04 (mirror) |
| A-22 | Suspension/resume recorded on an activity | Progress Change for the suspended span | S04 |
| A-23 | Out-of-sequence progress | Progress Change; recalculated dates depend on the scheduling option in force | – |
| A-24 | Fragnet inserted, net zero at PC | Zero — before/after delta at PC | S16 |
| A-25 | Fragnet or chain inserted, push at PC | Fragnet Delay at the before/after delta, never face-value length | S17 |
| A-26 | Single new activity inserted | Treated as A-25 — a fragnet of one | S18 |
| A-27 | Activity deleted from the network | Negative Fragnet/New Activity at the before/after delta | S16 (mirror) |
| A-28 | Logic rewired around a WBS restructure | Net of the individual link edits, to Relationship Change | S07 + S09 |
| A-29 | Implied link, delay | Implied Logic — candidate initiator | S19 |
| A-30 | Implied link, gain | Negative Implied Logic, same candidate discipline | S19 (mirror) |
| A-31 | Resource levelling push | Resource Levelling, after the three-part test | S20 |
| A-32 | Resource levelling release | Negative Resource Levelling | S20 (mirror) |
| A-33 | Activity calendar reassigned (e.g. 5-day → 7-day) | Calendar Change at the recalculated delta | – |
| A-34 | Calendar body edited (holidays, exceptions) | Calendar Change; affects every activity on that calendar, charged via the driving path | – |

A-01, 02, 05, 07, 09, 12, 13, 18, 20, 24, 25, 26, 29 and 31 are exactly the base scenarios diagrammed above. The rest, below:

**A-03 — Remaining duration increased.** B started on time and was actualised to the data date; its remaining duration was then edited from 2 to 7 days. The edit is a plan change, so it charges Duration — not Progress — even though the bar is already underway.

![A-03](pages/images/delay-attribution-2/A-03.png)

**A-04 — Remaining duration decreased.** The same shape, opposite sign: remaining work cut from 4 to 2 days.

![A-04](pages/images/delay-attribution-2/A-04.png)

**A-06 — Relationship added, non-driving.** A link was added from C, but C finishes inside B's float. The schedule gained logic, not delay — recorded, but charged zero.

![A-06](pages/images/delay-attribution-2/A-06.png)

**A-08 — Relationship deleted, no driver left.** The only link into B was deleted. Nothing in the schedule now explains B's position, so the case escalates: Logic+ surfaces D as the candidate driver and the charge lands in Implied Logic.

![A-08](pages/images/delay-attribution-2/A-08.png)

**A-10 — Lag decreased.** The lag on A→B was cut from 3 days to 0, and B pulls forward — the gain twin of S08.

![A-10](pages/images/delay-attribution-2/A-10.png)

**A-11 — Relationship type changed.** The relationship was edited from finish-to-start to start-to-start+1. Measured as a delete plus an add, charged at the net.

![A-11](pages/images/delay-attribution-2/A-11.png)

**A-14 — Finish-side constraint added.** A Finish-No-Later-Than constraint was added. Dates don't actually move — total float goes negative instead. The pressure is flagged, but since nothing moved, the PC charge is zero.

![A-14](pages/images/delay-attribution-2/A-14.png)

**A-16 — Mandatory constraint added.** A Mandatory Start pins B to day 9 regardless of what the logic wants. The suppressed link is shown dotted, so the reader knows logic was overridden — but it isn't charged for it.

![A-16](pages/images/delay-attribution-2/A-16.png)

**A-19 — Early actual or forecast start.** B started 2 days earlier than planned with its logic satisfied — the gain twin of a late start, charged negative to Progress.

![A-19](pages/images/delay-attribution-2/A-19.png)

**A-21 — Fast progress.** Started on time and finished in half the planned time. Negative Progress Change.

![A-21](pages/images/delay-attribution-2/A-21.png)

**A-22 — Suspension and resume.** B worked 2 days, stopped for 3, then resumed. The suspension stretches the bar; the suspended span is Progress Change.

![A-22](pages/images/delay-attribution-2/A-22.png)

**A-23 — Out-of-sequence progress.** B started before A finished — the work on site got ahead of the logic. The overridden link is shown dotted; the recalculated effect depends on which scheduling option is in force (see P-02 and P-03 in the Family P section below).

![A-23](pages/images/delay-attribution-2/A-23.png)

**A-27 — Activity deleted.** B was deleted outright — a yellow baseline bar with no blue bar above it. Its successor rewires and pulls forward; the credit is carried against the deleted activity at the before/after delta.

![A-27](pages/images/delay-attribution-2/A-27.png)

**A-28 — Logic rewired in a WBS restructure.** One link deleted, another added, in a single restructure. The charge is the net of the two edits, carried on B with both links named.

![A-28](pages/images/delay-attribution-2/A-28.png)

**A-30 — Implied link, gain.** B sits 3 days earlier than anything in the schedule explains, tracking D's finish exactly — the gain twin of S19, with the same candidate-initiator discipline.

![A-30](pages/images/delay-attribution-2/A-30.png)

**A-32 — Resource levelling release.** The conflict that held B late in the baseline was resolved, and levelling releases B early. Negative Resource Levelling, subject to the same three-part test as S20.

![A-32](pages/images/delay-attribution-2/A-32.png)

**A-33 — Activity calendar reassigned.** B moved from a 5-day to a 7-day calendar — identical work content, two fewer elapsed days. The core case for the Calendar Change category.

![A-33](pages/images/delay-attribution-2/A-33.png)

**A-34 — Calendar body edited.** Two holidays were added to B's calendar. No edit to the activity itself, but two more elapsed days — and every other activity sharing that calendar is affected the same way.

![A-34](pages/images/delay-attribution-2/A-34.png)

</details>

<details>
<summary><strong>Family T — How delay travels to completion (12 situations)</strong></summary>

A cause and its journey to Practical Completion are two separate questions, answered one after the other. Delay moves along the driving path; where the hand-over happens depends on the relationship type; and the charge always arrives at PC still named to wherever it started.

![Family T — how delay travels to completion](pages/images/delay-attribution-2/T.png)

| ID | Situation | Treatment | Builds on |
|---|---|---|---|
| T-01 | Finish-to-start driving | Hand-over at the successor's start | S01 |
| T-02 | Start-to-start driving | Window closes at the start; predecessor's finish performance is charged zero | S10 |
| T-03 | Finish-to-finish driving | Hand-over at the finish; successor's start is outside the driven chain | S11 |
| T-04 | Start-to-finish driving | Predecessor start to successor finish; rare but defined | S15 |
| T-05 | Partial explanation | Split: logic charged what it proves, residual to Progress Change | S12 |
| T-06 | Non-driving link, float absorption | Zero charge — only the binding path transmits | S14 |
| T-07 | Concurrent drivers, exact tie | Full charge against each driver; counted once at PC | S13 |
| T-08 | Near-concurrency within a threshold | Flagged as near-concurrent, so a small recalculation difference can't silently flip the story | – |
| T-09 | Multi-hop relay through several activities | The charge stays with the originating cause, however many activities it passes through | S01 |
| T-10 | Redundant parallel links to one successor | The binding link is identified; the others are recorded as non-driving | S14 |
| T-11 | Driving path switches mid-window | Sequential split — each cause charged for the sub-period it actually drove | S12 |
| T-12 | Gain blocked by a secondary path | Mitigation charged only for the PC movement actually achieved | S02 |

T-01 to T-07 are exactly the base scenarios above (S01, S10, S11, S15, S12, S14, S13). The rest, below:

**T-08 — Near-concurrency.** C finishes one day behind driver A. On the arithmetic, A drives — in practice, that's close enough to be a coin toss. The near-concurrency flag preserves how close it was, so a one-day recalculation difference can't silently flip the story.

![T-08](pages/images/delay-attribution-2/T-08.png)

**T-09 — Multi-hop relay.** A's 3-day extension passes through B and C untouched — they both move, but the charge doesn't; it arrives at PC still named to A. This is what keeps a per-activity total meaningful even across long chains.

![T-09](pages/images/delay-attribution-2/T-09.png)

**T-10 — Redundant parallel links.** Two links reach B, but only A's link actually binds. The redundant one is recorded, so the network history stays complete, but charged nothing.

![T-10](pages/images/delay-attribution-2/T-10.png)

**T-11 — Driving path switches mid-window.** A drove the first 2 days of B's movement; then a new constraint took over and drove 3 more. One window, two drivers in sequence — the charge splits by sub-period rather than forcing a single winner.

![T-11](pages/images/delay-attribution-2/T-11.png)

**T-12 — Gain blocked by a secondary path.** A gave back 3 days of duration, but C never moved and remains the driver — the gain never actually reached Practical Completion. Mitigation is only credited for the completion movement it genuinely achieved, which here is zero.

![T-12](pages/images/delay-attribution-2/T-12.png)

</details>

<details>
<summary><strong>Family P — Work already underway (5 situations)</strong></summary>

Once a bar is genuinely in progress, the same edit can mean two different things — and the data date is the border between them. These five are the cases where the state of the work itself changes the reading, rather than combining with something else by rule.

![Family P — work already underway](pages/images/delay-attribution-2/P.png)

| ID | Situation | Treatment | Builds on |
|---|---|---|---|
| P-01 | In-progress bar: actual start plus revised remaining | One bar splits two ways — the actualised portion is Progress Change, the remaining-duration edit is Duration Change | S01 vs S04 |
| P-02 | Retained logic vs. progress override | The scheduling option changes the recalculated dates themselves; the option in force is recorded with every charge | – |
| P-03 | Out-of-sequence progress | Successor started before its logic was satisfied; treatment depends on the scheduling option, charged to Progress Change | – |
| P-04 | Completed activity, later logic edits | The window has closed — edits to logic on completed work charge zero at PC | – |
| P-05 | Prospective symmetry | Identical treatments apply to forecast bars; the language is always "forecast," never "as-built" | – |

**P-01 — Actual start plus revised remaining.** One bar, two territories: the 2-day late start is fact and charges Progress; the remaining-duration edit from 2 to 5 days is a plan change and charges Duration. The data date is the border between the two.

![P-01](pages/images/delay-attribution-2/P-01.png)

**P-02 — Retained logic vs. progress override.** B started out of sequence while A still had work outstanding. "Retained logic" and "progress override" are two different scheduling options that forecast different dates from identical facts — so which option was in force always travels with the charge, or two runs of the tool could disagree with no visible reason why.

![P-02](pages/images/delay-attribution-2/P-02.png)

**P-03 — Out-of-sequence progress.** The work on site got ahead of the logic. The gain is Progress Change; the overridden link is shown dotted; the recalculation follows P-02's rule.

![P-03](pages/images/delay-attribution-2/P-03.png)

**P-04 — Completed activity, later logic edits.** Both activities are complete. A lag edit on their link changes nothing — actualised dates can't move, the window has already closed, and the edit charges zero.

![P-04](pages/images/delay-attribution-2/P-04.png)

**P-05 — Prospective symmetry.** The whole network sits ahead of the data date — every bar shown is a forecast. The treatment is identical to S01; only the word changes, from "as-built movement" to "forecast movement."

![P-05](pages/images/delay-attribution-2/P-05.png)

</details>

<details>
<summary><strong>Family K — Counting the days (3 situations)</strong></summary>

The same delay can be a different number on a different calendar — so before any charge is stated in days, Logic+ has to know *whose* days it's counting. Three rules settle it.

![Family K — counting the days](pages/images/delay-attribution-2/K.png)

| ID | Situation | Treatment | Builds on |
|---|---|---|---|
| K-01 | Cross-calendar day-count translation | A delta crossing calendars is counted in the successor's calendar, from the hand-over point onward | – |
| K-02 | PC milestone calendar | Movement at PC is always measured on the PC milestone's own calendar | – |
| K-03 | Calendar vs. levelling — telling them apart | Both can move dates with every other category silent. The tiebreak: a calendar delta reproduces exactly from the calendar diff; levelling needs a corroborating resource conflict too | S20 |

**K-01 — Cross-calendar translation.** A runs on a 5-day calendar, B on a 7-day one. A's slip is handed over at the link and counted in B's days from that point forward — the delta changes denomination right at the hand-over.

![K-01](pages/images/delay-attribution-2/K-01.png)

**K-02 — The PC milestone's own calendar.** Activity days and milestone days can genuinely differ. Whatever calendars the delay crossed on its way through the schedule, the number reported at PC is always counted on the PC milestone's own calendar.

![K-02](pages/images/delay-attribution-2/K-02.png)

**K-03 — Calendar vs. levelling, the fingerprint test.** B moved 2 days with every category silent — the exact signature both a calendar change and resource levelling can produce. The tiebreak: rerun the delta against the calendar diff. If it reproduces the movement exactly, it's a Calendar Change; levelling additionally needs a corroborating resource conflict in the window B vacated.

![K-03](pages/images/delay-attribution-2/K-03.png)

</details>

<details>
<summary><strong>Family X — Several changes at once (4 situations)</strong></summary>

Two or more causes hitting the same activity inside one reporting window. The answer here isn't a diagram for every possible combination — it's a fixed order of extraction: peel the causes off one at a time in the same sequence every time, and require the pieces to add back exactly to the real movement at PC.

![Family X — several changes at once](pages/images/delay-attribution-2/X.png)

| ID | Situation | Treatment | Builds on |
|---|---|---|---|
| X-01 | Constraint plus duration change, same window | Extract the constraint effect first, then the duration effect — the residual must reconcile | – |
| X-02 | Logic edit plus progress, same window | Logic extracted first; the unexplained residual goes to Progress Change | – |
| X-03 | Fragnet insertion plus successor progress, same window | The before/after delta is computed with the progress movement isolated, so the fragnet isn't charged for performance it didn't cause | S16/S17 |
| X-04 | The general ordering rule | Constraints → logic → durations → progress residual, every time; the signed sum of all four equals the movement at PC | – |

**X-01 — Constraint plus duration.** A new constraint and a duration edit landed in the same reporting window. The constraint is peeled off first (+3), the duration second (+2), and the two pieces reconcile exactly to the +5 seen at PC.

![X-01](pages/images/delay-attribution-2/X-01.png)

**X-02 — Logic edit plus progress.** A lag edit explains 2 of the 5 days that moved; the remaining 3 have no logic explanation and go to Progress. Logic is always extracted first, and the residual is never allowed to hide inside a logic charge.

![X-02](pages/images/delay-attribution-2/X-02.png)

**X-03 — Fragnet plus successor progress.** A fragnet went into the schedule while B also ran slow in the same window. The insertion's before/after delta is computed with B's progress isolated out (+3), so the fragnet isn't wrongly charged for B's own performance (+2). Two causes, two owners, one total that still reconciles.

![X-03](pages/images/delay-attribution-2/X-03.png)

**X-04 — The general ordering rule.** The rule underneath this whole family: constraints, then logic, then durations, then whatever's left goes to Progress — with the signed pieces always summing exactly to the real movement at PC.

</details>
