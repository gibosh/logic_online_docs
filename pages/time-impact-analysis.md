---
page-id: time-impact-analysis
route: /time-impact-analysis
title: Time-Impact Analysis
audience: external
status: draft
version: 1.5.0
last-reviewed: 2026-10-28
blocked-reason: 2026-10-28 – Setting up, Building a fragnet, Reading the result, Schedule impact views and Comparing fragnets REWRITTEN against the current source (TiaScheduleWorkspace.tsx, TimeImpactAnalysis.tsx, FragnetInlineTable.tsx, FragnetFieldHelp.tsx, ExportImpactedSchedule.tsx): the UI had moved on from the 13 Oct text – the Schedule settings button is now Fragnet settings, Refresh analysis is now Analyse Target, Stack fragnets and Suggest tasks (guided insertion) no longer exist, the Cascade view is gone (four views: Full Network / Driving Path / Moved Only / Charged), the four-figure strip is now three (Original target finish / Target finish after fragnets / Target movement – wording is "target", not PC), the fragnet editor is now an inline Gantt with Save/Cancel, activity calendar replaces WBS, and fragnets can be saved as XER/PP/XML (PRs #528, #530, #531, #533, #538, #542; LUSB-1314, 1343, 1350, 1329). NOT verified live: the exact behaviour of the Charged view (its old per-activity labels were not found in the source, so it is described minimally), the legend button location, whether .mpp schedules can be analysed (only that saving back is unsupported), and the Asta-specific scheduling notes. Not screenshot-verified. Previous note: 2026-10-13 – Updated for LUSB-1307 (TIA UI, v1.2.18) and LUSB-1320 (P6 parity, v1.2.23), verified against v1.2.25 TimeImpactAnalysis.tsx, Charts.tsx, TimeImpactAnalysisPage.tsx. Label "Target Activity" (was "Milestone Activity" in app / "Practical completion activity" here). Scheduling/Relationship lag now in a "Schedule settings" gear popover, plus new "Calculate start-to-start lag from" (Early/Actual Start). Net ΔPC redefined (duration+lag zeroed, links kept) and no longer claimed to reconcile; new "dependent fragnets" tag; Driving figure gains "No completion movement" / "Rescheduled completion"; Cascade middle step now "Schedule response" (no longer "absorbed by float"); duration validation; years in dates. Not screenshot-verified. Earlier note: New module, written from source (frontend/src/time-impact-analysis/) but not yet checked against the running app or a live traceback-suggested insertion. Scoped deliberately to what's implemented and tested today. The "Reconciliation vs source float" panel/section was removed 2026-09-18 (`062259547`) – TIA no longer blocks its own display on a reconciliation mismatch or missing data date; Full network/Driving path now always show the original schedule even if the impact calculation fails.
---

## Time-Impact Analysis – Overview

Time-Impact Analysis (TIA) is a different kind of tool to the rest of Logic+: instead of reporting on what a schedule already shows, it lets you build a hypothetical delay event, insert it into the real schedule at the point you choose, and see exactly how much it moves – or doesn't move – Practical Completion.

This is the classic forensic-scheduling technique used to test a delay claim before it's made: model the event as a small chain of activities (a **fragnet**), tie it into the network where it's said to have happened, and let Logic+ recalculate the schedule to show the real effect. Everything runs live, in your browser, against a schedule you already have loaded – nothing is uploaded unless you choose to [save the impacted schedule](#saving-the-impacted-schedule).

This is not the same job as [Delay Analysis](pages/delay-analysis.md). Delay Analysis explains what a chain of *already-recorded* schedule changes actually did between two real uploaded updates. Time-Impact Analysis is for testing a scenario that may not correspond to any uploaded schedule at all – "if this event happened here, for this long, what would it have done to the finish date?"

Reach it from **[Delay Analysis](pages/delay-analysis.md)** in the left-hand navigation – it's one of the two delay-analysis methods offered there, alongside Traceback.

## Setting up

Select a **project** and **schedule** to analyse, and the **Target Activity** – the completion point you're measuring against, auto-detected from the schedule (the last activity on its primary float path, or a finish milestone), but you can choose a different one from the dropdown.

The schedule opens as a **Full Network** Gantt chart. Build your fragnets in the tray at the bottom (see [below](#building-a-fragnet)), then click **Analyse Target** to calculate the impact.

Three settings control how the schedule recalculates. They're under the **Fragnet settings** button (the gear icon beside **Analyse Target**), defaulted from the schedule file's own settings but adjustable. These are the same progressed-schedule and lag-calendar options P6 itself uses – if you're not sure which your schedule was built with, the defaults already reflect what's in the file.

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

Fragnets are built in the tray at the bottom of the screen, on the **Fragnet Details** tab (the other tab is **Completion & Comparison** – see [Reading the result](#reading-the-result)). Start a fragnet either way:

- Click **+ New Fragnet**, or
- Use the add-fragnet button on an activity's row in the activity list (*Add fragnet after [activity ID]*) – the new fragnet starts at that activity. (This button is available in the **Full Network** and **Driving Path** views.)

The tray lists your fragnets in a table with these columns:

| Column | What it is |
|---|---|
| **Include** | Tick to include the fragnet in the analysis. The tray header shows how many are included (e.g. *"2 fragnets included in the impact"*). |
| **ID Prefix** | Becomes the fragnet's ID and prefixes its activities – **ABC** gives ABC-1, ABC-2, and so on. Leave it blank and the next available default prefix is used. |
| **Fragnet** | The fragnet's name. If you enter one, Logic+ creates a child WBS with that name under the predecessor's WBS; leave it blank to use the predecessor's WBS. |
| **Fragnet predecessor** / **Fragnet successor** | The existing schedule activities the new chain ties into. Search the dropdown by activity ID or name, **or** click into the field and then click an activity in the main Gantt chart to pick it. |
| **Action** | **Go to fragnet in schedule**, **Edit** (the pencil – becomes a tick when you're done), and **Delete** (the bin, with a confirmation: *"Delete fragnet?"*). |

The **?** icon beside each column heading explains that field.

### Editing a fragnet's activities

Editing a fragnet opens a small Gantt chart underneath its row. It shows the predecessor and successor activities for reference, with your fragnet's own activities between them. Its columns are **Activity ID**, **Activity name**, **Calendar**, **Duration (wd)**, the relationship to the next activity (Finish-to-Start, Start-to-Start, Finish-to-Finish or Start-to-Finish) and its lag in working days, and **Action**.

- Click **+** to add an activity. Enter a **name**, choose a **calendar** from the schedule's own calendars, a **duration** (zero or a positive number of working days), and the **relationship** and **lag** to the next activity.
- Confirm a new or edited row with the tick, or by pressing Enter. Use the bin to discard a new row. Named rows also save when you click away.
- To overlap two activities, change a link to Start-to-Start or use a negative lag.

While you edit, the chart previews the fragnet. If something stops the preview (for example a circular link), **Preview unavailable** explains why and **Save** is disabled.

When you're happy, click **Save** to keep the fragnet or **Cancel** to throw away your changes (cancelling a brand-new fragnet removes it). If you try to switch view, tab or schedule with unsaved edits, Logic+ asks **Discard fragnet changes?** – *"You have unsaved fragnet edits. Switching away will discard these changes."* – with **Discard changes** or **Keep editing**.

Your fragnets are remembered in your browser, so they're still there when you come back to the same schedule on the same computer.

### Running the analysis

Multiple fragnets can exist at once – tick **Include** on the ones you want in the result. Changes don't recalculate immediately: edit freely, then click **Analyse Target** to update the results. While changes are waiting, a message reads *"Changes ready to analyse. Select Analyse Target to update results."* If any fragnet activity has an invalid duration, it reads *"Fix invalid durations before analysing."* and the analysis won't run until it's corrected.

## Reading the result

Open the **Completion & Comparison** tab. A three-figure strip shows:

| Figure | Meaning |
|---|---|
| **Original target finish** | The target activity's finish before any fragnet is inserted. If the schedule's own dates can't be used as a starting point, this reads **Without fragnet delays** instead – the target finish when the schedule is recalculated with no fragnets. |
| **Target finish after fragnets** | The target's finish with the included fragnets inserted |
| **Target movement** | The change, in calendar days (**cd**). Underneath, **24/7 calendar** notes the days are calendar days, **no impact applied** shows when no fragnet is included, and **target unavailable** shows when the target can't be calculated |

Any warnings from the calculation are listed below the strip.

## Schedule impact views

Four view tabs sit above the chart:

| View | Shows |
|---|---|
| **Full Network** | Every activity in the schedule – the view you start in |
| **Driving Path** | The activities on the driving path to the target, with your fragnets. Clicking **Analyse Target** switches to this view |
| **Moved Only** | Just the activities whose dates changed. Their previous positions show as baseline bars, with lines showing the movement to the impacted dates |
| **Charged** | The delay charged to each affected activity |

A **legend** (the info button on the chart toolbar) explains the colours for the current view, and the chart toolbar also has **Configure columns**, fullscreen, and a [print preview](pages/gantt-viewer.md#printing).

**If the impact calculation can't complete** (for example, a data or logic issue in the schedule), the **Full Network** and **Driving Path** views still show the original, unimpacted schedule.

## Comparing fragnets

Also on the **Completion & Comparison** tab, **Compare Selected Fragnets** shows a chart and a table with one row per included fragnet:

- **Gross movement (cd)** – the movement that fragnet causes on its own. If it's linked to other fragnets it depends on, those upstream fragnets are included too.
- **Net movement (cd)** – the completion movement removed when that fragnet's duration and lag are set to zero, keeping its links in place – its marginal contribution with every included fragnet combined
- A **drives target** tag against whichever fragnet's net effect is largest
- An **All together** row with the combined movement. Beside it, a **dependent fragnets** tag when one fragnet relies on another's links (their gross figures share those dependencies, so **don't add them together**), or otherwise a **concurrency** tag when the fragnets' combined effect is less than the sum of their individual (gross) effects – a sign their impacts overlap in time.

Because concurrent delays overlap, the net figures **won't always add up** to the combined total movement – the table shows the total alongside them.

## Saving the impacted schedule

Once you've run the analysis, click **Save schedule** (the disk icon beside **Analyse Target**) to keep the schedule *with your fragnets inserted*. In the **Save schedule** dialog, enter a **Filename**, then choose where it goes:

- **Save to Logic+ project** – uploads it to the project as a new schedule, exactly like any other upload (so it needs the **Edit** role and is processed before it's ready)., unticked by default
- **Save to external [format]** – downloads it to your computer (ticked by default)

The file is saved in the same format as the schedule you analysed: **XER** (P6), **PP** (Asta Powerproject) or **XML** (MSPDI). Saving a Microsoft Project **.mpp** file isn't supported – the button is disabled with a tip pointing you to download the original from Schedule Manager. The button is also disabled until the analysis has been run and is up to date.

## Calculation and other logic

**Data used:** the selected schedule packet only – its activities, relationships, and calendars. No separate upload or comparison schedule is needed; a fragnet is inserted into, and compared against, this one schedule.

**How it's calculated:** Logic+ runs a full forward/backward-pass recalculation of the schedule twice – once as-is, once with the fragnet's activities and its two connecting relationships added – using the **Fragnet settings** selected above. For P6 schedules (.xer), the recalculation matches P6's own behaviour for calendars, milestones, progress, constraints and as-late-as-possible activities, so the as-is result lines up with the imported dates. Asta Powerproject (.pp) schedules are recalculated using Asta's own scheduling rules, including how summary activities take their dates. Level of Effort and WBS Summary activities take their dates from the recalculated activities they span, and an activity with no duration is treated as zero. Practical Completion is read from the completion activity's calculated finish in each version; the movement is the difference between the two, in both calendar days and working days on the schedule's reference calendar.

**Gross vs. net, and concurrency:** with more than one fragnet included, "gross" analyses each fragnet against the unmodified schedule – together with any other fragnets it's linked to and depends on. "Net" is the movement removed when that one fragnet's duration and lag are set to zero, leaving its links in place, so each fragnet's net figure is its own marginal contribution. When fragnets compete for the same window of delay, the net figures can add up to less than the combined movement; concurrency is flagged whenever the combined effect comes in below the sum of the individual gross effects.
