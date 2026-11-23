---
page-id: getting-started
title: Getting Started with Logic+ Online
audience: external
status: draft
version: 2.7.0
last-reviewed: 2026-11-13
blocked-reason: 2026-11-13 – EPS Admin renamed Project Admin (v1.2.36). 2026-10-28 – 2026-10-28 – Permissions are live (PR #529/#545): replaced the "permissions aren't available yet" note and the "shared with everyone" wording, added the empty landing page; groups are now folders; Schedule Manager entry mentions archiving. Verified against NoAccess.tsx and ProjectsPage.tsx. 2026-09-30 – Added "Choosing a workspace" after users created projects in their organisation's workspace by mistake. Verified against v1.2.17: TopMenu.tsx/TenantSelector.tsx (selector next to the logo, aria-label "Select workspace"), tenantSlice.ts ("My Workspace" is the personal tenant and listed first; reopens the last-used workspace per user per browser via localStorage, else an invited workspace, else the first; switching navigates to /projects). No per-project permissions today – `updated_project_permission` event type exists (backend/types/events.ts, since 2026-03) but nothing uses it, and project remove has no role check – PM confirmed permissions work is in progress; remove the note when it ships. Also corrected Logging in: self-service sign-up has been live since 2026-09-25 (pages/signing-up.md), so "no self-service sign-up" was wrong. "Prefer to watch?" video (added under Quick Start 2026-09-25) now embeds an unlisted YouTube video (https://youtu.be/Q-hTNdksTDs, channel @LWARELogicPlus) as of 2026-09-26 – tried SharePoint external-share hosting first (rejected: visible re-encoding quality loss), then self-hosted the file directly in this repo (rejected: ~53MB permanently bloating git history, plus slow load with no adaptive streaming), now YouTube (unlisted, so it's excluded from search but viewable/embeddable by anyone with the link – same privacy model as the site itself). The self-hosted file and its faststart fix are gone from this repo; kept below as a historical note in case another video ever gets self-hosted here and hits the same two bugs. Historical, for the self-hosted attempt: (1) the exported MP4 had its moov atom after the video data instead of before it, so browsers couldn't read duration/seek info – fixed with a lossless faststart remux (moov moved to front, stco/co64 chunk-offset tables patched by hand, no ffmpeg available). (2) A relative <source src="video/..."> inside a raw HTML <video> tag doesn't get path-rewritten by Docsify the way markdown images/links do – Docsify only rewrites its own markdown-syntax links, not literal embedded HTML – so the browser resolved it against the page's own location instead of the markdown file's folder, and 404'd. Also, this site is reverse-proxied under /docs/ on the production domain, so even a root-relative path would only work on one of the two hosting URLs. Fixed by hardcoding the full production URL (https://logicplus.smartcoro.com/docs/guides/video/...) – if this ever needs a relative path instead, remember raw HTML tags need it root-relative to wherever docsify's own index.html actually lives, not folder-relative like <img> is. Restructured 2026-09-25 as the single source of copy for beta-tester onboarding (this page + a PDF export of it, no separate fork) – added a "Logging in" section (previously missing entirely), refreshed and expanded the module overview against current app state, renamed "The workflow" to "Quick Start," and removed "Uploading multiple schedules"/"Supported file formats" as standalone sections – both were fully duplicated on pages/schedule-manager.md (Key Concepts, Uploading schedules) already, so they're now just a short parenthetical in Quick Start's upload step instead. The "Logging in" section is drafted from Auth0/infra source (__infra/tofu/prod/*.tf, LoginPage.tsx, GuardedRoute.tsx) – confirmed: login URL, no self-service sign-up, org-based access. NOT yet confirmed by the tech lead: the exact account-creation experience (does Auth0 send an automatic set-password email, or is that a manual step?) – deliberately kept out of this page rather than guessed, since getting that wrong would actively lock testers out. Also flag for engineering, not stated in this page: a user with no organisation membership sees no error at all after logging in (TenantSelector renders nothing) – covered here only by a blanket "contact support" line, per the PM's direction to keep this simple.
---

## What is Logic+ Online?

Logic+ Online is a web-based tool for analysing construction schedules. You upload your project's schedule files, and Logic+ runs a set of analytics on them covering schedule health, delay risk and how the critical path behaves.

## Logging in

Go to **https://logicplus.smartcoro.com/login** and click **Sign In**.

New to Logic+? You can start a trial yourself: see [Signing Up](pages/signing-up.md). If your organisation has invited you, use the link in your invitation email. If you can't log in, contact **support@lware.com**.

## Choosing a workspace

Your projects live in a **workspace**. The workspace selector at the top of the screen, next to the Logic+ logo, shows which one you're in:

- **My Workspace**: your personal space. Only you can see projects here.
- **Your organisation's workspace**: shown under your organisation's name if you're on a Team or Enterprise licence. You see only the folders and projects your organisation has given you access to.

A project belongs to the workspace it was created in, and the two never mix: a project in My Workspace doesn't appear in your organisation's workspace, and the reverse is also true. **Check the selector before you create a project.** Logic+ reopens the last workspace you used in that browser, and switching workspace takes you back to All Projects.

> **Note:** In your organisation's workspace, your **role** on each folder and project (**View**, **Edit** or **Owner**) decides what you can see and do. If you've just joined and see *"Nothing has been shared with you yet"*, ask one of the **Project Admins** listed on that page to give you access. See [Permissions and Access](pages/permissions.md). For a project that's just for you, create it in **My Workspace**.

## Quick Start

1. **Create a project.** Check you're in the right **workspace** (see above), then add a project in [All projects](pages/all-projects) to hold your schedule files. You can put it in a folder.
2. **Upload a schedule.** Select the project, open [Schedule Manager](pages/schedule-manager) and drag in a schedule file (`.xer`, `.mpp`, `.pp` or `.xml`). Upload more than one to compare updates.
3. **Wait for processing.** The status shows "Processing…" while Logic+ reads the file, then "Ready for analysis".
4. **Explore the views.** Use the left-hand navigation to move between modules.

**Prefer to watch?** A short video covers the same steps:

<iframe width="100%" style="max-width:640px;aspect-ratio:16/9" src="https://www.youtube.com/embed/Q-hTNdksTDs" title="Logic+ Online Overview" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## The modules

The left-hand navigation has five modules:

- **[All Projects](pages/all-projects)**: create projects, organise them into folders and open the one you want to work in. It also shows who has access to each folder and project, and it's where you start each time you come back.
- **[Schedule Manager](pages/schedule-manager)**: upload schedule files for the selected project and track their processing. You also exclude schedules from analysis, archive and restore them, and add custom fields to tell uploads apart here.
- **[Schedule Viewer](pages/gantt-viewer)**: explore and compare schedules in the Gantt chart, including activity relationships, driving logic and how dates moved between updates.
- **[Analytics](pages/analytics-overview)**: nine automated checks covering schedule quality (DCMA, CIOB and other standards), forecasting (confidence, variance, completion date, float burn-down), and delay and risk (critical path evolution, bow wave compression, causes of change, and a Monte Carlo risk simulation).
- **[Delay Analysis](pages/delay-analysis)**: two methods. **Traceback** finds the activities driving project delay and attributes the delay day by day. **[Time-Impact Analysis](pages/time-impact-analysis)** models a hypothetical delay event and tests how it affects Practical Completion.

Most views only show useful data once you've selected a project with processed schedules.

## Getting help

Open **Documentation** from the user menu (top right, next to Sign Out) to reach this site from inside Logic+, then use the navigation on the left or the search box.

For anything this site doesn't answer, or to send feedback, contact **support@lware.com**.
