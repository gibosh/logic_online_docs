---
page-id: causes-of-change
route: /module/6/project/:projectId
title: Causes of Change
audience: external
status: draft
version: 1.0.1
last-reviewed: 2026-09-03
blocked-reason: Module 6 is now built (confirmed against current codebase — frontend/src/components/modules/CausesOfChangeModule/, frontend/src/analytics/causesOfChange.ts, with an in-repo README documenting the same calculation). Content below is written from that code and README, but has not yet been checked against the running app — confirm on-screen wording, Gantt rendering, and CSV export before promoting to complete.
---

## Causes of Change – Overview

When a group of activities suddenly moves later together between schedule updates, something happened upstream that pushed them. A planner often has a hunch about what it was – a permit delay, a subcontractor issue, a late decision – but no systematic way to test it, and no way to separate a genuine trigger event from ordinary monthly re-baselining noise.

Causes of Change automates that search: for every pair of consecutive schedule updates, it finds groups of activities whose logically-connected dates moved later by the same number of days, identifies the upstream activity in that group's own schedule logic as the candidate root cause, and shows how large the group's movement was compared to the update's own typical (background) movement. A shift that is genuinely distinctive stands out from routine admin clearly.

## Report details

Each detected group is shown as an **event**: an expandable row giving the update date, the group's WBS category (or categories, if the group spans more than one), how many activities moved, and by how many days. Collapsed, each event also shows:

- a **drift badge** – either "≈ N-day typical delay" (routine, in line with the rest of that update) or "+N days beyond M-day typical delay" (standout, when the group's movement exceeds the update's typical movement by 25 days or more);
- a **root-cause summary** – "Candidate root: *(activity)* → N downstream activities", "Multiple possible roots · N upstream activities", or "Root unresolved · logic cycle", each optionally flagged when one or more root candidates start exactly on the update's data date;
- a **constraint-evidence summary** – how many date/type constraints changed among the group's activities, and how many of those changes align with the movement, when any changed.

Expanding an event opens a previous-update-versus-current-update timeline (ghost bars = previous update, solid bars = current update), root candidates listed first, with:

- a **root-cause callout** spelling out the candidate root cause (or the ambiguous/cycle state) in full;
- a **constraint-evidence callout** with the supporting detail behind the summary badge;
- constraint markers plotted on each activity's current-update bar – filled diamond = primary constraint, outlined = secondary, black = unchanged, yellow = changed, red = changed and aligned with the activity's own movement, grey = a removed constraint's previous position;
- relationship arrows between connected activities (toggle on/off; a second toggle shows/hides the `FS`/`SS`/`FF`/`SF` label on each arrow), on by default;
- an **Export event CSV** button for that one expanded event, and an **Export all CSV** button (module-level, next to the filters) that exports every detected event including ones currently hidden by filters.

Above the event list, three controls filter and reorder what's shown: **Minimum change days** (defaults to the most recent update's typical delay), **Minimum activities affected** (defaults to 2), and **Order events** (defaults to the built-in ranking; alternatives are start date, activity-days impact, delay duration, or activity count). A **Show** selector controls which reference lines appear on the expanded timeline: data date, baseline (previous-update) dates, both, or neither.

## Calculation and other logic

**Data used:** every uploaded schedule update, each activity's best-available start/finish dates, its WBS category (the schedule's own highest-level WBS breakdown, same resolver as Critical Path Evolution's WBS timeline), its defined predecessor/successor relationships (including type), and its primary/secondary constraint type and date.

**How groups are found:** consecutive schedule updates are compared pair by pair, matching activities by activity ID. For each matched activity, Logic+ checks whether its start or its finish moved to a later calendar day (not earlier, and not both directions cancelling out — a whole-activity shift, a start-only shift with duration compression, and a finish-only shift are all captured separately). Two moved activities are then linked into the same group only when the specific pair of dates their schedule relationship connects moved later by the *same* number of days: a finish-to-start (FS) link compares the predecessor's finish shift to the successor's start shift, SS compares start-to-start, FF finish-to-finish, and SF start-to-finish. A group can extend through a chain of such matching links and can cross WBS categories; two activities that moved by the same amount but have no matching logic path between them are **not** grouped. At least two relationship-aligned activities are required to form a group.

**How the root cause is identified:** within a group, the activity (or activities) with no incoming matching-movement link from another group member are the upstream candidates. Exactly one such candidate that reaches every other member through the chain is shown as the **identified** candidate root cause, with the rest as its downstream effects. More than one such candidate makes the group **ambiguous**. No candidate at all (every member has an incoming link from another member) means the group's logic forms a **cycle**, and the root is left **unresolved**. This is read directly off the schedule's own predecessor/successor direction — it is not a proof of external causation.

**How "typical" and "standout" are judged:** for each update, the *typical* (background) delay is the median of every matched activity's largest date movement that update — the ordinary month-to-month churn, not just this group's. A group's *excess* delay is its own movement minus that typical figure; groups at 25 days or more excess are ranked ahead of everything else and flagged as standout by the drift badge. Ranking after that follows activity-day impact (activities affected × days moved), then excess delay, then group size, then update recency.

**Constraint evidence:** for every activity in a group, its primary and secondary constraint slots are compared with the preceding update (moving a constraint between primary and secondary without other change is ignored). A changed constraint is marked as *supporting* the movement only when it is a start/finish exact-date or lower-bound type, the relevant activity boundary actually moved later, the constraint was added/changed/moved later, and the boundary lands within one day of the constraint date. This is supporting schedule evidence for the movement, not proof the constraint was the binding cause.

## Important

The candidate root cause is inferred purely from the schedule's own recorded logic direction and matching date movement — it is **not** proof of the real-world event that caused the first activity to move. A permit delay, a subcontractor issue, or a late decision still has to be confirmed by the planner; Logic+ narrows down where in the schedule to look, and gives a starting point for that investigation rather than a conclusion. This distinction matters in any context where delay analysis may be used in a contractual or dispute setting — the module's own on-screen caveat makes it explicit.
