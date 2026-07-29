---
page-id: key-changes-schedule-viewer
title: Schedule Viewer – Key Changes from Desktop
audience: external
status: draft
version: 1.1.0
last-reviewed: 2026-07-29
---

## Schedule Viewer Key Changes

Records significant differences between the Logic+ Desktop Schedule Viewer and the Logic+ Online Gantt Viewer. 

---

### File selection

**Desktop:** Files selected from a persistent **List of Schedules** panel below the gantt chart.

**Online:** Two dropdowns in the Gantt Viewer toolbar replace the List of Schedules:
- **Comparison** – the schedule being analysed (current update)
- **Baseline** – the reference schedule for delay comparison

Options are labelled by the data date of each upload. All schedule packets uploaded for the project appear in both lists. There is no separate file management panel.

### Entering Delay Analysis / Traceback mode

**Desktop:** Mode switching is handled by top menu and tools. Excluded activities that related to traceback mode where shown in grey.

**Online:** Three explicit mode buttons are used to move between the modes -  **Schedule Viewer**, **Traceback Setup**, **Delay Analysis**. Excluded activities are only visible in **Traceback Setup** mode. Switching mode changes both the toolbar controls and the bottom tray. Switching away from Delay Analysis while a traceback is unsaved opens a prompt to save or discard before switching.


### Group and Sort

**Desktop:** Single grouping option of WBS or Date structures only.

**Online:** A **Group Settings** panel (gear icon, top right of the toolbar) lets you control how activities are grouped and filtered. Grouping options include WBS, Dates and Activity Codes. 

Opening the panel shows:

| Option | Choices |
|--------|---------|
| Outline Level | 1 / 2 / 3 / 4 |
| Start | Year / Month / Week |
| Exclude Summary Tasks | On/Off |
| Exclude Level of Effort Tasks | On/Off |
| Show Task Count | On/Off |

Click **Apply Settings** to apply. Click **Use Default Settings** to reset to Outline Level 3, Start Month, both exclusions on.

**Note:** this field was labelled "Planned Start" until 2026-07-28 (LUSB-1068) and also changed what it groups by – previously the activity's baseline planned start, now its current start date. Relevant if you're documenting or screenshotting this against an older build.

### Go To

**Desktop:** No ability to navigate between activities via their predecessor/successor links.

**Online:** A **Go To** button appears next to each related activity in the activity tray (**Relationships** tab) and the traceback tray (**Candidate Scoring** tab). Clicking it scrolls and highlights the Gantt chart to that activity.

### Sharing a view

**Desktop:** No equivalent – files and view state live only on the machine that opened them.

**Online:** The browser address bar reflects the current project, Comparison/Baseline schedule selection, and active traceback. Copying and sending that URL lets a colleague open the same view directly, including the same traceback result, without re-selecting anything. (LUSB-1085, merged 2026-07-24.)

### Schedule History

**Desktop:** Activity History tab allowed comparison of a single data attribute across schedule versions.

**Online:** The **Changes over time** tab in the activity tray allows the comparison of multiple data attributes for the selected activity across every schedule upload for the project in a single table. Default columns: Start, Finish, Total Float (days), Duration.
