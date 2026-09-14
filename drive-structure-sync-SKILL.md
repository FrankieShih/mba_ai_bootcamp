---
name: drive-structure-sync
description: Perform the monthly integrity check for the Google Drive hierarchy documented in Documents/DRIVE_STRUCTURE.md. Verify the top-level folders, academic-year folders, current-year subfolders, and key Database, Assets, and Website locations. Update the document when IDs, names, contents, or the current academic year change. Called by todo-reminder on the first Sunday of each month and available for manual runs.
---

# Drive Structure Sync

`Documents/DRIVE_STRUCTURE.md` is the working index of Google Drive folder and file IDs. This skill proactively verifies that index once per month. Its purpose is to detect changes, not to inventory the entire Drive, so limit the review to IDs and locations already documented.

## Verification Scope and Workflow

1. Read all of `Documents/DRIVE_STRUCTURE.md` and note every documented ID and description.

2. **Six top-level folders:** Call `listFolderContents` once on root folder `1z8etXodEBVjI16jJjElaFjJheVV54ble`. Compare the six names and IDs with the `顶层结构` (“Top-Level Structure”) table and identify additions, removals, or renames.

3. **Academic-year folders:** Call `listFolderContents` once on `俱乐部年度资料` (“Club Annual Records”), folder ID `1zzyCUlC_2gX8q6_9Q8wjSoD6KFjSgdCo`, and compare it with the `各学年文件夹 ID` (“Academic-Year Folder IDs”) table.

   - Prioritize detecting a new year folder, such as `2027-2028`. All current-year skills—including `finance-sync`, `permit-sync`, and `calendar-sync`—must switch when a new current academic year appears.
   - For a new year, inspect its `文档`, `多媒体`, `表格`, and `财务` (“Documents,” “Multimedia,” “Spreadsheets,” and “Finance”) subfolders. Record the IDs that actually exist, add a row in the existing format, and move the current-year marker to the new row.
   - Verify existing year-folder names and IDs.

4. **Current-year subfolders:** Inspect each of the four documented subfolders with `listFolderContents`.

   - Give highest priority to named files used by other skills, such as the current team-information spreadsheet, Todo list, and club management guide. Confirm that the files still exist and their spreadsheet or document IDs remain correct.
   - Note new files and correct descriptions that are no longer accurate.
   - Do not inspect every sheet tab or volatile internal structure. Consumer skills load that content when they run; this document only needs to identify the file and its correct ID.

5. **Database, Assets, and Website:** Inspect each folder once. Confirm the documented IDs and descriptions for the roster and team-event databases; jersey, website, sponsor, and crest asset folders; and the Google Site and contact spreadsheet.

6. **Personal Information and Original Backups:** These folders are large and change infrequently. Confirm only that `listFolderContents` can access them without error; do not enumerate their full contents.

7. Update `Documents/DRIVE_STRUCTURE.md` directly whenever a difference is found; its maintenance notice already authorizes this.

   - When an ID cannot be resolved, distinguish confirmed deletion or movement from an unresolved access or service error. Never describe an uncertain failure as a confirmed deletion.
   - Correct stale content descriptions.
   - Add new academic years, folders, or important files using the document’s existing format.
   - Change the maintenance notice’s `最后更新: YYYY-MM-DD` (“Last Updated”) date to the current date after editing.

8. Report to the caller, not directly to the final user: the areas checked, each corrected change, the number of changes, and any unresolved anomaly requiring human confirmation. `todo-reminder` incorporates this result into its final Slack message.
