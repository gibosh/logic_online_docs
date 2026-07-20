---
page-id: schedule-quality
route: /module/3/project/:projectId
title: Schedule Quality
audience: external
status: draft
version: 1.0.0
last-reviewed: 2026-07-20
---

## Why this page exists

Before any forecast or delay analysis from a schedule can be trusted, the schedule itself needs to be well-built enough to produce reliable results. Schedule Quality runs an automated health check against every schedule you have uploaded for a project and tells you, in plain pass/fail terms, whether each one meets an industry-standard quality threshold.

A planner or client can run the same industry checks manually in P6 — and if they find a quality problem before you do, that is a credibility risk in a meeting. Automating the check means every analysis you run starts from a current, documented, defensible quality position, updated every time a new schedule is uploaded.

---

## Quality profiles

Select a profile from the dropdown to apply a different standard to the check. The profile applies to all schedules shown on the page.

| Profile | Standard | Pass threshold |
|---------|----------|----------------|
| DCMA 14-Point Assessment | DCMA-PAM-200-1 section 4 | All 14 checks must pass |
| CIOB PP21 — Standard Projects | CIOB Planning Protocol 2021 | 12 of 15 checks must pass |
| CIOB PP21 — Major Projects | CIOB Planning Protocol 2021 | All 15 checks must pass |
| Additional Industry Best Practice | Supplementary checks | Both checks must pass |

The default profile is **DCMA 14-Point Assessment**. Change the profile to apply a different standard — for example, CIOB PP21 Major Projects for large infrastructure contracts where the full 15-check standard is expected.

---

## Schedule list

The left panel lists all schedules uploaded for the project, one row per schedule, ordered by data date. Each row shows:

- **Data date** of the schedule
- **Overall pass/fail** badge for the selected profile

Click a row to load that schedule's full check results in the detail panel on the right.

---

## Check detail

The right panel shows all checks run against the selected schedule, one row per check.

| Column | Description |
|--------|-------------|
| # | Check number within the profile |
| Check name | Name of the quality check |
| Result | Metric value for this schedule (e.g. percentage of activities with no logic) |
| Threshold | The pass/fail threshold for this profile |
| Pass/Fail | Green **Pass** or red **Fail** badge, or **N/A** if the check does not apply |

**Click any check row** to expand it and see the list of activities that contributed to a fail result. Up to 50 offending activities are shown. Use this list to identify exactly which activities need to be corrected in P6 before re-uploading.

**N/A** means the check is not applicable for the selected profile (for example, some CIOB PP21 Standard checks are omitted relative to the Major Projects profile).

---

## What the checks cover

The DCMA 14-Point Assessment covers:

1. Logic — activities missing a predecessor or successor, threshold ≤ 5%
2. Leads — negative-lag relationships, threshold 0% (none allowed)
3. Lags — positive-lag relationships, threshold ≤ 5%
4. Relationship types — Finish-to-Start relationships, threshold ≥ 90% of all relationships
5. Hard constraints — activities with a hard date constraint applied, threshold ≤ 5%
6. High float — activities with total float over 44 working days, threshold ≤ 5%
7. Negative float — activities with negative total float, threshold 0% (none allowed)
8. High duration — activities with duration over 44 days against the baseline, threshold ≤ 5%
9. Invalid dates — forecast dates before, or actual dates after, the status date, threshold 0% (none allowed)
10. Resources — activities with a duration but no resource assigned, threshold 0% (none allowed; not applicable if the schedule is not resource-loaded)
11. Missed tasks — activities due to have started or finished by the status date that have not, threshold ≤ 5% (not applicable without a baseline)
12. Critical path test — unbroken chain of critical-flagged activities reaching the completion milestone (a proxy for the full DCMA test, which requires an intentional-delay reschedule)
13. Critical path length index — (critical path length + total float) ÷ critical path length, pass at ≥ 0.95
14. Baseline execution index — completed activities ÷ activities due to have completed by the status date, pass at ≥ 0.95

Checks are measured against incomplete activities only, excluding WBS summary rows and Level-of-Effort activities.

The CIOB PP21 profiles apply a related but distinct set of 15 checks aligned to the CIOB Planning Protocol 2021 — largely mirroring the DCMA checks above, plus a few unique to CIOB (unresolvable calendar references, named key-date milestones forecast past their target, duplicate activity IDs or names, and whether the schedule has actually been rescheduled to the status date). Some CIOB thresholds are stricter than their DCMA equivalent — for example, CIOB's float and duration checks allow 0% over the 44-day mark, where DCMA allows up to 5%. CIOB PP21 Standard omits three of the fifteen checks (logic type, hard constraints, and unique identifiers) that the Major Projects profile requires.

The Additional Best Practice profile adds two supplementary checks: long lags (lags greater than 44 working days, threshold ≤ 5% of all relationships) and soft constraints (As-Soon-As-Possible or Start/Finish-No-Earlier-Than constraints on incomplete activities, threshold ≤ 5% of incomplete activities).

A handful of checks are implemented as a defensible proxy for the literal industry-standard test rather than the full test itself — the critical path test above, and (CIOB only) the "works fully described" check (proxied by WBS assignment) and the "has the programme been rescheduled" check (proxied by comparing forecast dates against calculated early dates). Where a check uses a proxy, the check detail panel states this against that check.
