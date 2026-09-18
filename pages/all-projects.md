---
page-id: all-projects
route: /projects
title: All Projects
audience: external
status: draft
version: 1.0.0
last-reviewed: 2026-09-18
blocked-reason: New page – Schedule Manager and All projects split into two genuinely different screens this pull (feature/table-backed-projects). Content verified directly against ProjectsPage.tsx source, not yet screenshot-verified live.
---

## About this page

All projects is the home base for organising your projects – create them, group them, and choose which one you're currently working in. Open it from **All projects** in the left-hand navigation, below the divider under Schedule Manager.

Once you've selected a project here, use **[Schedule Manager](pages/schedule-manager.md)** in the sidebar to upload and manage that project's schedule files.

## Key concepts

**Project** – a named container that groups one or more schedule files together, identified everywhere in Logic+ by a short **project code**.

**Group** – an optional way to organise projects, similar to a folder. Groups can be nested inside other groups. A project can belong to one group, or none.

**Project code** – a 2-character code (letters and/or numbers) shown as a badge next to every project's name, and used in the page address whenever you're working inside that project (e.g. `SO` for a project called "Solar Farm East"). Logic+ suggests a code automatically from the project name when you create a project – edit it if you'd prefer something else. **Once a project is created, its code can't be changed.**

## Browsing and searching

Projects are listed in a table, grouped under their group (if any); projects with no group appear separately under **Unassigned projects**. Each group row shows how many projects it contains (counting projects in any nested subgroups too); each project row shows its code, name, and how many schedule files it has.

Use the **Search projects** box to filter by project name or code. Searching also expands any group containing a match, and shows only the groups/projects that match.

Click the arrow next to a group's name to expand or collapse it.

## Selecting a project

**Click anywhere on a project's row** to make it your current project. This doesn't take you anywhere – it updates the **Project** selector at the top of the sidebar and enables the project-scoped items below it (Schedule Viewer, Delay Analysis, Analytics, Schedule Manager). To actually work with that project, click one of those sidebar items next, typically **Schedule Manager** to upload or manage its schedules.

## Creating a group

Click **New group**. Give it a **Name**, and optionally a **Parent group** to nest it inside an existing group (leave as **No parent (top level)** for a top-level group).

**To rename a group or move it:** open its actions menu (**...**) and choose **Edit group**.

**To nest a new group inside an existing one:** open the parent group's actions menu and choose **Add subgroup**.

**To delete a group:** open its actions menu and choose **Delete group**. A confirmation dialog warns that projects inside the group will be deleted too, and any direct subgroups will be moved up to the top level – read it carefully before confirming.

## Creating a project

Click **New project** (or **Add project** on a specific group's row, to create it already inside that group). Give it a **Name** – Logic+ suggests a **Project code** automatically as you type, which you can overwrite before saving. Optionally choose a **Group** to file it under (default: **No group**, meaning it appears under Unassigned projects).

**To rename a project or move it to a different group:** open its actions menu (**...**) and choose **Edit project**. The project code can't be changed here – it's fixed once the project is created.

**To delete a project:** open its actions menu and choose **Delete project**. A confirmation dialog warns that this removes the project and its schedule files – read it carefully before confirming.

## Note

An empty state (**"Create a project or group to get started"**) is shown when you have no projects yet. Searching for something that doesn't match anything shows **"No matching projects."** instead of an empty list.
