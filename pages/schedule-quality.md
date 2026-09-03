---
page-id: schedule-quality
route: /module/3/project/:projectId
title: DCMA 14-Point Check
audience: external
status: complete
version: 1.2.0
last-reviewed: 2026-09-03
---

## DCMA 14-Point Check – Overview

Before any forecast or delay analysis from a schedule can be trusted, the schedule itself needs to be well-built enough to produce reliable results. Schedule Quality runs an automated health check against every schedule you have uploaded for a project and tells you, in plain pass/fail terms, whether each one meets an industry-standard quality threshold.

A planner or client can run the same industry checks manually in P6 – and if they find a quality problem before you do, that is a credibility risk in a meeting. Automating the check means every analysis you run starts from a current, documented, defensible quality position, updated every time a new schedule is uploaded.

---

## Quality profiles

Select a profile from the dropdown to apply a different standard to the check. The profile applies to all schedules shown on the page. A **"View rules (PDF)"** link sits next to the dropdown for the DCMA and CIOB profiles, opening the source standard document the checks are built from (there is no equivalent PDF for the Best Practice profile, since it isn't drawn from a single published standard).

| Profile (dropdown label) | Standard | Pass threshold |
|---------|----------|----------------|
| DCMA 14-Point Assessment | DCMA-PAM-200-1 section 4 | All 14 checks must pass |
| CIOB PP21 – Standard Projects | CIOB Planning Protocol 2021 | 12 of 15 checks must pass |
| CIOB PP21 – Major Projects | CIOB Planning Protocol 2021 | All 15 checks must pass |
| Additional Industry Best Practise Checks | Supplementary checks | Both checks must pass |

The default profile is **DCMA 14-Point Assessment**. Change the profile to apply a different standard – for example, CIOB PP21 Major Projects for large infrastructure contracts where the full 15-check standard is expected.

Note the exact wording differs slightly by profile and by where it's shown: the two CIOB entries appear in the dropdown as above, but the results panel header for them reads **"CIOB PP21 Stress Test (Standard Projects)"** / **"...(Major Projects)"**. The fourth profile is labelled **"Additional Industry Best Practise Checks"** everywhere in the app (matching the spelling above, not "Practice").

---

## Schedule list

The left panel lists all schedules uploaded for the project, one row per schedule, ordered by data date (oldest first). Each row shows:

- **Data date** of the schedule
- **Schedule** – the schedule's own identifier
- **Score** – checks passed out of checks applicable for the selected profile (e.g. `12/14`)
- **Result** – a green **Pass** or red **Fail** badge for the selected profile, plus a small info icon if any check on that schedule carries an explanatory note (see Proxy and fallback notes, below) – hover it to read the note without expanding anything

The most recently uploaded schedule is selected by default. Click a row to load that schedule's full check results in the detail panel on the right.

---

## Check detail

The right panel shows all checks run against the selected schedule, one row per check.

| Column | Description |
|--------|-------------|
| # | Check number within the profile |
| Check | Name of the quality check |
| Result | Metric value for this schedule (e.g. percentage of activities with no logic) |
| Target | The pass/fail threshold for this profile |
| Count | The raw numerator / denominator behind the Result percentage (e.g. `3/57`) |
| Status | Green **Pass** or red **Fail** badge, or **N/A** if the check does not apply |

**Click any check row** to expand it. The expanded view shows the check's full description, an **Export CSV** button for that check alone, any proxy or fallback note that applies (see below), and then the offending items – split into separate **Activities** and **Relationships** tables where relevant, since the two carry different columns (activities: project/activity ID, name, detail, unit; relationships: predecessor and successor project/ID/name, plus detail and unit). Up to 50 offending items are shown per table, with a "+N more" row if there are more. Use this list to identify exactly what needs correcting in P6 before re-uploading.

**N/A** means the check is not applicable for the selected profile (for example, some CIOB PP21 Standard checks are omitted relative to the Major Projects profile) or, for a couple of DCMA checks, that the schedule doesn't carry what the check needs (no baseline, or not resource-loaded).

---

## Assessment Scope

A separate panel, between the schedule list and the check detail, shown whenever a schedule is selected. Where the check detail panel tells you the *result*, Assessment Scope tells you exactly *what population of activities and relationships fed that result* – useful when a check's number looks surprising and you want to confirm nothing was wrongly included or excluded.

It has three levels, each exportable to CSV on its own:

1. **Base populations** – a top-level table (e.g. "Schedule rows," "Relationships," and profile-specific populations such as "Incomplete activities" or "Critical-path activities") showing how many items were Included, how many Excluded, and which check numbers draw on that population.
2. **Expand a population** to see a category breakdown – for "Schedule rows," for example, how many fall into Activities / Milestones / Level of Effort / Summary-WBS rows, and whether each category is Included or Excluded from that population.
3. **Expand a category** to see the individual activities or relationships in it, each tagged with a Pass / Fail / N/A / Excluded badge per check number that uses it – letting you trace a single activity through to exactly which checks it was scored against and how it did.

---

## What the checks cover

The DCMA 14-Point Assessment covers:

1. Logic – activities missing a predecessor or successor, threshold ≤ 5%
2. Leads – negative-lag relationships, threshold 0% (none allowed)
3. Lags – positive-lag relationships, threshold ≤ 5%
4. Relationship types – Finish-to-Start relationships, threshold ≥ 90% of all relationships
5. Hard constraints – activities with a hard date constraint applied, threshold ≤ 5%
6. High float – activities with total float over 44 working days, threshold ≤ 5%
7. Negative float – activities with negative total float, threshold 0% (none allowed)
8. High duration – activities with duration over 44 days against the baseline, threshold ≤ 5%
9. Invalid dates – actual dates after the status date, or (for activities that haven't started/finished yet) forecast dates before the status date, threshold 0% (none allowed)
10. Resources – activities with a duration but no resource assigned, threshold 0% (none allowed; not applicable if the schedule is not resource-loaded)
11. Missed tasks – activities baselined to have started or finished by the status date that have not, threshold ≤ 5% (not applicable without a baseline)
12. Critical path test – unbroken chain of critical-flagged activities reaching the completion milestone (a proxy for the full DCMA test, which requires an intentional-delay reschedule)
13. Critical path length index (CPLI) – (critical path length + total float) ÷ critical path length, pass at ≥ 0.95
14. Baseline execution index (BEI) – completed activities ÷ activities due to have completed by the status date, pass at ≥ 0.95

Most of these checks are measured against **incomplete activities only**, excluding WBS summary rows, Level-of-Effort activities, **and milestones**. Two checks are the exception: Invalid Dates (9) and Missed Tasks (11) are measured against the full activity population, completed activities included. The three relationship-based checks (Leads, Lags, Relationship types) aren't measured against activities at all – their denominator is the total relationship count.

The CIOB PP21 profiles apply a related but distinct set of 15 checks aligned to the CIOB Planning Protocol 2021 – largely mirroring the DCMA checks above, plus four unique to CIOB: named key-date milestones forecast past their target date, unresolvable calendar references, duplicate activity IDs or names, and whether the schedule has actually been rescheduled to the status date. CIOB's activity population deliberately **keeps milestones** (DCMA's excludes them), since CIOB's float and duration stress tests are meant to apply to them too. Most CIOB thresholds are stricter than their DCMA equivalent – for example, CIOB's float and duration checks allow 0% over the 44-day mark, where DCMA allows up to 5% – and CIOB's own hard-constraint list is slightly broader than DCMA's. CIOB PP21 Standard omits three of the fifteen checks that the Major Projects profile requires: Logic Type, Constraints, and Unique Identifiers.

The Additional Best Practice profile adds two supplementary checks: long lags (lags greater than 44 working days, threshold ≤ 5% of all relationships) and soft constraints (As-Soon-As-Possible or Start/Finish-No-Earlier-Than constraints on incomplete activities, threshold ≤ 5% of incomplete activities).

### Proxy and fallback notes

A handful of checks are implemented as a defensible proxy for the literal industry-standard test rather than the full test itself, because the full test needs something Logic+ doesn't have (like a deliberately delayed what-if reschedule):

- **Critical path test** (DCMA 12 / CIOB 15) – proxied by checking that critical-flagged activities form an unbroken chain to completion.
- **CIOB "works fully described"** (CIOB 10) – proxied by whether activities have a WBS assignment.
- **CIOB "has the programme been rescheduled"** (CIOB 14) – proxied by comparing forecast dates against calculated early dates.
- **CIOB "key dates"** (CIOB 11) – matched by activity-ID prefix (`AD-`/`KD-`) with a date parsed from the milestone's name, since there's no separate key-date flag in the source data.

Where a check uses a proxy, both the check-detail expand view and the schedule-list row's info icon state this. Separately, **High Duration (DCMA 8)** has its own fallback rather than a true proxy: on a schedule with no baseline uploaded, it doesn't go N/A like the other baseline-dependent checks – it silently compares against the *current* schedule's own durations instead, and flags that substitution as a note rather than withholding a result.

## Calculation and other logic

**Data used:** for each uploaded schedule, activity-level data covering predecessor/successor links and lag, constraint types, float, duration, actual and forecast dates, calendars, and WBS codes, plus – for the checks that need one – a baseline for comparison. The baseline used is currently **whichever uploaded schedule has the earliest data date**, not a schedule you've separately marked as the baseline.

**How it's calculated:** DCMA 14-Point, CIOB PP21, and Additional Best Practice are three separately implemented check sets, not one shared engine with relabelled outputs – they share only a few low-level helpers (for example, how a relationship link or a float value is read from the schedule), and even define their own separate hard-constraint lists rather than reusing one. Even where two profiles test a conceptually similar thing, such as float or duration, the pass/fail thresholds genuinely differ between standards rather than being the same number reused. There's no separate "generate report" step and no stored report distinct from the schedule data itself – but results for a given schedule and profile are cached client-side once computed, and only recomputed the next time that project's schedules are freshly fetched from the server, not on every visit to the page.

## Note

CSV export is available at several levels: a single check's offenders, the whole check-detail table for a schedule, and – separately – any Assessment Scope population, category, or the full assessment scope.
