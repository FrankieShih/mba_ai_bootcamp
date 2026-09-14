---
name: permit-sync
description: Process new facility-permit PDFs in Drive, such as Howard County Recreation and Parks permits. Extract the venue, reservation schedule, exceptions, fee, payment status, and due date; classify the reservation as training or a scrimmage; update the current team spreadsheet; invoke calendar-sync; append the finance entry; create payment or renewal reminders; and mark the file as processed. Use for requests to process a permit or maintain field information. Manual invocation only; do not schedule by cron.
---

# Facility Permit Sync

The club may receive one or more facility permits each season. Most cover club-booked training fields; occasionally a permit covers a scrimmage or another explicitly identified use. Default to training only when the permit gives no indication of a match or opponent.

## Data Sources

- **Permit locations:** A permit may appear in either the current academic year’s `文档` (“Documents”) or `财务` (“Finance”) folder. Check [Documents/DRIVE_STRUCTURE.md](../../../Documents/DRIVE_STRUCTURE.md) for the current IDs, then scan both folders with `listFolderContents`. Select PDFs whose filenames contain `Permit`, case-insensitively, and do not begin with `[已处理] ` (“Processed”).
- Permits in the Finance folder belong only to this skill. `finance-sync` must skip them to avoid conflicting status prefixes.
- **Team spreadsheet:** `2026-2027 球队信息表` (“2026–2027 Team Information”), current spreadsheet ID `1B5bOM1dIqB4AOKCarZp68coushRbY7-3VOkKHixk1SM`. Confirm the current-year ID in `DRIVE_STRUCTURE.md`. This workflow may update the current semester’s training tab, cup tab for scrimmages, and `财务表` (“Finance Ledger”).
- **Todo list:** spreadsheet ID `1VlysVi6CLT7BT4GbUaVgur_NZlcWfMo9hp1hIQx9a7w`, `Sheet1`.
- **Calendar:** delegate all event creation and reconciliation to `calendar-sync`; do not call `createEvent` directly here.

## Workflow

1. For each new permit PDF:

   a. Download it to a temporary directory with `downloadFile` and inspect it with `Read`.

   b. Extract the permit number, approval status, facility and field name, every actual reservation date and time, category label, total fee, payment total, remaining balance, and payment-schedule due date. Expand recurring patterns such as `Occurs every <weekday> effective <start> until <end>`. Exclude every listed `Exception` date.

2. Classify the use:

   - Treat labels such as `Practice`, a clearly stated training purpose, or the complete absence of match or opponent information as **training**.
   - Treat `Game`, `Scrimmage`, `Tournament`, an opponent name, or an explicit user statement as a **scrimmage**, recorded in the cup tab.
   - Training normally occurs on weekday evenings. If a training reservation is not Monday through Friday after roughly 5:00 p.m. and neither the permit nor the user explains why, continue processing but flag the unusual day and time for confirmation in the final report. Do not silently reclassify it.

3. Determine whether the permit is new or revises an existing permit. This check is mandatory.

   Search the finance ledger’s `项目` and `备注` (“Item” and “Notes”) columns and the training or cup headers for the permit number or the same venue with overlapping dates.

   - If no match exists, treat it as a new permit.
   - If the same permit number already exists, treat it as an update:
     - update existing schedule headers in place when venue or time changed; schedule columns are plans, not an immutable transaction ledger;
     - never edit or delete an existing finance row. Append a correction row such as `<venue> permit <number> fee correction (original $X → current $Y)`, record only the difference in `变动` (“Change”), and identify the corrected entry in notes; and
     - state exactly where the prior match was found in the final report.
   - If evidence is only suggestive—for example, the same venue but nonoverlapping dates, or mismatched permit numbers—do not choose between new and update. Report the ambiguity, skip all writes for that file, and leave its filename unchanged for a later confirmed run.

   ### 3a. Training

   Read the live header of `秋季训练` or `春季训练` (“Fall/Spring Training”). Find `总出勤` (“Total Attendance”) and the largest existing training number. Sort the permit’s actual reservations chronologically, excluding exceptions. Insert one column per reservation before `总出勤` with `add_columns`; number them sequentially and write headers following the live format:

   ```text
   第<N>次训练
   时间：<M.D> <H:MMam/pm>-<H:MMam/pm>
   地点：<field>(<facility>)
   ```

   Leave player attendance cells empty for later attendance maintenance.

   ### 3b. Scrimmage

   Read the live structure of `秋季杯赛` or `春季杯赛` (“Fall/Spring Cup”); do not assume a fixed column count. Append a match group following the existing `第N场, 对手:` pattern and fill in the permit’s time and location. If no opponent is stated, use `待补充（permit 未注明对手）` (“To Be Added—Opponent Not Listed on Permit”) and remind the manager to complete it.

4. Invoke `calendar-sync` after the schedule update and before marking the file processed. It must perform a full reconciliation so the new or revised session, match, and supporting tasks appear on the calendar. This invocation may occur before or after steps 5 and 6, but it is mandatory.

5. Append the finance entry. Reread the entire live `财务表` immediately before writing to determine the actual last row. Never modify or delete earlier rows.

   - `项目`: `<venue> training-field permit (permit <number>, <start–end dates>)`;
   - `变动`: negative total fee;
   - `余额`: a formula using the previous row’s balance plus this row’s change, never a static calculation;
   - `备注`: approval status, payment status, and remaining balance plus due date when unpaid; and
   - `参考文件`: `https://drive.google.com/file/d/<fileId>/view`.

6. Append Todo reminders, deduplicating by exact task text plus the year and month of the due date.

   - If the permit is unpaid or partially paid, add: category `场地费用` (“Facility Fee”); task `缴纳 <venue> permit(<number>)场地费 $<amount>`; owner from the management guide or roster when a finance role is specified, otherwise `队长` (“Captain”); status `待执行` (“To Do”); due date from the permit; and notes containing the amount and permit link.
   - Always add a renewal review: category `场地续订` (“Facility Renewal”); task `评估是否续订 <venue>训练场地(当前 permit 到 <end date> 到期)`; owner `队长`; status `待讨论` (“To Discuss”); due date equal to the permit’s end date; and notes containing the permit link and number. Do not subtract two weeks—the weekly reminder already highlights items due within 14 days.

   These reminders prepare managers to act. Do not contact vendors, make payments, or renew a facility on their behalf; observe `Documents/BOUNDARIES.md`.

7. Only after schedule, Calendar, finance, and Todo updates all succeed, rename the file to `[已处理] <original filename>`. Leave skipped ambiguous files unchanged.

8. Report:

   - the number of new permit files found and processed;
   - for each permit, venue, covered dates, total fee, classification, and evidence for new versus update;
   - unusual non-weekday-evening training times;
   - schedule columns or match groups added or revised, the finance row added including balance, and Todo reminders added;
   - `calendar-sync` counts for created and updated events plus its list of obsolete events that were marked but not deleted;
   - a request that the manager review the spreadsheet, finance ledger, and Todo list; and
   - any ambiguous file that was skipped, clearly separated from processed files.
