---
page-id: time-impact-analysis
route: /time-impact-analysis
title: Time-Impact Analysis
audience: external
status: draft
version: 1.6.0
last-reviewed: 2026-11-23
blocked-reason: 2026-11-23 – LUSB-1392: No data date notice for an undated schedule (TimeImpactAnalysisPage.tsx). Fixed broken punctuation in the save options. Humanizer rewrite of the prose; linked headings kept verbatim. 2026-10-28 – Setting up, Building a fragnet, Reading the result, Schedule impact views and Comparing fragnets REWRITTEN against the current source (TiaScheduleWorkspace.tsx, TimeImpactAnalysis.tsx, FragnetInlineTable.tsx, FragnetFieldHelp.tsx, ExportImpactedSchedule.tsx): the UI had moved on from the 13 Oct text – the Schedule settings button is now Fragnet settings, Refresh analysis is now Analyse Target, Stack fragnets and Suggest tasks (guided insertion) no longer exist, the Cascade view is gone (four views: Full Network / Driving Path / Moved Only / Charged), the four-figure strip is now three (Original target finish / Target finish after fragnets / Target movement – wording is "target", not PC), the fragnet editor is now an inline Gantt with Save/Cancel, activity calendar replaces WBS, and fragnets can be saved as XER/PP/XML (PRs #528, #530, #531, #533, #538, #542; LUSB-1314, 1343, 1350, 1329). NOT verified live: the exact behaviour of the Charged view (its old per-activity labels were not found in the source, so it is described minimally), the legend button location, whether .mpp schedules can be analysed (only that saving back is unsupported), and the Asta-specific scheduling notes. Not screenshot-verified. Previous note: 2026-10-13 – Updated for LUSB-1307 (TIA UI, v1.2.18) and LUSB-1320 (P6 parity, v1.2.23), verified against v1.2.25 TimeImpactAnalysis.tsx, Charts.tsx, TimeImpactAnalysisPage.tsx. Label "Target Activity" (was "Milestone Activity" in app / "Practical completion activity" here). Scheduling/Relationship lag now in a "Schedule settings" gear popover, plus new "Calculate start-to-start lag from" (Early/Actual Start). Net ΔPC redefined (duration+lag zeroed, links kept) and no longer claimed to reconcile; new "dependent fragnets" tag; Driving figure gains "No completion movement" / "Rescheduled completion"; Cascade middle step now "Schedule response" (no longer "absorbed by float"); duration validation; years in dates. Not screenshot-verified. Earlier note: New module, written from source (frontend/src/time-impact-analysis/) but not yet checked against the running app or a live traceback-suggested insertion. Scoped deliberately to what's implemented and tested today. The "Reconciliation vs source float" panel/section was removed 2026-09-18 (`062259547`) – TIA no longer blocks its own display on a reconciliation mismatch or missing data date; Full network/Driving path now always show the original schedule even if the impact calculation fails.
---

## Overview

Time-Impact Analysis (TIA) works differently from the rest of Logic+. Instead of reporting on what a schedule already shows, it lets you build a hypothetical delay event, insert it into the real schedule at the point you choose, and see exactly how far it moves Practical Completion, if at all.

This is the classic forensic-scheduling technique for testing a delay claim before it's made. You model the event as a small chain of activities (a **fragnet**), tie it into the network where it's said to have happened, and let Logic+ recalculate the schedule to show the effect. Everything runs live in your browser, on a schedule you already have loaded, and nothing is uploaded unless you choose to [save the impacted schedule](#saving-the-impacted-schedule).

TIA does a different job from the Traceback method in [Delay Analysis](pages/delay-analysis.md). Traceback explains what a chain of *already-recorded* schedule changes did between two real uploaded updates. TIA tests a scenario that may not match any uploaded schedule: "if this event happened here, for this long, what would it do to the finish date?"

Open it from **[Delay Analysis](pages/delay-analysis.md)** in the left-hand navigation, where it's one of the two methods offered, alongside Traceback.

## Setting up

Select a **project** and **schedule** to analyse, and the **Target Activity**: the completion point you're measuring against. Logic+ detects it from the schedule (the last activity on its primary float path, or a finish milestone), and you can choose a different one from the dropdown.

The schedule needs a data date. If it doesn't have one, a **No data date** notice replaces the analysis until you set the date in [Schedule Manager](pages/schedule-manager.md#schedules-without-a-data-date).

The schedule opens as a **Full Network** Gantt chart. Build your fragnets in the tray at the bottom (see [below](#building-a-fragnet)), then click **Analyse Target** to calculate the impact.

Three settings control how the schedule recalculates. They're under the **Fragnet settings** button (the gear icon beside **Analyse Target**), and they default to the schedule file's own settings, which you can change. These are the same progressed-schedule and lag-calendar options P6 uses, so if you're not sure which your schedule was built with, keep the defaults, which already match the file.

**Scheduling** matters only for an activity that has started but not finished. It decides where the *remaining*, unworked part of that activity goes when the schedule recalculates:

| Option | What it does |
|---|---|
| Retain logic | The remaining work stays tied to the activity's original predecessor logic even once it's under way. Its position, and whether it still counts as driven by that predecessor, follows the network logic instead of where progress on site resumed. The trade-off is that the remaining work can end up in a position that doesn't match real progress. |
| Progress override | Once an activity is under way, its logic tie to its predecessor is ignored for the remaining work, which starts from the schedule's current progress point instead, whatever the original link implied. |
| Actual dates | Works like Progress override, ignoring logic once work starts, but if the schedule recorded a resume date for the remaining work, that date is used, never earlier than the schedule's own progress point. |

**Relationship lag** sets which calendar converts each link's lag (for example "start 5 days after") into elapsed time, because 5 days means something different on a 5-day-week calendar than on a 7-day one. It applies to every link in the network:

| Option | What it does |
|---|---|
| Predecessor calendar | Each link's lag is converted using the predecessor activity's own calendar. |
| Successor calendar | Each link's lag is converted using the successor activity's own calendar instead. |
| 24-hour calendar | Each link's lag is converted using a fixed round-the-clock, seven-day calendar, regardless of either activity's own working pattern. |
| Project default | Each link's lag is converted using the project's overall default calendar. |

**Calculate start-to-start lag from** sets whether, for an activity that has started, a start-to-start link's lag is counted from its **Early Start** or its **Actual Start**.

## Building a fragnet

You build fragnets in the tray at the bottom of the screen, on the **Fragnet Details** tab. The other tab is **Completion & Comparison** (see [Reading the result](#reading-the-result)). There are two ways to start a fragnet:

- Click **+ New Fragnet**.
- Use the add-fragnet button on an activity's row in the activity list (*Add fragnet after [activity ID]*). The new fragnet starts at that activity. This button is available in the **Full Network** and **Driving Path** views.

The tray lists your fragnets in a table with these columns:

| Column | What it is |
|---|---|
| **Include** | Tick to include the fragnet in the analysis. The tray header shows how many are included (e.g. *"2 fragnets included in the impact"*). |
| **ID Prefix** | Becomes the fragnet's ID and prefixes its activities: **ABC** gives ABC-1, ABC-2 and so on. Leave it blank to use the next available default prefix. |
| **Fragnet** | The fragnet's name. If you enter one, Logic+ creates a child WBS with that name under the predecessor's WBS; leave it blank to use the predecessor's WBS. |
| **Fragnet predecessor** / **Fragnet successor** | The existing schedule activities the new chain ties into. Search the dropdown by activity ID or name, **or** click into the field and then click an activity in the main Gantt chart. |
| **Action** | **Go to fragnet in schedule**, **Edit** (the pencil, which becomes a tick while you edit) and **Delete** (the bin, which asks you to confirm: *"Delete fragnet?"*). |

The **?** icon beside each column heading explains that field.

### Editing a fragnet's activities

Editing a fragnet opens a small Gantt chart under its row. It shows the predecessor and successor activities for reference, with your fragnet's activities between them. Its columns are **Activity ID**, **Activity name**, **Calendar**, **Duration (wd)**, the relationship to the next activity (Finish-to-Start, Start-to-Start, Finish-to-Finish or Start-to-Finish) and its lag in working days, and **Action**.

- Click **+** to add an activity. Enter a **name**, then choose a **calendar** from the schedule's own calendars, a **duration** (zero or a positive number of working days), and the **relationship** and **lag** to the next activity.
- Confirm a new or edited row with the tick, or by pressing Enter. Use the bin to discard a new row. Named rows also save when you click away.
- To overlap two activities, change a link to Start-to-Start or use a negative lag.

While you edit, the chart previews the fragnet. If something prevents the preview, such as a circular link, **Preview unavailable** explains why and **Save** is disabled.

When you're done, click **Save** to keep the fragnet or **Cancel** to discard your changes; cancelling a brand-new fragnet removes it. If you try to switch view, tab or schedule with unsaved edits, Logic+ asks **Discard fragnet changes?** (*"You have unsaved fragnet edits. Switching away will discard these changes."*), with **Discard changes** and **Keep editing**.

Your fragnets are stored in your browser, so they're still there when you come back to the same schedule on the same computer.

### Running the analysis

You can have several fragnets at once; tick **Include** on the ones you want in the result. Changes don't recalculate straight away, so edit as much as you like, then click **Analyse Target** to update the results. While changes are waiting, a message reads *"Changes ready to analyse. Select Analyse Target to update results."* If a fragnet activity has an invalid duration, it reads *"Fix invalid durations before analysing."* and the analysis won't run until you fix it.

## Reading the result

Open the **Completion & Comparison** tab. A strip of three figures shows:

| Figure | Meaning |
|---|---|
| **Original target finish** | The target activity's finish before any fragnet is inserted. If the schedule's own dates can't be used as a starting point, this reads **Without fragnet delays** instead: the target finish when the schedule is recalculated with no fragnets. |
| **Target finish after fragnets** | The target's finish with the included fragnets inserted |
| **Target movement** | The change, in calendar days (**cd**). Underneath, **24/7 calendar** notes the days are calendar days, **no impact applied** shows when no fragnet is included, and **target unavailable** shows when the target can't be calculated |

Any warnings from the calculation are listed below the strip.

## Schedule impact views

Four view tabs sit above the chart:

| View | Shows |
|---|---|
| **Full Network** | Every activity in the schedule. This is the view you start in. |
| **Driving Path** | The activities on the driving path to the target, with your fragnets. Clicking **Analyse Target** switches to this view. |
| **Moved Only** | Only the activities whose dates changed. Their previous positions show as baseline bars, with lines showing the movement to the impacted dates. |
| **Charged** | The delay charged to each affected activity |

A **legend** (the info button on the chart toolbar) explains the colours in the current view. The chart toolbar also has **Configure columns**, fullscreen and a [print preview](pages/gantt-viewer.md#printing).

**If the impact calculation can't complete**, for example because of a data or logic issue in the schedule, the **Full Network** and **Driving Path** views still show the original, unimpacted schedule.

## Comparing fragnets

Also on the **Completion & Comparison** tab, **Compare Selected Fragnets** shows a chart and a table with one row per included fragnet:

- **Gross movement (cd)**: the movement that fragnet causes on its own. If it depends on links to other fragnets, those upstream fragnets are included too.
- **Net movement (cd)**: the completion movement removed when that fragnet's duration and lag are set to zero, with its links kept in place. This is its own contribution with every included fragnet combined.
- A **drives target** tag on whichever fragnet has the largest net effect.
- An **All together** row with the combined movement. Beside it, a **dependent fragnets** tag shows when one fragnet relies on another's links; their gross figures share those dependencies, so **don't add them together**. Otherwise, a **concurrency** tag shows when the combined effect is less than the sum of the individual (gross) effects, which means their impacts overlap in time.

Because concurrent delays overlap, the net figures **won't always add up** to the combined movement, so the table shows the total alongside them.

## Saving the impacted schedule

Once you've run the analysis, click **Save schedule** (the disk icon beside **Analyse Target**) to keep the schedule *with your fragnets inserted*. In the **Save schedule** dialog, enter a **Filename**, then choose where it goes:

- **Save to Logic+ project** uploads it to the project as a new schedule, like any other upload, so it needs the **Edit** role and is processed before it's ready. It's unticked by default.
- **Save to external [format]** downloads it to your computer. It's ticked by default.

The file is saved in the same format as the schedule you analysed: **XER** (P6), **PP** (Asta Powerproject) or **XML** (MSPDI). You can't save a Microsoft Project **.mpp** file this way; the button is disabled, with a tip pointing you to download the original from Schedule Manager. The button is also disabled until the analysis has been run and is up to date.

## Calculation and other logic

**Data used:** the selected schedule packet only: its activities, relationships and calendars. No separate upload or comparison schedule is needed, because a fragnet is inserted into, and compared against, this one schedule.

**How it's calculated:** Logic+ recalculates the schedule twice with a full forward and backward pass, once as it is and once with the fragnet's activities and its two connecting relationships added, using the **Fragnet settings** above. For P6 schedules (.xer), the recalculation matches P6's own handling of calendars, milestones, progress, constraints and as-late-as-possible activities, so the as-is result lines up with the imported dates. Asta Powerproject (.pp) schedules are recalculated with Asta's own scheduling rules, including how summary activities take their dates. Level of Effort and WBS Summary activities take their dates from the recalculated activities they span, and an activity with no duration is treated as zero. Practical Completion is read from the completion activity's calculated finish in each version, and the movement is the difference between the two, in both calendar days and working days on the schedule's reference calendar.

**Gross, net and concurrency:** with more than one fragnet included, "gross" analyses each fragnet against the unmodified schedule, together with any other fragnets it's linked to and depends on. "Net" is the movement removed when that one fragnet's duration and lag are set to zero, with its links left in place, so each fragnet's net figure is its own contribution. When fragnets compete for the same window of delay, the net figures can add up to less than the combined movement. Concurrency is flagged whenever the combined effect is less than the sum of the individual gross effects.
