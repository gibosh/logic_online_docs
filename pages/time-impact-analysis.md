---
page-id: time-impact-analysis
route: /time-impact-analysis
title: Time-Impact Analysis
audience: external
status: draft
version: 1.4.0
last-reviewed: 2026-10-13
blocked-reason: 2026-10-13 – Updated for LUSB-1307 (TIA UI, v1.2.18) and LUSB-1320 (P6 parity, v1.2.23), verified against v1.2.25 TimeImpactAnalysis.tsx, Charts.tsx, TimeImpactAnalysisPage.tsx. Label "Target Activity" (was "Milestone Activity" in app / "Practical completion activity" here). Scheduling/Relationship lag now in a "Schedule settings" gear popover, plus new "Calculate start-to-start lag from" (Early/Actual Start). Net ΔPC redefined (duration+lag zeroed, links kept) and no longer claimed to reconcile; new "dependent fragnets" tag; Driving figure gains "No completion movement" / "Rescheduled completion"; Cascade middle step now "Schedule response" (no longer "absorbed by float"); duration validation; years in dates. Not screenshot-verified. Earlier note: New module, written from source (frontend/src/time-impact-analysis/) but not yet checked against the running app or a live traceback-suggested insertion. Scoped deliberately to what's implemented and tested today. The "Reconciliation vs source float" panel/section was removed 2026-09-18 (`062259547`) – TIA no longer blocks its own display on a reconciliation mismatch or missing data date; Full network/Driving path now always show the original schedule even if the impact calculation fails.
---

## Time-Impact Analysis – Overview

Time-Impact Analysis (TIA) is a different kind of tool to the rest of Logic+: instead of reporting on what a schedule already shows, it lets you build a hypothetical delay event, insert it into the real schedule at the point you choose, and see exactly how much it moves – or doesn't move – Practical Completion.

This is the classic forensic-scheduling technique used to test a delay claim before it's made: model the event as a small chain of activities (a **fragnet**), tie it into the network where it's said to have happened, and let Logic+ recalculate the schedule to show the real effect. Everything runs live, in your browser, against a schedule you already have loaded – nothing is uploaded or saved elsewhere.

This is not the same job as [Delay Analysis](pages/delay-analysis.md). Delay Analysis explains what a chain of *already-recorded* schedule changes actually did between two real uploaded updates. Time-Impact Analysis is for testing a scenario that may not correspond to any uploaded schedule at all – "if this event happened here, for this long, what would it have done to the finish date?"

Reach it from **[Delay Analysis](pages/delay-analysis.md)** in the left-hand navigation – it's one of the two delay-analysis methods offered there, alongside Traceback.

## Setting up

Select a **project** and **schedule** to analyse, and the **Target Activity** – the completion point you're measuring against, auto-detected from the schedule (the last activity on its primary float path, or a finish milestone), but you can choose a different one from the dropdown.

Three settings control how the schedule recalculates. They're under the **Schedule settings** button (the gear icon on the **Select Schedule** card), defaulted from the schedule file's own settings but adjustable. These are the same progressed-schedule and lag-calendar options P6 itself uses – if you're not sure which your schedule was built with, the defaults already reflect what's in the file.

**Scheduling** – this only matters for an activity that's already under way (has started but not finished). It decides how the *remaining*, not-yet-worked portion of that activity gets positioned when the schedule recalculates:

| Option | What it does |
|---|---|
| Retain logic | The remaining work stays tied to the activity's original predecessor logic, even once it's under way – its position (and whether it still counts as "driven" by that predecessor) keeps following the network logic rather than where progress on site actually resumed. This can produce a remaining-work position that doesn't match real progress, which is the trade-off this option accepts. |
| Progress override | Once an activity is under way, its logic tie to its predecessor is set aside for the remaining work – it's repositioned to start from the schedule's own current progress point instead, regardless of what the original logic link implied. |
| Actual dates | Behaves like Progress override – logic is set aside once work starts – but if the schedule itself recorded an explicit resume date for the remaining work, that recorded date is used, never earlier than the schedule's own progress point. |

**Relationship lag** – every logic link that carries a lag (e.g. "start 5 days after") has to convert that lag into elapsed time on *some* calendar, since 5 days means something different on a 5-day-week calendar than a 7-day one. This setting picks which calendar does the converting for every link in the network:

| Option | What it does |
|---|---|
| Predecessor calendar | Each link's lag is converted using the predecessor activity's own calendar. |
| Successor calendar | Each link's lag is converted using the successor activity's own calendar instead. |
| 24-hour calendar | Each link's lag is converted using a fixed round-the-clock, seven-day calendar, regardless of either activity's own working pattern. |
| Project default | Each link's lag is converted using the project's overall default calendar. |

**Calculate start-to-start lag from** – for an activity that's already started, whether a start-to-start link's lag is counted from its **Early Start** or its **Actual Start**.

## Building a fragnet

Click **+ New fragnet** to add one. Each fragnet has:

- One or more **activities** – name, duration (zero or a positive number of working days), and calendar, added with **+ add activity to this fragnet**
- **Links** between its own activities (Finish-to-Start, Start-to-Start, Finish-to-Finish, or Start-to-Finish, with a working-day lag) – change a link to Start-to-Start or use a negative lag to overlap two activities
- A **fragnet predecessor** and **fragnet successor** – the existing schedule activities (or another fragnet's activities) the new chain ties into, each with its own relationship type and lag. Both are searchable – start typing an activity ID or name to filter the list instead of scrolling through every activity in the schedule.

**Suggest tasks for this fragnet** runs a real traceback (the same Default-profile engine used in Delay Analysis) backward from the fragnet's successor, and offers the traced chain of activities as ready-made **insertion windows** – pick one to set the fragnet's predecessor/successor automatically, populated with the activities the traceback actually found along that path, rather than starting from a blank fragnet.

Multiple fragnets can exist at once. Tick a fragnet's checkbox to include or exclude it from the analysis; use **Stack fragnets** to view every included fragnet together, or switch it off to focus on one at a time.

Changes to a fragnet don't recalculate immediately – edit freely, then click **Refresh analysis** when you're ready to see the result. A status message tells you when there are unsaved changes waiting to be analysed. If any fragnet activity has an invalid duration, the message reads *Fix invalid durations before analysing* and the analysis won't run until it's corrected.

## Reading the result

A four-figure strip at the top of the results panel:

| Figure | Meaning |
|---|---|
| Original PC | Practical Completion before the fragnet is inserted |
| PC after | Practical Completion with the fragnet inserted |
| Movement (ΔPC) | The change, in calendar days |
| Driving | **On the driving path** or **Off the driving path** – whether the fragnet's own activities are on the driving path to completion. **No completion movement** – the fragnet doesn't move completion at all. With no fragnet included, **No impact applied** – or **Rescheduled completion**, if recalculating the schedule on its own moves completion |

## Schedule impact views

Five view modes, switched from the toolbar above the chart:

| View | Shows |
|---|---|
| **Full network** | Every activity in the schedule |
| **Moved only** | Just the activities whose dates actually changed (plus the fragnet itself), distinguishing the driver (on the path to completion) from passengers merely dragged along |
| **Driving path** (default) | The activities on the driving path, before and after – original dates shown as a dashed outline behind the current bar where they differ |
| **Charged** | One row per affected activity, each labelled with its own calendar-day change and whether it **reaches PC** or is **absorbed** by float. A line underneath compares the charges that reach PC with the total movement – marked ✓ when they match, or showing both figures when they don't |
| **Cascade** | A three-step flow: the fragnet's elapsed span, the **schedule response** (how calendars, dependency links and float shape the effect), and the resulting movement at Practical Completion |

Dates and timeline labels include the year. Across every view, a dashed amber line marks the schedule's data date, and moved activities carry a small arrow from their original position to their current one.

**If the impact calculation can't complete** (for example, a data or logic issue in the schedule), **Full network** and **Driving path** still show the original, unimpacted schedule. The other three views (Moved only, Charged, Cascade) can't be shown without a completed calculation – switching to one of them shows: *"Impact results are unavailable. Select Driving path or Full network to view the original schedule."*

## Comparing fragnets

With two or more fragnets included, a comparison chart and table show, for each fragnet:

- **Gross ΔPC** – the movement that fragnet causes on its own. If it's linked to other fragnets it depends on, those upstream fragnets are included too.
- **Net ΔPC** – the completion movement removed when that fragnet's duration and lag are set to zero, keeping its links in place – its marginal contribution with every included fragnet combined
- A **drives PC** tag against whichever fragnet's net effect is largest
- A **dependent fragnets** tag when one fragnet relies on another's links. Their gross figures share those dependencies, so **don't add them together**.
- Otherwise, a **concurrency** tag when the fragnets' combined effect is less than the sum of their individual (gross) effects – a sign their impacts overlap in time

Because concurrent delays overlap, the net figures **won't always add up** to the combined total movement – the table shows the total alongside them.

## Calculation and other logic

**Data used:** the selected schedule packet only – its activities, relationships, and calendars. No separate upload or comparison schedule is needed; a fragnet is inserted into, and compared against, this one schedule.

**How it's calculated:** Logic+ runs a full forward/backward-pass recalculation of the schedule twice – once as-is, once with the fragnet's activities and its two connecting relationships added – using the schedule settings selected above. For P6 schedules (.xer), the recalculation matches P6's own behaviour for calendars, milestones, progress, constraints and as-late-as-possible activities, so the as-is result lines up with the imported dates. Level of Effort and WBS Summary activities take their dates from the recalculated activities they span, and an activity with no duration is treated as zero. Practical Completion is read from the completion activity's calculated finish in each version; the movement is the difference between the two, in both calendar days and working days on the schedule's reference calendar.

**Gross vs. net, and concurrency:** with more than one fragnet included, "gross" analyses each fragnet against the unmodified schedule – together with any other fragnets it's linked to and depends on. "Net" is the movement removed when that one fragnet's duration and lag are set to zero, leaving its links in place, so each fragnet's net figure is its own marginal contribution. When fragnets compete for the same window of delay, the net figures can add up to less than the combined movement; concurrency is flagged whenever the combined effect comes in below the sum of the individual gross effects.
