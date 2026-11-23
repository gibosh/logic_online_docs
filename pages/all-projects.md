---
page-id: all-projects
route: /projects
title: All Projects
audience: external
status: draft
version: 1.4.0
last-reviewed: 2026-11-23
blocked-reason: 2026-11-23 – Corrected against ProjectsPage.tsx: project rows have icon buttons (access, Open Schedule Manager - needs a seat, Edit project, Archive project), only folders have the ... menu (Manage/View access, Edit folder, Add subfolder, Archive folder). Humanizer rewrite of the prose. 2026-11-13 – Archiving (PR #555 / LUSB-1364, v1.2.36): Delete folder / Delete project are now Archive folder / Archive project, with an Archived items dialog for Project Admins (restore, permanently delete). Removed the "only empty folders can be deleted" rule. EPS Admin renamed Project Admin. Verified against ProjectsPage.tsx and ArchivedProjects.tsx at v1.2.40; not screenshot-verified. Logs is shown to a Project Admin who holds a licence (hasSeat in ProjectsPage.tsx). 2026-10-28 – 2026-10-28 – Permissions release (PR #529/#545): new Access and Author columns, access dialog, no-access landing page, Logs button. "Group" is now "Folder" throughout (New folder / Add subfolder / Edit folder / Delete folder) – this page still said group. Single-click no longer selects a project; double-click or Open Schedule Manager opens it. Verified against ProjectsPage.tsx, ProjectDragRows.tsx, LogsPage.tsx, not screenshot-verified. Sidebar Project selector behaviour not re-checked. Earlier note: New page – Schedule Manager and All projects split into two genuinely different screens this pull (feature/table-backed-projects). Content verified directly against ProjectsPage.tsx source, not yet screenshot-verified live. Drag-and-drop project reassignment (`ProjectDragRows.tsx`, `@dnd-kit/core`) added 2026-09-24, also verified directly against source, not yet screenshot-verified live.
---

All projects is where you organise your projects: create them, put them in folders and open the one you want to work in. Open it from **All projects** in the left-hand navigation, below the divider under Schedule Manager.

Once you've opened a project, use **[Schedule Manager](pages/schedule-manager.md)** in the sidebar to upload and manage its schedule files.

What you see and can change here depends on your access. See **[Permissions and Access](pages/permissions.md)**. Buttons you don't have permission to use are greyed out, and folders and projects you have no access to aren't shown.

## Key concepts

**Project**: a named container for one or more schedule files, identified everywhere in Logic+ by a short **project code**.

**Folder**: an optional way to organise projects. Folders can sit inside other folders, and a project belongs to one folder or to none.

**Project code**: a 2-character code (letters, numbers or both) shown as a badge next to every project's name. It also appears in the page address while you work inside the project, for example `SO` for a project called "Solar Farm East". Logic+ suggests a code from the project name when you create a project, and you can edit the suggestion. **Once a project is created, its code can't be changed.**

## Browsing and searching

The table has these columns: **Name**, **Access**, **Count**, **Author** and **Actions**. **Access** shows your role on each folder and project (**View**, **Edit**, **Owner** or **Project Admin**). **Count** shows how many projects a folder contains, including those in nested subfolders, and how many schedule files a project has. **Author** shows who created the item. Projects with no folder appear separately under **Unassigned projects**.

Use the **Search projects** box to filter by project name or code. The table then shows only the folders and projects that match, with any folder containing a match expanded.

Click a folder's arrow to expand or collapse it, or use **Expand all** or **Collapse all** above the table.

**Drag and drop to move a project.** Drag a project row onto a folder's row to move it there. The folder expands so you can see the project land. Drop it onto **Unassigned projects** to take it out of its folder. If the move fails, for example because of a network error or because you lack access at the destination, a message explains why. Moving needs the **Owner** role on the project and **Edit** or above at the destination.

## Opening a project

**Double-click a project's row**, or click its **Open Schedule Manager** button (the arrow icon), to go to that project's Schedule Manager. The project-scoped items in the sidebar (Schedule Viewer, Delay Analysis and Analytics) then work on that project. Opening a project needs a licence seat; without one, the button is disabled with the hint "A seat is required to open schedules".

## Creating a folder

Click **New folder**. Give it a **Name**, and optionally choose a parent in the **Folder** field. Leave it as **None** for a top-level folder.

If you've clicked a folder in the table, **New folder** and **New project** put the new item inside it. A line above the table says *"New items go into [folder]"*, and **Clear selection** takes you back to the top level. Only a **Project Admin** can create top-level folders.

Each folder row has an actions menu (**...**) with these options:

- **Edit folder** renames the folder or moves it to another parent.
- **Add subfolder** creates a new folder inside it.
- **Archive folder** asks you to confirm (**Archive [name]?**) that you want to archive the folder and all the folders, projects and active schedules inside it. Archiving needs the **Owner** role on the folder. See [Archiving and restoring](#archiving-and-restoring).

## Creating a project

Click **New project**, or **Add project** on a folder's row to create the project inside that folder. Give it a **Name**. Logic+ suggests a **Project code** as you type, and you can overwrite it before saving. Optionally choose a **Folder** to file it under. The default, **None**, puts it under Unassigned projects. You become the project's **Owner**.

Each project row has icon buttons for its actions (hover for the name):

- **Edit project** (the pencil) renames the project or moves it to a different folder. The project code is fixed once the project is created.
- **Archive project** (the archive-box icon) asks you to confirm (**Archive [name]?**) that you want to archive the project and its active schedules. Archiving needs the **Owner** role, and nothing is lost: see [Archiving and restoring](#archiving-and-restoring).

## Archiving and restoring

Archiving takes a folder or project out of everyday use without destroying it. Archived folders and projects are hidden from the table and can't be opened or edited. Archiving a folder archives everything inside it: subfolders, projects and their active schedules. Schedules that were archived separately beforehand stay archived on their own.

**Project Admins** see an **Archived items** button at the top of the page, which opens **Archived folders and projects**, an expandable tree of everything archived.

- **To restore:** tick folders or projects (ticking a folder includes what's inside it) and click **Restore selected (*N*)**. Restoring a project also restores the folders above it and the schedules that were archived with it. Schedules archived independently stay in [Archived files](pages/schedule-manager.md#archived-files).
- **To delete permanently:** tick one or more branches and click **Permanently delete selected (*N*)**. Only **fully selected** branches are deleted. A folder you've only partly ticked is kept, and a note tells you so. A confirmation (**Permanently delete [N] items?**) lists the names and warns: *"This permanently deletes the selected folders and projects, including all their schedules—even independently archived schedules. This cannot be undone."*
- **Select all** ticks everything, and **Refresh** reloads the list.

You can undo archiving but not permanent deletion. Only Project Admins can restore or permanently delete folders and projects.

## Managing access

Each folder and project has an access button that opens the list of who can see it. It reads **Manage access** if you're an Owner or Project Admin, and **View access** otherwise. On a project it's the people icon in the row; on a folder it's in the actions menu. See **[Permissions and Access](pages/permissions.md)** for the roles and how to add people.

## Logs

Project Admins see a **Logs** button at the top of this page. It opens **Organisation logs**, a record of folder and project actions (added, updated and removed) with **When**, **Who**, **Action** and **Name** columns. **Download TXT** saves the log as a text file, and the **Schedule logs** link switches to the schedule-level record (see [Schedule Manager](pages/schedule-manager.md#schedule-logs)).

## When there's nothing to show

- **"Create a project or folder to get started."** You have access, but nothing has been created yet.
- **"No matching projects."** Your search matched nothing.
- **"Nothing has been shared with you yet"** Your organisation hasn't given you access to anything yet. The page lists your Project Admins to contact; see [Permissions and Access](pages/permissions.md#if-you-cant-see-anything).
- **"Unable to load permissions. Changes are disabled."** Logic+ couldn't check your access, so editing is turned off. Use **Retry permissions**.
