---
page-id: glossary
title: Glossary
audience: external
status: draft
version: 1.6.0
last-reviewed: 2026-11-23
blocked-reason: 2026-11-23 – Added Fragnet and Practical Completion (PC); Data Date notes the Not set flag (LUSB-1392); dropped the title-repeating heading. Humanizer rewrite of the prose. 2026-11-13 – EPS Admin replaced by Project Admin; added App Owner (v1.2.36); Archive now covers folders and projects. 2026-10-28 – 2026-10-28 – Added Archive, Folder and the four access roles (permissions release, PR #529; schedule archiving, PR #551). Earlier: Schedule/XER entries updated 2026-09-23 – were XER-only, now reflect the four accepted schedule formats (shipped 2026-09-17).
---

Key terms used in Logic+ Online and in this documentation.

**Activity**
A single task in a schedule, defined by an ID, name, start date, finish date and type. Primavera P6 calls these activities; Microsoft Project calls them tasks.

**App Owner**
The person who owns the organisation's account. They manage billing, seats and all projects, and can hand ownership to someone else. Not the same as the folder and project **Owner** role. See [Subscription & Billing](pages/subscription-billing.md#the-licensing-model).

**Archive**
Moving a schedule, project or folder out of everyday use without deleting it. Archived schedules go to **Archived files**, and archived projects and folders go to **Archived items** (Project Admins only). You can undo archiving with **Restore**, but not permanent deletion. See [Schedule Manager](pages/schedule-manager.md#archived-files) and [All projects](pages/all-projects.md#archiving-and-restoring).

**Baseline**
An earlier snapshot of the schedule used as a reference point. Comparing the current schedule with a baseline shows delays, shifts and changes to the critical path.

**Bow Wave**
A picture of work volume across the project timeline. A bow wave builds as work accumulates and releases as activities complete. A wave that keeps pushing forward without releasing can indicate schedule pressure.

**Category**
A classification applied to activities in the schedule, used in Logic+ to group and compare activity data.

**Critical Path**
The longest sequence of dependent activities, which sets the project's earliest possible finish date. Any delay to an activity on the critical path delays the project's end date.

**Data Date**
The date the schedule snapshot was recorded. All progress in the schedule is measured as of the data date. Analyses need it, so a schedule without one is flagged **Not set** in Schedule Manager.

**Float (Total Float)**
How long an activity can be delayed without delaying the project's end date. Zero float means the activity is on the critical path.

**Folder**
An optional container for organising projects and other folders. Access given on a folder applies to everything inside it.

**Fragnet**
A small chain of activities that represents a delay event or new scope. In Time-Impact Analysis you build fragnets and insert them into the schedule to test their effect; in Traceback results, new scope found in the schedule is charged as **New fragnet**. See [Time-Impact Analysis](pages/time-impact-analysis.md).

**Level of Effort (LOE)**
An activity type for work that runs across a span of time instead of producing a specific deliverable, such as project management or safety monitoring. Logic+ shows LOE activities with a distinct bar style in the Gantt viewer.

**Owner / Editor / Viewer**
Access roles given on a folder or project, shown as **Owner**, **Edit** and **View**. Viewers look and analyse, Editors work on schedules, and Owners can also move, archive and manage access. See [Permissions and Access](pages/permissions.md).

**Practical Completion (PC)**
The completion point that delay analysis measures against, usually the project's completion milestone. Delay attribution charges each day of movement at PC to a cause. See [Delay Attribution Scenarios](pages/delay-attribution-2.md).

**Project**
A named container in Logic+ for one or more schedule files. A Logic+ project usually corresponds to a single construction project or programme.

**Project Admin**
An organisation-wide access role with Owner-level control over every folder and project. Only Project Admins can create, rename, move or archive top-level folders, and only they see **Archived items** and **Archived files**. See [Permissions and Access](pages/permissions.md).

**Schedule**
An uploaded schedule file (`.xer`, `.mpp`, `.pp`, or `.xml`) containing a snapshot of a project programme at one point in time.

**Traceback**
The method in Delay Analysis that identifies the chain of activities most responsible for project delay. Working back from an end activity, it scores candidate predecessors at each step and picks the strongest. You choose an algorithm profile (**Default** unless you change it), which sets how candidates are chosen. See [Traceback Setup](pages/traceback-setup.md) and [How the Traceback method works](pages/traceback-engine.md).

**WBS (Work Breakdown Structure)**
A hierarchy that groups activities to organise and summarise schedule data. WBS codes appear as summary rows in the Gantt viewer.

**XER**
The native export format of Primavera P6, one of the four schedule formats Logic+ accepts, alongside `.mpp`, `.pp` and `.xml`. See [Schedule Manager](pages/schedule-manager.md).
