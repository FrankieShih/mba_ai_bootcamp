---
name: finance-sync
description: Process newly uploaded invoices and receipts in the club’s current-year Finance folder. Extract the amount, purpose, date, counterparty, and transaction direction; append a row to the Finance Ledger; and rename the source file to prevent duplicate processing. Flag uncertain documents for human review instead of guessing. Use for requests to reconcile accounts or process new finance documents. Manual invocation only; do not schedule by cron.
---

# Finance Sync

Reconcile new invoices and receipts from the current academic year’s Finance folder into the finance ledger. When a document is unclear, create a review item instead of inventing a value.

## Data Sources

- **Invoice and receipt folder:** the `财务` (“Finance”) subfolder of the current academic year, currently `1fBuq0bvz-__1MGyqD5DxtIWAh8F7nELU` for 2026–2027. This ID changes annually. Before running, check [Documents/DRIVE_STRUCTURE.md](../../../Documents/DRIVE_STRUCTURE.md).
- **Destination ledger:** the `财务表` (“Finance Ledger”) tab in `2026-2027 球队信息表` (“2026–2027 Team Information”), current spreadsheet ID `1B5bOM1dIqB4AOKCarZp68coushRbY7-3VOkKHixk1SM`. Columns are `项目`, `变动`, `余额`, `备注`, and `参考文件` (“Item,” “Change,” “Balance,” “Notes,” and “Reference File”). Confirm the current-year spreadsheet ID before use.

## Workflow

1. List every file in the Finance folder with `listFolderContents`.

   Skip filenames containing `Permit`, case-insensitively. Those belong exclusively to `permit-sync`, whose status prefix differs.

   Treat filename prefixes as the processing ledger:

   - no prefix: new and requires processing;
   - `[已入账] ` (“Recorded”): already processed; skip; and
   - `[待人工核对] ` (“Needs Manual Review”): a review row already exists; skip unless the user explicitly asks to reprocess reviewed files.

2. For each new file:

   a. Download it to a temporary directory with `downloadFile` and inspect the PDF or image with `Read`.

   b. Extract the amount, date, counterparty, and purpose. Determine whether it is an **expense** paid by the club or **income** received by the club from the document header and context. Do not assume every invoice is an expense. If the amount or direction is unclear, treat extraction as failed.

   c. Reread every current row in `财务表` immediately before appending. Determine the actual last row and current balance from the live sheet, because other skills or users may have changed it.

   d. If extraction succeeds, append one row:

   - `项目`: counterparty and purpose, such as `Long Reach HS field permit`;
   - `变动`: a negative number for an expense or a positive number for income;
   - `余额`: a formula referencing the previous row, such as `=<previous balance cell>+<current change cell>`, never a manually calculated static value;
   - `备注`: concise source details such as invoice number and date; and
   - `参考文件`: `https://drive.google.com/file/d/<fileId>/view`.

   Then rename the source file to `[已入账] <original filename>`.

   e. If extraction fails or the document is unrelated to finance, still append a row:

   - `项目`: `待核实: <original filename>` (“Needs Verification”);
   - `变动` and `余额`: blank or `待人工填写` (“To Be Completed Manually”);
   - `备注`: the specific ambiguity or failure; and
   - `参考文件`: the direct Drive link.

   Then rename the source file to `[待人工核对] <original filename>`.

3. This skill is append-only. Never modify or delete an existing finance-ledger row. If a later document reveals that an earlier balance basis may be wrong, explain it in a new row’s notes rather than changing history.

4. Report the number of new files processed, rows appended successfully, and files sent for manual review. List review filenames and ask the manager to inspect those ledger rows and confirm the renamed files in the Finance folder.
