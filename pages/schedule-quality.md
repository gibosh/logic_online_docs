---
page-id: schedule-quality
route: /module/3/project/:projectId
title: Schedule Quality
audience: external
status: complete
version: 1.4.0
last-reviewed: 2026-09-23
blocked-reason: Renamed page title/H1 from "DCMA 14-Point Check" to "Schedule Quality" 2026-09-23, matching the module tile and sidebar parent link naming everywhere else on the site – the page has covered four quality profiles (DCMA plus two CIOB PP21 variants plus Best Practice) since at least 2026-09-08's review, and the old title no longer reflected that scope. Also fixed the CIOB PP21 Standard threshold wording, which contradicted the page's own later explanation of which checks it omits.
---

## Overview

A forecast or delay analysis is only as reliable as the schedule behind it. Schedule Quality runs an automated health check on every schedule uploaded to a project and tells you, as a plain pass or fail, whether each one meets an industry quality standard.

A planner or client can run the same checks by hand in P6, and if they find a quality problem before you do, it undermines your analysis. Running the check automatically, every time a new schedule is uploaded, means each analysis starts from a current, documented quality position.

## Quality profiles

Choose a profile from the dropdown to apply a different standard. The profile applies to every schedule on the page. For the DCMA and CIOB profiles, a **"View rules (PDF)"** link next to the dropdown opens the standard the checks are built from. The Best Practice profile has no PDF, because it isn't drawn from a single published standard.

| Profile (dropdown label) | Standard | Pass threshold |
|---------|----------|----------------|
| DCMA 14-Point Assessment | DCMA-PAM-200-1 section 4 | All 14 checks must pass |
| CIOB PP21 – Standard Projects | CIOB Planning Protocol 2021 | 12 checks run (3 of the 15 are omitted for this profile), all 12 must pass |
| CIOB PP21 – Major Projects | CIOB Planning Protocol 2021 | All 15 checks must pass |
| Additional Industry Best Practise Checks | Supplementary checks | Both checks must pass |

The default profile is **DCMA 14-Point Assessment**. Change it to apply a different standard, for example CIOB PP21 Major Projects for large infrastructure contracts that expect the full 15-check standard.

The wording varies slightly by profile and by where it appears. The two CIOB entries appear in the dropdown as above, but the results panel header reads **"CIOB PP21 Stress Test (Standard Projects)"** or **"...(Major Projects)"**. The fourth profile is labelled **"Additional Industry Best Practise Checks"** throughout the app, spelt "Practise" as in the table.

## Schedule list

The left panel lists every schedule uploaded to the project, one row each, ordered by data date with the oldest first. Each row shows:

- **Data date**: the schedule's data date
- **Schedule**: the schedule's own identifier
- **Score**: checks passed out of the checks that apply for the selected profile (for example `12/14`)
- **Result**: a green **Pass** or red **Fail** badge for the selected profile. A small info icon also appears if any check on that schedule has an explanatory note (see Proxy and fallback notes below); hover it to read the note.

The most recently uploaded schedule is selected by default. Click a row to load that schedule's check results in the detail panel on the right.

## Check detail

The right panel shows every check run on the selected schedule, one row per check.

| Column | Description |
|--------|-------------|
| # | Check number within the profile |
| Check | Name of the quality check |
| Result | Metric value for this schedule (e.g. percentage of activities with no logic) |
| Target | The pass/fail threshold for this profile |
| Count | The raw numerator / denominator behind the Result percentage (e.g. `3/57`) |
| Status | Green **Pass** or red **Fail** badge, or **N/A** if the check does not apply |

**Click any check row** to expand it. The expanded view shows the check's full description, an **Export CSV** button for that check, any proxy or fallback note that applies (see below), and the failing items. Where relevant these are split into **Activities** and **Relationships** tables, because the two have different columns: activities show project/activity ID, name, detail and unit; relationships show the predecessor's and successor's project, ID and name, plus detail and unit. Each table shows up to 50 failing items, with a "+N more" row if there are more. Use the list to find what to correct in P6 before uploading again.

**N/A** means the check doesn't apply to the selected profile (for example, CIOB PP21 Standard omits some checks that Major Projects runs). For a couple of DCMA checks it can also mean the schedule lacks what the check needs: a baseline, or resource loading.

## Assessment Scope

The Assessment Scope panel sits between the schedule list and the check detail whenever a schedule is selected. The check detail shows each check's *result*; Assessment Scope shows *which activities and relationships fed that result*. Use it when a number looks surprising and you want to confirm nothing was wrongly included or excluded.

It has three levels, each of which you can export to CSV:

1. **Base populations**: a top-level table (for example "Schedule rows", "Relationships", and profile-specific populations such as "Incomplete activities" or "Critical-path activities") showing how many items were Included and Excluded, and which check numbers use that population.
2. **Expand a population** to see its categories. For "Schedule rows", for example, it shows how many are Activities, Milestones, Level of Effort or Summary-WBS rows, and whether each category is Included or Excluded.
3. **Expand a category** to see its individual activities or relationships. Each has a Pass, Fail, N/A or Excluded badge for every check number that uses it, so you can trace one activity through each check it was scored on.

## What the checks cover

The DCMA 14-Point Assessment covers:

1. Logic: activities missing a predecessor or successor, threshold ≤ 5%
2. Leads: negative-lag relationships, threshold 0% (none allowed)
3. Lags: positive-lag relationships, threshold ≤ 5%
4. Relationship types: Finish-to-Start relationships, threshold ≥ 90% of all relationships
5. Hard constraints: activities with a hard date constraint applied, threshold ≤ 5%
6. High float: activities with total float over 44 working days, threshold ≤ 5%
7. Negative float: activities with negative total float, threshold 0% (none allowed)
8. High duration: activities with a duration over 44 days against the baseline, threshold ≤ 5%
9. Invalid dates: actual dates after the status date, or forecast dates before the status date for activities that haven't started or finished, threshold 0% (none allowed)
10. Resources: activities with a duration but no resource assigned, threshold 0% (none allowed; not applicable if the schedule isn't resource-loaded)
11. Missed tasks: activities baselined to have started or finished by the status date that haven't, threshold ≤ 5% (not applicable without a baseline)
12. Critical path test: an unbroken chain of critical-flagged activities reaching the completion milestone (a proxy for the full DCMA test, which needs an intentional-delay reschedule)
13. Critical path length index (CPLI): (critical path length + total float) ÷ critical path length, pass at ≥ 0.95
14. Baseline execution index (BEI): completed activities ÷ activities due to have completed by the status date, pass at ≥ 0.95

Most of these checks are measured against **incomplete activities only**, excluding WBS summary rows, Level-of-Effort activities **and milestones**. Invalid Dates (9) and Missed Tasks (11) are the exceptions: they're measured against all activities, including completed ones. The three relationship checks (Leads, Lags, Relationship types) are measured against the total number of relationships instead of activities.

The CIOB PP21 profiles apply a related set of 15 checks based on the CIOB Planning Protocol 2021. Most mirror the DCMA checks above, and four are specific to CIOB: named key-date milestones forecast past their target date, calendar references that can't be resolved, duplicate activity IDs or names, and whether the schedule has been rescheduled to the status date. CIOB's activity population **keeps milestones**, which DCMA excludes, because CIOB's float and duration stress tests apply to them too. Most CIOB thresholds are stricter than the DCMA equivalent. For example, CIOB's float and duration checks allow 0% over the 44-day mark where DCMA allows up to 5%, and CIOB's hard-constraint list is slightly broader than DCMA's. CIOB PP21 Standard omits three of the fifteen checks that Major Projects requires: Logic Type, Constraints and Unique Identifiers.

The Additional Best Practice profile adds two checks: long lags (lags over 44 working days, threshold ≤ 5% of all relationships) and soft constraints (As-Soon-As-Possible or Start/Finish-No-Earlier-Than constraints on incomplete activities, threshold ≤ 5% of incomplete activities).

### Proxy and fallback notes

A few checks use a proxy for the industry test instead of the full test, because the full test needs something Logic+ doesn't have, such as a deliberately delayed what-if reschedule:

- **Critical path test** (DCMA 12 / CIOB 15) checks that critical-flagged activities form an unbroken chain to completion.
- **CIOB "works fully described"** (CIOB 10) checks whether activities have a WBS assignment.
- **CIOB "has the programme been rescheduled"** (CIOB 14) compares forecast dates with calculated early dates.
- **CIOB "key dates"** (CIOB 11) matches activities by activity-ID prefix (`AD-`/`KD-`) and reads the date from the milestone's name, because the source data has no separate key-date flag.

Where a check uses a proxy, both the expanded check and the info icon on the schedule-list row say so. **High Duration (DCMA 8)** works a little differently: on a schedule with no baseline, it doesn't show N/A like the other baseline checks. It compares each activity's duration with the schedule's own current durations instead, and notes this on the check.

## Calculation and other logic

**Data used:** for each uploaded schedule, activity-level data covering predecessor and successor links and lags, constraint types, float, durations, actual and forecast dates, calendars and WBS codes, plus a baseline for the checks that need one. The baseline is currently **the uploaded schedule with the earliest data date**; you can't mark a different schedule as the baseline.

**How it's calculated:** DCMA 14-Point, CIOB PP21 and Additional Best Practice are three separately built sets of checks. They share only a few low-level helpers, such as how a relationship link or a float value is read from the schedule, and each has its own hard-constraint list. Where two profiles test a similar thing, such as float or duration, their thresholds differ between standards. There's no separate report-generation step or stored report. Results for a schedule and profile are cached in your browser once calculated, and recalculated only the next time the project's schedules are fetched from the server, not on every visit.

**Critical Path Length Index (CPLI):** built from the schedule's completion point, which Logic+ finds from the network, not by name. It's the latest-finishing milestone with no successors; if there isn't one, the latest-finishing activity with no successors; and failing that, the latest-finishing activity in the schedule. (Forecast Confidence finds its completion milestone by name instead.) "Critical path length" here is the elapsed time between two dates, not a sum of activity durations along a chain:

> Critical path length = working days between the schedule's status date and the completion milestone's own finish date, measured on the completion milestone's own calendar
>
> CPLI = (critical path length + completion milestone's own total float) ÷ critical path length, pass at ≥ 0.95

The total float used is the completion milestone's own total float, not a figure summed or averaged across the activities leading to it. If the completion milestone can't be found, has no finish date, finished at or before the status date or has no resolvable calendar, or if the critical path length works out to zero, CPLI can't be calculated and the check shows N/A.

**Baseline Execution Index (BEI):** measured over a wider population than most DCMA checks, counting both completed and incomplete activities:

> BEI = activities completed ÷ (activities baselined to finish by the status date + activities with no usable baseline finish date to compare against)

Every completed activity in scope counts toward the top of the fraction, including ones that finished ahead of their baseline date, so BEI can read above 1.00. That follows the DCMA standard's own definition. An activity with no matching baseline record, or whose baseline record has no finish date, counts in the bottom of the fraction as if it were still due. This is deliberately cautious, because nothing confirms it was on track. If no baseline schedule is available, or no activity in scope was baselined to finish by the status date, BEI can't be calculated and the check shows N/A.

## Note

You can export CSV at several levels: one check's failing items, the whole check-detail table for a schedule, and, separately, any Assessment Scope population or category, or the full assessment scope.
