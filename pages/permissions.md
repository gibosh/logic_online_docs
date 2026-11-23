---
page-id: permissions
route: /projects
title: Permissions and Access
audience: external
status: draft
version: 1.1.0
last-reviewed: 2026-11-13
blocked-reason: 2026-11-13 – Renamed per v1.2.36 (PR #555/#557): EPS Admin is now "Project Admin" and the organisation-level Owner is now "App Owner" (the earlier note below that the "Project admin" name did not ship is superseded – it did ship, as "Project Admin"). Added archiving (PR #551/#555): folders and projects are archived, not deleted, and Project Admins restore or permanently delete them from Archived items. Removed "Only empty folders can be deleted" (archiving a folder now takes its contents with it). Who can set Project Admin changed: App Owners and Project Admins, no longer User Admins. Verified against RoleBadge.tsx, NoAccess.tsx, AdminRoleHelp.tsx, SeatManagement.tsx, ProjectsPage.tsx, ArchivedProjects.tsx at v1.2.40. Not screenshot-verified. Original 2026-10-28 note follows. New page, written 2026-10-28 from the shipped permissions release (PR #529, LUSB-1159, plus PR #545 / LUSB-1353) – verified against AccessDialog.tsx, RoleBadge.tsx, NoAccess.tsx, EpsAdminControl.tsx, SeatHandoverDialog.tsx, ProjectsPage.tsx, ScheduleManagerPage.tsx, devops/migration/006-resource-access.sql (the role/permission matrix) and backend/services/access/README.md. Not screenshot-verified live. Whether the 30-second refresh and the "directly shared items appear at the top level" behaviour are obvious to users in practice has not been checked live.
---

## About this page

Logic+ controls who can see and change each folder, project and schedule in your organisation. You're given a **role** on a folder or project, and that role decides what you can do there.

## The roles

You'll see these as badges in the **Access** column on [All projects](pages/all-projects.md).

| Role | Shown as | What it's for |
|---|---|---|
| **Viewer** | **View** | Look at schedules and run analysis. Can print and export what's on screen, but can't download schedule files. |
| **Editor** | **Edit** | Everything a Viewer can do, plus day-to-day project work: create folders and projects; upload, download, edit, exclude and archive schedules; and set up project display and custom fields. |
| **Owner** | **Owner** | Everything an Editor can do, plus the structural decisions: **move** or **archive** a folder or project, and **manage who has access**. |
| **Project Admin** | **Project Admin** | An organisation-wide role with Owner-level control over every folder and project. Only a Project Admin can create, rename, move or archive **top-level** folders, or see **Archived items**. A Project Admin doesn't need a licence to manage projects. |

> The **App Owner** on the Subscription page's seat list is a different thing: it marks the person who owns the *organisation's account and billing*, and it has nothing to do with the folder and project **Owner** role above. See [Subscription & Billing](pages/subscription-billing.md#the-licensing-model).

## How access flows down

- A role on a **folder** applies to everything inside it, including subfolders, projects and their schedules.
- A role on a **project** applies to all of that project's schedules.
- A *weaker* role further down never reduces access you already have from above. If you're an Editor on a folder, a View grant on one of its projects changes nothing.
- Removing a direct grant removes only that grant. Access inherited from a folder above stays.
- Folders and projects you have **no access** to aren't shown at all. If you have access to a project but not to the folder it sits in, the project appears at the top level of your list.
- **My Workspace**, your personal workspace, is yours alone and can't be shared.

## Seeing who has access

On [All projects](pages/all-projects.md) and in [Schedule Manager](pages/schedule-manager.md), use the access button. It reads **Manage access** if you're an Owner or Project Admin, and **View access** otherwise.

- **View access** opens a read-only list titled **Access to "[name]"**.
- **Manage access** opens **Manage access to "[name]"**.

The list, headed **People with access at this level**, shows only the people given access *on that folder or project*. People who inherit access from a folder above aren't listed.

## Giving someone access

1. Open **Manage access** for the folder or project.
2. Search by name or email and choose **Select a person**.
3. Choose their role (**Owner**, **Edit** or **View**) and add them.

You can add only people who already hold a licence in your organisation. To change someone's role, use the role dropdown beside their name. To remove them, use the bin button.

**At least one Owner must remain** on a folder or project. Logic+ won't let you remove or downgrade the last one.

## If you can't see anything

A new user whose organisation hasn't shared anything with them yet sees **"Nothing has been shared with you yet"**, with a **Your Project Admins** list of people to contact. If the organisation has no Project Admins yet, the page says **"Ask your App Owner to assign a Project Admin."**

## For App Owners and User Admins

### Making someone a Project Admin

Open **Manage seats** on the [Subscription](pages/subscription-billing.md) page. Each member has a **Project Admin** checkbox beside their Billing admin and User admin options. The **App Owner** and existing **Project Admins** can change it; a User admin who is neither can't. The last Project Admin's checkbox is disabled, so there's always at least one. You don't need a project licence yourself to do this.

### Removing a member or unassigning a licence

Removing someone, or taking their licence away, would leave anything they solely own without an Owner, so Logic+ shows an ownership handover first:

1. A dialog titled **Remove [name]?** or **Unassign licence from [name]?** lists the folders and projects they solely own. Each has a suggested replacement: the nearest Owner above it, or a Project Admin if there isn't one. Items someone else already co-owns need nothing.
2. Check or change each replacement (**Choose a licensed Owner**). If you're removing the last Project Admin, you must also **Choose a Project Admin**.
3. Click **Hand over and remove access**.

If the change is interrupted, it appears under **Pending seat changes**, where **Resume handover** finishes it safely.

## Good to know

- When permissions were introduced, the author of each existing folder and project became its Owner, so nothing was left without one.
- Your own access refreshes about every 30 seconds, so a newly granted or removed role can take up to 30 seconds to show.
- Folders and projects are archived rather than deleted. Archiving a folder also archives its subfolders, projects and active schedules, and archived items drop out of the normal lists. Project Admins can restore them, or delete them permanently, from **Archived items**. See [All projects](pages/all-projects.md#archiving-and-restoring).
- Moving an item needs two roles: Owner on the item, and Editor or above at the destination.
- Project Admins can see the activity log. See [All projects](pages/all-projects.md#logs).
