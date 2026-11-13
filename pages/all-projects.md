---
page-id: all-projects
route: /projects
title: All Projects
audience: external
status: draft
version: 1.3.0
last-reviewed: 2026-11-13
blocked-reason: 2026-11-13 – Archiving (PR #555 / LUSB-1364, v1.2.36): Delete folder / Delete project are now Archive folder / Archive project, with an Archived items dialog for Project Admins (restore, permanently delete). Removed the "only empty folders can be deleted" rule. EPS Admin renamed Project Admin. Verified against ProjectsPage.tsx and ArchivedProjects.tsx at v1.2.40; not screenshot-verified. Logs is shown to a Project Admin who holds a licence (hasSeat in ProjectsPage.tsx). 2026-10-28 – 2026-10-28 – Permissions release (PR #529/#545): new Access and Author columns, access dialog, no-access landing page, Logs button. "Group" is now "Folder" throughout (New folder / Add subfolder / Edit folder / Delete folder) – this page still said group. Single-click no longer selects a project; double-click or Open Schedule Manager opens it. Verified against ProjectsPage.tsx, ProjectDragRows.tsx, LogsPage.tsx, not screenshot-verified. Sidebar Project selector behaviour not re-checked. Earlier note: New page – Schedule Manager and All projects split into two genuinely different screens this pull (feature/table-backed-projects). Content verified directly against ProjectsPage.tsx source, not yet screenshot-verified live. Drag-and-drop project reassignment (`ProjectDragRows.tsx`, `@dnd-kit/core`) added 2026-09-24, also verified directly against source, not yet screenshot-verified live.
---

## About this page

All projects is the home base for organising your projects – create them, put them in folders, and open the one you want to work in. Open it from **All projects** in the left-hand navigation, below the divider under Schedule Manager.

Once you've opened a project here, use **[Schedule Manager](pages/schedule-manager.md)** in the sidebar to upload and manage that project's schedule files.

What you see and can change here depends on your access – see **[Permissions and Access](pages/permissions.md)**. Buttons you don't have permission to use are greyed out, and folders and projects you have no access to aren't shown.

## Key concepts

**Project** – a named container that groups one or more schedule files together, identified everywhere in Logic+ by a short **project code**.

**Folder** – an optional way to organise projects. Folders can be nested inside other folders. A project can belong to one folder, or none.

**Project code** – a 2-character code (letters and/or numbers) shown as a badge next to every project's name, and used in the page address whenever you're working inside that project (e.g. `SO` for a project called "Solar Farm East"). Logic+ suggests a code automatically from the project name when you create a project – edit it if you'd prefer something else. **Once a project is created, its code can't be changed.**

## Browsing and searching

Projects are listed in a table with these columns: **Name**, **Access**, **Count**, **Author** and **Actions**. The **Access** column shows your role on each folder and project (**View**, **Edit**, **Owner** or **Project Admin**). **Count** shows how many projects a folder contains (including those in nested subfolders) and how many schedule files a project has. **Author** shows who created it. Projects with no folder appear separately under **Unassigned projects**.

Use the **Search projects** box to filter by project name or code. Searching also expands any folder containing a match, and shows only the folders and projects that match.

Click a folder's arrow to expand or collapse it, or use **Expand all** / **Collapse all** above the table.

**Drag and drop to move a project.** Drag any project row and drop it onto a folder's row to move it there – the target folder expands automatically so you can see it land. Drop it onto **Unassigned projects** to take it out of its folder. If the move fails (a network error, or you don't have the access needed at the destination), a message explains why rather than silently reverting. Moving needs the **Owner** role on the project and **Edit** or above at the destination.

## Opening a project

**Double-click a project's row**, or open its actions menu and choose **Open Schedule Manager**, to go to that project's Schedule Manager. From there the project-scoped items in the sidebar (Schedule Viewer, Delay Analysis, Analytics) work on that project.

## Creating a folder

Click **New folder**. Give it a **Name**, and optionally choose a parent in the **Folder** field (leave it as **None** for a top-level folder).

If you've clicked a folder in the table, **New folder** and **New project** put the new item inside it – a line above the table says *"New items go into [folder]"*, with **Clear selection** to go back to the top level. Only a **Project Admin** can create top-level folders.

**To rename or move a folder:** open its actions menu (**...**) and choose **Edit folder**.

**To nest a new folder inside an existing one:** open the parent folder's actions menu and choose **Add subfolder**.

**To archive a folder:** open its actions menu and choose **Archive folder**. A confirmation (**Archive [name]?**) explains that this archives the folder and all its descendant folders, projects and active schedules. Archiving needs the **Owner** role on the folder. See [Archiving and restoring](#archiving-and-restoring).

## Creating a project

Click **New project** (or **Add project** on a specific folder's row, to create it already inside that folder). Give it a **Name** – Logic+ suggests a **Project code** automatically as you type, which you can overwrite before saving. Optionally choose a **Folder** to file it under (default: **None**, meaning it appears under Unassigned projects). You become the project's **Owner**.

**To rename a project or move it to a different folder:** open its actions menu (**...**) and choose **Edit project**. The project code can't be changed here – it's fixed once the project is created.

**To archive a project:** use its **Archive project** action (the archive-box icon in the project's row actions). A confirmation (**Archive [name]?**) explains that this archives the project and its active schedules. Archiving needs the **Owner** role. Nothing is lost – see [Archiving and restoring](#archiving-and-restoring).

## Archiving and restoring

Archiving takes a folder or project out of everyday use without destroying it. Archived folders and projects are hidden from the table and can't be opened or edited. Archiving a folder archives everything inside it – subfolders, projects and their active schedules. Schedules that were archived separately beforehand stay archived on their own.

**Project Admins** see an **Archived items** button at the top of the page, which opens **Archived folders and projects** – an expandable tree of everything archived.

- **To restore:** tick folders or projects (ticking a folder includes what's inside it) and click **Restore selected (*N*)**. Restoring a project also restores the folders above it, and the schedules that were archived along with it. Schedules archived independently stay in [Archived files](pages/schedule-manager.md#archived-files).
- **To delete permanently:** tick one or more branches and click **Permanently delete selected (*N*)**. Only **fully selected** branches are deleted – a folder you've only partly ticked is kept, and a note tells you so. A confirmation (**Permanently delete [N] items?**) lists the names and warns: *"This permanently deletes the selected folders and projects, including all their schedules – even independently archived schedules. This cannot be undone."*
- **Select all** ticks everything; **Refresh** reloads the list.

Archiving is reversible; permanent deletion isn't. Only Project Admins can restore or permanently delete folders and projects.

## Managing access

Each folder and project has an access button (**Manage access** if you're an Owner or Project Admin, **View access** otherwise) that opens the list of who can see it. See **[Permissions and Access](pages/permissions.md)** for roles and how to add people.

## Logs

Project Admins see a **Logs** button at the top of this page, opening **Organisation logs** – a record of folder and project actions (added, updated and removed), with **When**, **Who**, **Action** and **Name** columns. Use **Download TXT** to save the log as a text file. A **Schedule logs** link switches to the schedule-level record (see [Schedule Manager](pages/schedule-manager.md#schedule-logs)).

## When there's nothing to show

- **"Create a project or folder to get started."** – you have access, but nothing has been created yet.
- **"No matching projects."** – your search matched nothing.
- **"Nothing has been shared with you yet"** – your organisation hasn't given you access to anything. The page lists your Project Admins to contact; see [Permissions and Access](pages/permissions.md#if-you-cant-see-anything).
- **"Unable to load permissions. Changes are disabled."** – Logic+ couldn't check your access, so editing is turned off. Use **Retry permissions**.
