---
page-id: glossary
title: Glossary
audience: external
status: draft
version: 1.5.0
last-reviewed: 2026-11-13
blocked-reason: 2026-11-13 – EPS Admin replaced by Project Admin; added App Owner (v1.2.36); Archive now covers folders and projects. 2026-10-28 – 2026-10-28 – Added Archive, Folder and the four access roles (permissions release, PR #529; schedule archiving, PR #551). Earlier: Schedule/XER entries updated 2026-09-23 – were XER-only, now reflect the four accepted schedule formats (shipped 2026-09-17).
---

## Glossary

Key terms used in Logic+ Online and in this documentation.

---

**Activity**
A single task in a schedule. Defined by an ID, name, start date, finish date, and type. In Primavera P6 these are called activities; in Microsoft Project they are called tasks.

**App Owner**
The person who owns the organisation's account: manages billing, seats and all projects, and can hand ownership to someone else. Not the same as the folder and project **Owner** role. See [Subscription & Billing](pages/subscription-billing.md#the-licensing-model).

**Archive**
Moving a schedule, project or folder out of everyday use without deleting it. Archived schedules go to **Archived files**; archived projects and folders go to **Archived items** (Project Admins only). Archiving can be undone with **Restore**; permanent deletion can't. See [Schedule Manager](pages/schedule-manager.md#archived-files) and [All projects](pages/all-projects.md#archiving-and-restoring).

**Baseline**
An earlier snapshot of the schedule used as a reference point. Comparing the current schedule against a baseline reveals delays, shifts, and changes to the critical path.

**Bow Wave**
A representation of work volume across the project timeline. A bow wave builds as work accumulates and releases as activities complete. A wave that pushes forward without releasing can indicate schedule pressure.

**Category**
A classification applied to activities in the schedule, used in Logic+ to group and compare activity data.

**Critical Path**
The longest sequence of dependent activities that determines the project's earliest possible finish date. Any delay to a critical path activity delays the project end date.

**Data Date**
The date at which the schedule snapshot was recorded. All progress values in the schedule are measured as of the data date.

**Float (Total Float)**
The amount of time an activity can be delayed without delaying the project end date. Zero float = on the critical path.

**Folder**
An optional container for organising projects and other folders. Access you give on a folder flows down to everything inside it.

**Level of Effort (LOE)**
An activity type representing work that runs across a span of time rather than producing a specific deliverable (e.g. project management, safety monitoring). Logic+ displays LOE activities with a distinct bar style in the Gantt viewer.

**Owner / Editor / Viewer**
Access roles (shown as **Owner**, **Edit** and **View**) given on a folder or project. Viewers look and analyse, Editors work on schedules, Owners also move, archive and manage access. See [Permissions and Access](pages/permissions.md).

**Project**
A named container in Logic+ that groups one or more schedule files. A Logic+ project typically corresponds to a single construction project or programme.

**Project Admin**
An organisation-wide access role with Owner-level control over every folder and project. The only role that can create, rename, move or archive top-level folders, and the only one that sees **Archived items** and **Archived files**. See [Permissions and Access](pages/permissions.md).

**Schedule**
An uploaded schedule file (`.xer`, `.mpp`, `.pp`, or `.xml`) containing a point-in-time snapshot of a project programme.

**Traceback**
An algorithm that identifies the chain of activities most responsible for project delay, by scoring candidate predecessor activities at each step back from the completion milestone and picking the strongest chain. It's the method behind Delay Analysis's Traceback method specifically – Logic+ offers a choice of scoring profile (**Default** is the default) that weights the scoring criteria differently – see [Traceback Setup](pages/traceback-setup.md) and [How the Traceback method works](pages/traceback-engine.md).

**WBS (Work Breakdown Structure)**
A hierarchical grouping of activities used to organise and summarise schedule data. WBS codes appear as summary rows in the Gantt viewer.

**XER**
The native export format of Primavera P6, one of four schedule formats Logic+ accepts (alongside `.mpp`, `.pp`, and `.xml` – see [Schedule Manager](pages/schedule-manager.md)).
