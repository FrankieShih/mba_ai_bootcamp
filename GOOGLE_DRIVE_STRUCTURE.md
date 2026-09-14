# Google Drive Folder Structure Reference for Claude Code

This document is a reference index for **Claude Code**. It records the complete folder structure and folder IDs of the shared JHU CFC / Baltimore Origin Google Drive. When a user asks to find or edit a file, or when a skill needs to locate specific material, **check this document first** instead of traversing the entire Drive. A full traversal requires dozens of tool calls and is slow.

Root folder ID: `1z8etXodEBVjI16jJjElaFjJheVV54ble`. This is configured as the `DRIVE_FOLDER_ID` environment variable; see [INSTALL.md](INSTALL.md).

> **Maintenance note:** This document is a point-in-time snapshot, last updated on 2026-07-12. If an ID can no longer be found or a folder’s contents have changed, use `listFolderContents` to verify the current state and update this document. Do not assume the information below is permanently accurate.

---

## Top-Level Structure

The root contains six folders:

| Folder | ID | Contents |
| --- | --- | --- |
| `个人资料` — Personal Files | `1MlCcOXJZ_jRS5yDj66TxBvL3lhTRl1g_` | Personal photos and videos, organized in subfolders by person |
| `俱乐部年度资料` — Club Annual Records | `1zzyCUlC_2gX8q6_9Q8wjSoD6KFjSgdCo` | **Primary content**, organized by academic year from 2010–2017 through 2026–2027 |
| `备份文件原件` — Original File Backups | `1963oMtMVflJq477c8A0kqJY2ezXqfqSJ` (created 2026-07-07) | Original Word and Excel files that were converted to Google Docs or Sheets in Club Annual Records; archival only and not normally used |
| `数据库` — Database | `1BJp92Dc8CUu4jXK-nxbh6vPH3iK9VSQv` | Core structured data, including the player directory and event records |
| `素材` — Assets | `1adUoCZrXRfrtH5VxR5mYo7xvLzJqo8-V` | Brand assets such as jerseys, club crests, sponsor logos, and website media |
| `网站` — Website | `1vVkvy0Guemdv5x3ns1f94ZFYTfoYDBDU` | Club Google Site, contact form, and team-history content structure |

---

## Personal Files — `个人资料`

Folder ID: `1MlCcOXJZ_jRS5yDj66TxBvL3lhTRl1g_`

- Subfolders are organized by person. Each person’s folder contains that individual’s photos and videos.

---

## Database — `数据库`

Folder ID: `1BJp92Dc8CUu4jXK-nxbh6vPH3iK9VSQv`

As of 2026-07-07, the former `database` workbook, which contained `events` and `personal` tabs, has been split into two independent Google Sheets:

- **`人员名录` — Player Directory**: Google Sheet ID `1duAvDHOKOiE6ejOZPJOMl2Q5tACDguXu49IY93n1Dlg`. It has one tab, `Sheet1`, with columns `名字` / `在队年份` / `参与比赛` / `个人荣誉` (“Name” / “Years with Team” / “Matches and Events” / “Individual Honors”). When editing this sheet, only append new records or update existing rows by extending the years, appending events, or adding honors. **Never delete an existing name or previously recorded content.** This is a permanent rule.
- **`球队赛事` — Team Events**: Google Sheet ID `18Wi-d3Fra8Ikx7OlKg_qPjQ7XIpCReXmt8QNtokqDFc`. It has one tab, `sheet1`, with columns `队年` / `时间` / `事件` / `成绩` / `参与人员` / `综述` / `资料` (“Team Year” / “Date” / “Event” / “Result” / “Participants” / “Summary” / “Materials”). Each team-year group ends with an `XX综合纪事` or `XX年度纪事` overall-summary row. Its team-year, date, and result cells are blank. Use its participant list as evidence when identifying a player’s years with the team, but do not treat it as a specific match when updating the player directory. Event summaries and materials often mention MVPs or best-player awards; extract these when updating individual honors.
- **`球队词汇表` — Team Glossary**: Google Doc ID `10jhdvBw77k6QKcGyrVvpsCJ6_HyOM6On5fOweJLbQIY`, added 2026-08-01. It contains the event-name aliases, tactical terminology, and player nicknames formerly stored in the deleted repository file `Documents/GLOSSARY.md`. Read this glossary before processing the Player Directory or Team Events. The same competition may be recorded under different names, for example `四校赛 = 宾大邀请赛` and `藤杯 = 常春藤杯 = IVY CUP`. The glossary lives in Drive because it must be updated during normal operations and remain directly editable from cloud Slack sessions and by club managers. See `CLAUDE.md`, section “Repository Files vs. Drive Files,” for the full maintenance boundary.

  As of 2026-08-27, the glossary has two main sections:

  - `通用词汇` — General Glossary: club names, competition names, and tactical terms shared across academic years.
  - `年度词汇` — Annual Glossary: currently player nicknames, organized by academic-year subheading such as `2026-2027学年 人员昵称`.

  Player nicknames are year-specific because different people may use the same English name in different years. Always locate the current-year nickname subsection before reading or adding a nickname. Do not hard-code a specific academic year in code or skills; use `findSectionsByHeading` at runtime.

---

## Assets — `素材`

Folder ID: `1adUoCZrXRfrtH5VxR5mYo7xvLzJqo8-V`

| Subfolder | ID | Contents |
| --- | --- | --- |
| `球衣` — Jerseys | `1ankMeHkofCqWqoHGazg1uXAxnbCtHGto` | `jersey.png` and five jersey-design screenshots |
| `网站` — Website Assets | `1XXnLkNaruooftS41pEKWUKkOWFBxcZ47` | `ins.png` and `rednote.jpg/png`, including social-media icons and QR codes |
| `赞助商` — Sponsors | `1EoWAvH3Vpc1VwybbuOqjfLwkTf73kQ38` | Sponsor-specific folders described below |
| `队徽` — Club Crest | `1lNBIdz5a4Hfn9HyWRitep0VC7MAHN9Hd` | JHU CFC logo in blue, outline, and white versions, with `.eps`, `.jpg`, and `.png` files; 11 files total |

The Sponsors folder contains one folder per sponsor:

- Allset: `1UrqhPBLjvlm2wVN0E4iUKR5EXnjDFFDi` — `AW_gw.png`, `AW_w.png`, and the `AlimamaShuHeiTi` font subfolder.
- Appliance4less: `1OE5IuH9ql_fhWEhv4by581D9J8cy02T0` — `new sponsor.PNG`.
- AsianBite: `1Ts8tDts8SNQbttoP0FgXhaWriZGB2TIK` — `AsianBite.png` and `AsianBite_white.png`.
- Baltimore Motorsports: `13eZyu3iHkhkYqVS_4iqHER3KsmPd10M2` — `logo.pdf` and `logo.png`.
- LunarNight: `13Qd8QUO5d9k44xLJkTaXa56ZIlLpRy6j` — `sponsor_black.png`.

---

## Website — `网站`

Folder ID: `1vVkvy0Guemdv5x3ns1f94ZFYTfoYDBDU`

- **Johns Hopkins University Chinese Football Club** — Google Site ID `1QzQp-xko5RFFnctZNI3xB_Ugfql1nf8V`.
- **`联系表格` — Contact Form** — Google Form ID `1sRo18Ml4X8IIVNYg1inkLatsykqZFvSn1hK7Wu53hUg`.
- **`网站内容proposal` — Website Content Proposal** — Google Doc ID `1GgSogg3PVhz0CRbaEyTNh3mSnIoqCNiO0mXB24uihd0`. During verification on 2026-07-12, the previously documented `队志内容structure.docx`, ID `1K0cfvf6Rv3Js2JMR91kkBnA_V1_8NMdq`, was no longer in this location. The current Google Doc appears to serve a similar purpose, but its precise content and role still require confirmation.

---

## Club Annual Records — `俱乐部年度资料`

Folder ID: `1zzyCUlC_2gX8q6_9Q8wjSoD6KFjSgdCo`

Records are organized by academic year. There is no 2020–2021 folder, probably because club activity was interrupted during the pandemic. Depending on the year, a folder may contain `文档`, `多媒体`, `表格`, and `财务` (“Documents,” “Multimedia,” “Spreadsheets,” and “Finance”). Not every year has all four; newer years generally have more complete structures, while older years may have missing or empty folders.

### Academic-Year Folder IDs

| Academic year | Year-folder ID | Documents | Multimedia | Spreadsheets | Finance |
| --- | --- | --- | --- | --- | --- |
| 2010–2017 | `1kEC3bRQY35TPIbdOuZ-tZffpOKwzrRI3` | `1Cb9zhuO7bZveNTXMu8yxLWzglE5y5MRm` (empty) | `1Pht5gp2IDnNlpL_3s4MAsp912RB98Ybj` | — | — |
| 2017–2018 | `1Onw2DwkDBNRUUGD2gGAcJ_7q0N1eGShJ` | `1bQxJSAYaGo_IMn0t9M5y-oN5vtQi-3Un` (empty) | — | — | — |
| 2018–2019 | `1vEU8h-48bH6pXozPQyOmQt14iXbqOwRf` | `16PLVTZ_yTpqjmtLGUR8jXOVM1QdQA_vk` (empty) | `1RcAG4xGGYafyoMGWR0dMSKCnilofv7Yb` | — | — |
| 2019–2020 | `13TbKVaxXX4Zplgz1jlHglqDtDfaOdJi0` | `1ctMn1LxVCjrgRhgbXh-ZiaVdZh3QJ5Bq` | `1KqakGM7UQfcPdoYJ1La7fAPXF5PgTIpV` | — | — |
| 2021–2022 | `1XdLQUgX4XKl68I9H-HLOM9cwcAe_uIZW` | `14-lnLkKiAiFj5JrkWC9Vh_SmFDnKAbB8` | `1-WjG4UPlWYdROiUmMsaxoEJsDqNTZra4` | — | — |
| 2022–2023 | `1mFz9P20LmdlhusFHpk69K2QHuPNmf6-y` | `1QEK-UIAV6dRXjV85sLNF7hVwymWsswRH` | `1z1cKLry9ry827Qs1K2D9VTzkZ8cwRzut` | — | — |
| 2023–2024 | `1QOV4HtDt-YSpmsVon-b4yd9fhr7DARV_` | `1CO70NQ3ilLuU8Efs5Yvi_22OuMnfJQMK` | `142NEGy__La8jkZnYjtLBzTx7GJaaO-r4` | — | — |
| 2024–2025 | `1dvTCAo6pmooVz5mlA66ym-GqoEFZh3sr` | `1hDYXIz1ziOfgUlIfCgHk19ZLYEfcXwHP` | `10aFlsPdh02KcN75Yo4GK18Kf8d5pEXYs` | — | — |
| 2025–2026 — marked current year; most complete | `1ibd2KGuA7Wazb2UIDN35U5qP1j8_pdqc` | `1_nClnHq-C0N6-6E1YFKGF3UdGYhRpIh3` | `19vK3IMDyE_pPj4BcowvfJK6XN8JXqL7u` | `1_F-L7YkOdjZ2lcK17HtrBO1Xn0mBh-yQ` | `18xd5jGMNdjrMOPPYv8KMK2L1BxWcjIbf` |
| 2026–2027 | `1pSIj7zaxAWI9erespUioQ01Bj48XLeci` | `1svSvT9Yh-B4FD6SoZcuK8YNEd8SEy2r4` | `1ynMedY39M9Fl2CsmsKsXkUJYSnLINaqd` | `1ENBwMTKbOm6dohGJZx75S-TLEbQhqCKi` | `1fBuq0bvz-__1MGyqD5DxtIWAh8F7nELU` (empty) |

The 2024–2025 and 2026–2027 year-folder roots originally each contained a `管理章程.docx` (“Management Charter”). As of 2026-07-07, neither original remains in its former location:

- The 2024–2025 original, ID `1zVrh9Sm5NmhyCESflZKCVgbXuP16nNcD`, was deleted. Its location now contains a native Google Doc named `管理章程`, ID `1dtgKp-OHLrywtIv1H8N_-1MIWa3k235y9OevuRptfz8`.
- The 2026–2027 original, ID `1XXF6josN1K-AxdbywahuykpN7aPCt_nI`, still exists but is not in either year folder or Original File Backups; its current location must be searched when needed. Its former location now contains a native Google Doc named `管理章程`, ID `1BXnlh-zkVkCTScxq5gpC9dvjTE7gEyfG6N3GR0I_dt4`.

Use the native Google Doc versions for routine work.

### Original-File Archiving Note — 2026-07-07

The user manually converted Word and Excel files from academic-year `文档` folders into corresponding native Google Docs and Sheets with the same filenames. The original Office files were moved to the root-level `备份文件原件` folder. Some originals were deleted directly and may legitimately be absent from that archive.

Most files now found in academic-year Documents folders are native Google Docs or Sheets, not `.docx` or `.xlsx`. Open those native files directly. Search Original File Backups, folder ID `1963oMtMVflJq477c8A0kqJY2ezXqfqSJ`, only when the original Office formatting or formulas are specifically needed.

### Typical Subfolder Contents Across Academic Years

This is the union of common contents; not every year includes every item.

- **`文档` — Documents:** competition registration forms, rosters, competition handbooks in PDF, and team-statistics workbooks. Most are now native Google Docs or Sheets following the 2026-07-07 conversion. Older years contain less material, and the 2017–2018 and 2018–2019 Documents folders are empty.
- **`多媒体` — Multimedia:** subfolders organized by event or date. Common names include `XXXXLOCAL` for routine local activity; `四校赛`, `校友杯`, `藤杯`, `宾州杯`, `联盟杯`, `特拉华杯`, and `美东杯` for external competitions; plus `训练`, `联赛`, `定装照`, `年度照片`, `赛季总结`, and recruitment activities. Competition years may cross academic-year boundaries. Older years have simpler structures. Albums often contain dozens or hundreds of files; do not enumerate all contents unless necessary. Retrieve by event, filename, or date filter.
- **`表格` — Spreadsheets:** recruitment forms, historical player-information forms, and response sheets or folders. Observed only in 2025–2026 and 2026–2027.
- **`财务` — Finance:** finance summaries, sponsor ledgers, facility-permit PDFs, and receipt PDFs. Observed primarily in 2025–2026. The 2026–2027 Finance folder was empty during the day on 2026-07-12 but received its first invoice that evening. Facility permits are not guaranteed to appear here; the first 2026–2027 permit, R100872 on 2026-07-27, appeared directly in Documents. `permit-sync` must scan both folders.

### 2026–2027 Team Information Spreadsheet

Google Sheet ID: `1B5bOM1dIqB4AOKCarZp68coushRbY7-3VOkKHixk1SM`, located in the 2026–2027 Documents folder.

This spreadsheet is the authoritative source for recurring training and league schedules. Calendar automation must read it rather than infer dates from broad descriptions in the management guide. Its contents change frequently, so the following is a structural reference only. Always reread live scheduling data before use.

After cleanup on 2026-07-12, its tabs were:

- `大名单及个人信息` — Roster and Personal Information: names, birthdays, identity documents, and competition status.
- `秋季训练` — Fall Training: summer informal training; at the time, two sessions with dates and locations in their headers.
- `春季训练` — Spring Training: empty template with the same structure.
- `秋季联赛`, `秋季杯赛`, `春季联赛`, and `春季杯赛` — Fall/Spring League/Cup match tables; most had empty headers, and formal season dates had not yet been entered.
- `财务表` — Finance Ledger: columns `项目` / `变动` / `余额` / `备注` / `参考文件`, beginning with `上一年结余` (“Prior-Year Balance”). Maintained by `finance-sync` since 2026-07-12.

### Finance Sync Skill — Added 2026-07-12

`.claude/skills/finance-sync/SKILL.md` scans the current 2026–2027 Finance folder, ID `1fBuq0bvz-__1MGyqD5DxtIWAh8F7nELU`, for newly uploaded invoices and receipts, extracts amount and purpose, and appends entries to `财务表` in the Team Information spreadsheet.

Processed filenames receive either `[已入账]` (“Recorded”) or `[待人工核对]` (“Needs Manual Review”). Do not manually remove these prefixes or treat the renamed file as a replacement. This skill is invoked manually and is not part of an automated cron job. It skips any filename containing `Permit`, which belongs to `permit-sync`.

### Facility Permit Sync Skill — Added 2026-07-27

`.claude/skills/permit-sync/SKILL.md` processes permit PDFs exported by Howard County and similar parks departments, such as `Permit#R100872.pdf`. Because permits may appear in Documents rather than Finance, it scans both folders. It extracts venue, dates, times, and fees, then:

1. appends one column per training session to `秋季训练` or `春季训练`, or adds a scrimmage to `秋季杯赛` or `春季杯赛`;
2. invokes `calendar-sync` to create the session or scrimmage and its supporting actions;
3. appends a facility-fee entry to the Finance Ledger; and
4. adds an unpaid-balance reminder when needed and a permit-renewal reminder to the Todo list.

After successful processing, it prefixes the filename with `[已处理]` (“Processed”). It is manually invoked and is not part of cron.

**First production validation on 2026-07-27:** Permit R100872 covered Howard County’s Troy Park Field #2 from 2026-08-26 through 2026-11-18, every Wednesday from 8:00–10:00 p.m., for 12 sessions. The total fee was $3,840 and remained unpaid at processing time. The workflow added training sessions 3 through 14 to the Fall Training tab, created 60 Calendar events—12 training sessions plus four supporting events per session—appended a Finance Ledger entry with a change of `-3840.00` and a resulting balance of **-2026.529935**, and added two Todo items: payment due 2026-08-12 and renewal review due 2026-11-18.

**Management-guide inconsistency discovered during processing:** The July guide’s Fall Training section said the training plan should be finalized by Wednesday at 10:00 p.m. and teams finalized and published Thursday at 3:00 p.m. This wording appears to assume Thursday training. The new permit scheduled training for Wednesday evening. Applying the fixed weekdays literally would place plan confirmation after training and team publication the following day. The run instead interpreted the rules relative to training day: plan confirmation Tuesday at 10:00 p.m. and team publication Wednesday at 3:00 p.m. Calendar descriptions disclose this judgment. Managers should confirm the interpretation and preferably rewrite the guide using “the day before training” and “training day” rather than fixed weekdays.

### Calendar Sync Skill — Separated from Todo Reminder on 2026-07-27

`.claude/skills/calendar-sync/SKILL.md` contains the Calendar maintenance logic formerly embedded in `todo-reminder`. It allows `permit-sync` and weekly reminders to use the same reconciliation workflow. The core safety rule is unchanged: `deleteEvent` is blocked. Events judged obsolete are marked and reported, not deleted, preventing a repeat of the accidental bulk deletion on 2026-07-12. See `CLAUDE.md`, section “Known Limitations.”

### Attendance Sync Skill — Added 2026-08-27

`.claude/skills/attendance-sync/SKILL.md` accepts an actual attendance list and date, finds the nearest session or match in the relevant training, league, or cup tab, and writes `1` for attendees without changing anyone else. There is no separate Attendance sheet; attendance is stored directly in these tabs, which already contain summary columns such as `总出勤` (“Total Attendance”).

Names are resolved to the roster’s `prefered name`, then through the current academic year’s nickname section in the Team Glossary. Unresolved names must be confirmed with the user rather than guessed, and confirmed new aliases are added to the current-year glossary. The skill is manually invoked and not part of cron.

**First production validation on 2026-08-27:** In a local conversation, the user supplied a group message—`周三训练 7:45换完鞋 troy park`—and 23 names without an explicit date. The workflow inferred that the list referred to the previous day, Wednesday 2026-08-26, and matched it exactly to the first Fall Training session on 8/26. Sixteen names matched directly or indirectly. Seven aliases—`棉花糖🍀`, `橙留香`, `洛`, `Joey`, `sss`, `wjm`, and `Will`—required confirmation. After the question, the live roster was reread; this revealed that Jason and Joey (杨雨让) had already been added, and that Will was already 冯盛’s `prefered name`, not 唐威廉 as initially suspected. The final write added 23 rows to Fall Training with name, `1`, and a `SUM` formula in Total Attendance. Five confirmed nickname records were appended to the Team Glossary using `appendMarkdown`; `appendTableRows` does not work on this Doc and returns `Spreadsheet not found`.

---

## Usage Guidance

- **Finance or reimbursement records:** Start with the current-year Finance subfolder. The existing snapshot marks 2025–2026 as the current year and identifies its `财务总表` as the main summary, but verify the current academic year before use.
- **Recruitment or player-information forms:** Use the current-year Spreadsheets subfolder.
- **Structured player or event data:** Use `人员名录` and `球队赛事` in the root Database folder rather than the former combined `database` workbook.
- **Exact training or league schedule:** Use the 2026–2027 Team Information spreadsheet in that year’s Documents folder. The management guide describes approximate annual rules, not authoritative dates.
- **Photos or videos from a match:** Use the corresponding academic year’s Multimedia folder and locate the event or date subfolder. Avoid listing hundreds of files when filtering by name or date is sufficient.
- **Brand assets:** Use the root Assets folder.
- **Management charters:** The 2024–2025 and 2026–2027 year-folder roots each contain a native Google Doc named `管理章程`; since 2026-07-07, these are no longer `.docx` files.
- **Original Word or Excel file:** Search Original File Backups by filename instead of using the converted native Google version.
