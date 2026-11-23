---
page-id: causes-of-change
route: /module/6/project/:projectId
title: Causes of Change
audience: external
status: draft
version: 1.2.0
last-reviewed: 2026-09-08
blocked-reason: Module 6 is now built (confirmed against current codebase — frontend/src/components/modules/CausesOfChangeModule/, frontend/src/analytics/causesOfChange.ts, with an in-repo README documenting the same calculation). Content below is written from that code and README, but has not yet been checked against the running app — confirm on-screen wording, Gantt rendering, and CSV export before promoting to complete.
---

## Overview

When a group of activities moves later together between schedule updates, something upstream pushed them. A planner often has a hunch about the cause, such as a permit delay, a subcontractor issue or a late decision, but no systematic way to test it or to tell a real trigger event apart from routine monthly re-baselining.

Causes of Change does that search. For every pair of consecutive schedule updates, it finds groups of logically connected activities whose dates moved later by the same number of days, and identifies the upstream activity in that group's schedule logic as the candidate root cause. It also compares the group's movement with the update's typical (background) movement, so a distinctive shift stands out from routine changes.

## Report details

Each group found appears as an **event**: an expandable row giving the update date, the group's WBS category (or categories, if it spans more than one), how many activities moved, and by how many days. Collapsed, each event also shows:

- a **drift badge**: either "≈ N-day typical delay" (routine, in line with the rest of that update) or "+N days beyond M-day typical delay" (standout, when the group's movement exceeds the update's typical movement by 25 days or more);
- a **root-cause summary**: "Candidate root: *(activity)* → N downstream activities", "Multiple possible roots · N upstream activities", or "Root unresolved · logic cycle", each flagged when one or more root candidates start exactly on the update's data date;
- a **constraint-evidence summary**, when any constraints changed: how many date or type constraints changed among the group's activities, and how many of those changes line up with the movement.

Expanding an event opens a timeline comparing the previous update with the current one (faint bars for the previous update, solid bars for the current one), with the root candidates listed first. It includes:

- a **root-cause callout** that states the candidate root cause, or the ambiguous or cycle state, in full;
- a **constraint-evidence callout** with the detail behind the summary badge;
- constraint markers on each activity's current-update bar: a filled diamond for the primary constraint and an outlined one for the secondary, coloured black when unchanged, yellow when changed and red when changed in line with the activity's own movement, with grey marking a removed constraint's previous position;
- relationship arrows between connected activities, on by default, with a toggle to hide them and a second toggle to show or hide the `FS`/`SS`/`FF`/`SF` label on each arrow;
- an **Export event CSV** button for that event, and an **Export all CSV** button next to the filters, which exports every event found, including any hidden by the filters.

Above the event list, three controls filter and order the events: **Minimum change days** (defaults to the most recent update's typical delay), **Minimum activities affected** (defaults to 2) and **Order events** (defaults to the built-in ranking; the alternatives are start date, activity-days impact, delay duration and activity count). A **Show** selector chooses which reference lines appear on the expanded timeline: data date, baseline (previous-update) dates, both or neither.

## Calculation and other logic

**Data used:** every uploaded schedule update, with each activity's best available start and finish dates, its WBS category (the schedule's highest-level WBS breakdown, worked out the same way as in Critical Path Evolution's WBS timeline), its predecessor and successor relationships and their types, and its primary and secondary constraint types and dates.

**How groups are found:** Logic+ compares consecutive schedule updates pair by pair, matching activities by activity ID. For each matched activity, it checks whether the start or the finish moved to a later calendar day, ignoring earlier moves and moves that cancel out. A whole-activity shift, a start-only shift with a shorter duration, and a finish-only shift are each recorded separately. Two moved activities join the same group only when the pair of dates their relationship connects moved later by the *same* number of days. A finish-to-start (FS) link compares the predecessor's finish shift with the successor's start shift, SS compares start with start, FF finish with finish, and SF start with finish. A group can extend along a chain of such links and can cross WBS categories, but two activities that moved by the same amount with no matching logic path between them are **not** grouped. A group needs at least two activities linked in this way.

**How the root cause is identified:** within a group, the upstream candidates are the activities with no incoming matching-movement link from another member. If there's exactly one candidate and it reaches every other member through the chain, it's shown as the **identified** candidate root cause, with the rest as its downstream effects. More than one candidate makes the group **ambiguous**. If there's no candidate, because every member has an incoming link from another member, the group's logic forms a **cycle** and the root is **unresolved**. This is read from the direction of the schedule's own predecessor and successor links, and it doesn't prove what caused the movement in the real world.

**How "typical" and "standout" are judged:** for each update, the *typical* (background) delay is the median movement of every activity that moved later in that update, not only the group's members. Each activity's movement is the larger of its start shift and finish shift, rounded to whole days:

> Typical (background) delay for an update = median, across every matched activity that moved later, of (the larger of its start-shift or finish-shift in days), rounded to the nearest whole day

A group's *excess* delay is its own shared movement minus that typical figure:

> Group's excess delay = the group's own shared movement (days) − that update's typical delay

Groups with 25 days or more of excess delay rank ahead of all others and get the standout drift badge. After that, the ranking follows activity-day impact, then excess delay, then group size, then how recent the update is:

> Activity-day impact = number of activities in the group × the group's shared movement (days)

In full, the order is: standout groups (excess delay ≥ 25 days) first, then highest activity-day impact, then highest excess delay, then largest group, then most recent update.

**Constraint evidence:** for each activity in a group, the primary and secondary constraints are compared with the previous update. Moving a constraint between primary and secondary with no other change is ignored. A changed constraint counts as *supporting* the movement only when all of these are true: it's a start or finish exact-date or lower-bound type; the activity's relevant start or finish actually moved later; the constraint was added, changed or moved later; and that start or finish lands within one day of the constraint date. Supporting evidence of this kind doesn't prove the constraint caused the movement.

## Important

The candidate root cause comes only from the direction of the schedule's logic and from matching date movements. It **isn't proof** of the real-world event that made the first activity move. A permit delay, subcontractor issue or late decision still has to be confirmed by the planner: Logic+ narrows down where in the schedule to look and gives a starting point for that investigation, not a conclusion. Keep this in mind wherever the analysis might be used in a contractual or dispute setting; the module's own on-screen caveat says the same.
