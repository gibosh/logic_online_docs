---
page-id: glossary
title: Glossary
audience: external
status: draft
version: 1.1.0
last-reviewed: 2026-09-03
---

## Glossary

Key terms used in Logic+ Online and in this documentation.

---

**Activity**
A single task in a schedule. Defined by an ID, name, start date, finish date, and type. In Primavera P6 these are called activities; in Microsoft Project they are called tasks.

**Baseline**
An earlier snapshot of the schedule used as a reference point. Comparing the current schedule against a baseline reveals delays, shifts, and changes to the critical path.

**Bow Wave**
A representation of work volume across the project timeline. A bow wave builds as work accumulates and releases as activities complete. A wave that pushes forward without releasing can indicate schedule pressure.

**Category**
A classification applied to activities in the schedule. Categories are used in Logic+ to group and compare activity data across views such as the Category Float Matrix and Category Critical Path.

**Critical Path**
The longest sequence of dependent activities that determines the project's earliest possible finish date. Any delay to a critical path activity delays the project end date.

**Data Date**
The date at which the schedule snapshot was recorded. All progress values in the schedule are measured as of the data date.

**Float (Total Float)**
The amount of time an activity can be delayed without delaying the project end date. Zero float = on the critical path.

**Level of Effort (LOE)**
An activity type representing work that runs across a span of time rather than producing a specific deliverable (e.g. project management, safety monitoring). Logic+ displays LOE activities with a distinct bar style in the Gantt viewer.

**Packet**
The unit Logic+ breaks an uploaded schedule into internally so the analytics pipeline can process it. This is a backend/engineering term, not a status you'll see on screen – the upload status badge itself just reads **processing**, then **processed**.

**Project**
A named container in Logic+ that groups one or more schedule files. A Logic+ project typically corresponds to a single construction project or programme.

**Schedule**
An uploaded `.xer` file containing a point-in-time snapshot of a project programme.

**Slack (Total Slack)**
Similar to total float but calculated differently in some schedule engines. Logic+ records both where available.

**Traceback**
An algorithm that identifies the chain of activities most responsible for project delay, by scoring candidate predecessor activities at each step back from the completion milestone and picking the strongest chain. Logic+ offers a choice of scoring profile (Replica v2.1 is the default) that weights the scoring criteria differently – see [Traceback Setup](pages/traceback-setup.md) and [How Traceback and Delay Attribution Work](pages/traceback-engine.md).

**WBS (Work Breakdown Structure)**
A hierarchical grouping of activities used to organise and summarise schedule data. WBS codes appear as summary rows in the Gantt viewer.

**XER**
The native export format of Primavera P6. Logic+ uses XER files as its primary input format.
