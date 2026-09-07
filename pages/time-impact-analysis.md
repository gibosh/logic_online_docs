---
page-id: time-impact-analysis
route: /time-impact-analysis
title: Time-Impact Analysis
audience: external
status: draft
version: 1.0.0
last-reviewed: 2026-09-07
blocked-reason: New module, written from source (frontend/src/time-impact-analysis/) but not yet checked against the running app or a live traceback-suggested insertion. Scoped deliberately to what's implemented and tested today.
---

## Time-Impact Analysis – Overview

Time-Impact Analysis (TIA) is a different kind of tool to the rest of Logic+: instead of reporting on what a schedule already shows, it lets you build a hypothetical delay event, insert it into the real schedule at the point you choose, and see exactly how much it moves – or doesn't move – Practical Completion.

This is the classic forensic-scheduling technique used to test a delay claim before it's made: model the event as a small chain of activities (a **fragnet**), tie it into the network where it's said to have happened, and let Logic+ recalculate the schedule to show the real effect. Everything runs live, in your browser, against a schedule you already have loaded – nothing is uploaded or saved elsewhere.

This is not the same job as [Delay Analysis](pages/delay-analysis.md). Delay Analysis explains what a chain of *already-recorded* schedule changes actually did between two real uploaded updates. Time-Impact Analysis is for testing a scenario that may not correspond to any uploaded schedule at all – "if this event happened here, for this long, what would it have done to the finish date?"

Reach it from the **Time-Impact Analysis** link in the main navigation bar.

## Setting up

Select a **project** and **schedule** to analyse, and the **Practical completion activity** – auto-detected from the schedule (the last activity on its primary float path, or a finish milestone), but you can choose a different one from the dropdown.

Two settings control how the schedule recalculates, defaulted from the schedule file's own settings but adjustable:

| Setting | Options |
|---|---|
| Scheduling | Retain logic · Progress override · Actual dates |
| Relationship lag | Predecessor calendar · Successor calendar · 24-hour calendar · Project default |

These are the same progressed-schedule and lag-calendar options P6 itself uses – if you're not sure which your schedule was built with, the defaults already reflect what's in the file.

## Building a fragnet

Click **+ New fragnet** to add one. Each fragnet has:

- One or more **activities** – name, duration, and calendar, added with **+ add activity to this fragnet**
- **Links** between its own activities (Finish-to-Start, Start-to-Start, Finish-to-Finish, or Start-to-Finish, with a working-day lag) – change a link to Start-to-Start or use a negative lag to overlap two activities
- A **fragnet predecessor** and **fragnet successor** – the existing schedule activities (or another fragnet's activities) the new chain ties into, each with its own relationship type and lag

**Suggest tasks for this fragnet** runs a real traceback (the same Replica v2.1 engine used in Delay Analysis) backward from the fragnet's successor, and offers the traced chain of activities as ready-made **insertion windows** – pick one to set the fragnet's predecessor/successor automatically, populated with the activities the traceback actually found along that path, rather than starting from a blank fragnet.

Multiple fragnets can exist at once. Tick a fragnet's checkbox to include or exclude it from the analysis; use **Stack fragnets** to view every included fragnet together, or switch it off to focus on one at a time.

Changes to a fragnet don't recalculate immediately – edit freely, then click **Refresh analysis** when you're ready to see the result. A status message tells you when there are unsaved changes waiting to be analysed.

## Reading the result

A four-figure strip at the top of the results panel:

| Figure | Meaning |
|---|---|
| Original PC | Practical Completion before the fragnet is inserted |
| PC after | Practical Completion with the fragnet inserted |
| Movement (ΔPC) | The change, in calendar days |
| Driving | Whether the fragnet's own activities are actually on the driving path to completion, absorbed by float (no effect), or moving the network without changing the completion date itself |

## Schedule impact views

Five view modes, switched from the toolbar above the chart:

| View | Shows |
|---|---|
| **Full network** | Every activity in the schedule |
| **Moved only** | Just the activities whose dates actually changed (plus the fragnet itself), distinguishing the driver (on the path to completion) from passengers merely dragged along |
| **Driving path** (default) | The activities on the driving path, before and after – original dates shown as a dashed outline behind the current bar where they differ |
| **Charged** | One row per affected activity, each labelled with its own calendar-day change and whether it **reaches PC** or is **absorbed** by float, plus a reconciliation line confirming the charges that reach PC sum to the total movement shown above |
| **Cascade** | A three-step flow: the fragnet's own event duration, how much of that was absorbed by existing float, and what's left as the net movement to Practical Completion |

Across every view, a dashed amber line marks the schedule's data date, and moved activities carry a small arrow from their original position to their current one.

## Comparing fragnets

With two or more fragnets included, a comparison chart and table show, for each fragnet:

- **Gross ΔPC** – the movement that fragnet alone would cause, ignoring every other fragnet
- **Net ΔPC** – its actual marginal contribution once every included fragnet is combined, so overlapping fragnets aren't double-counted
- A **drives PC** tag against whichever fragnet's net effect is largest
- A **concurrency** tag when the fragnets' combined effect is less than the sum of their individual (gross) effects – a sign their impacts overlap in time

The net figures always reconcile exactly to the combined total movement.

## Reconciliation vs source float

Before anything else runs, Logic+ recalculates your existing schedule from scratch (the same in-browser engine used for the fragnet analysis) and checks the result against the float values your source file already reported. The panel shows how many activities reconciled, or flags divergences if the recalculated network doesn't match the source data closely enough.

If the mismatch is too large, the analysis refuses to run at all rather than show figures built on a network that doesn't match your schedule – you'll see an error instead of a result.

## Calculation and other logic

**Data used:** the selected schedule packet only – its activities, relationships, and calendars. No separate upload or comparison schedule is needed; a fragnet is inserted into, and compared against, this one schedule.

**How it's calculated:** Logic+ runs a full forward/backward-pass recalculation of the schedule twice – once as-is, once with the fragnet's activities and its two connecting relationships added – using the scheduling and relationship-lag options selected above. Practical Completion is read from the completion activity's calculated finish in each version; the movement is the difference between the two, in both calendar days and working days on the schedule's reference calendar.

**Gross vs. net, and concurrency:** with more than one fragnet included, "gross" analyses each fragnet completely on its own against the unmodified schedule; "net" is the difference the combined result makes when that one fragnet is removed from the group – so each fragnet's net figure is genuinely its own marginal contribution, and the net figures across all included fragnets always add up to the combined movement. Concurrency is flagged whenever the fragnets' combined effect comes in below the sum of their individual gross effects, meaning they were competing for the same window of delay rather than adding cleanly.
