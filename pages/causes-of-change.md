---
page-id: causes-of-change
route: TBD — Analytics Dashboard (module ID not yet allocated)
title: Causes of Change
audience: external
status: stub
version: 1.0.0
last-reviewed: 2026-07-20
blocked-reason: Module not yet built (confirmed against current codebase — no matching component exists on main). Business-problem layer documented here; technical detail and UI description to follow once the module is live.
---

## Causes of Change — Overview

When a group of activities suddenly moves later together between schedule updates, something happened upstream that pushed them. A planner often has a hunch about what it was — a permit delay, a subcontractor issue, a late decision — but no systematic way to test it, and no way to separate a genuine trigger event from ordinary monthly re-baselining noise.

Causes of Change automates that search: it finds the cluster of activities that moved together, names the most plausible upstream trigger (the activity that directly links to the most of them), and shows how large the movement was compared to the schedule's own background variation. A shift that is genuinely distinctive stands out from routine admin clearly.

## Report details

*(Detail pending module build and live-app validation. Content will cover: how to read the cluster view, how to interpret the suggested trigger, how to assess the confidence of the pattern, and how to use this alongside Critical Path Evolution.)*

## Important

The trigger activity identified here **directly links to** the affected activities — it does not "cause" the shift in a proven sense. This is a statistical pattern-match, not a causal finding. Logic+ surfaces it as a starting point for investigation, not a conclusion. This distinction matters in contexts where delay analysis may be used in a contractual or dispute setting — the display makes it explicit.
