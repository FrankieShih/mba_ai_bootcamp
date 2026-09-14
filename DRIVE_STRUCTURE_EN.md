# Google Drive Folder Structure Reference (for Claude Code)

This document is a reference index for **Claude Code**. It records the complete folder structure and folder IDs of the JHU CFC / Baltimore Origin shared Drive.当用户要求查找、编辑某个文件,或某个 skill 需要定位特定Sources时,**先查这份Documents**,不要每次都重新遍历整个 Drive(遍历一次要几十次工具调用,很慢)。

Root folder ID:`1z8etXodEBVjI16jJjElaFjJheVV54ble`(set as the `DRIVE_FOLDER_ID` environment variable; see [INSTALL.md](INSTALL.md))。

> **Maintenance note**:这份Documents是某次Time点的快照(最后更新:2026-07-12)。如果发现某个 ID 已经找不到、或者Folder里多/少了东西,直接用 `listFolderContents` 现查并更新这份Documents,不要盲目相信下面写的Contents。

---

## Top-Level Structure

The root directory contains six folders:

| Folder | ID | Contents |
|---|---|---|
| Personal Files | `1MlCcOXJZ_jRS5yDj66TxBvL3lhTRl1g_` | 个人照片/视频,按人名分子Folder |
| Club Annual Records | `1zzyCUlC_2gX8q6_9Q8wjSoD6KFjSgdCo` | **主体Contents**,按Academic Year分Folder(2010-2017 ~ 2026-2027) |
| Original File Backups | `1963oMtMVflJq477c8A0kqJY2ezXqfqSJ`(2026-07-07 新建) | Stores original Word/Excel files that were converted to Google Docs/Sheets in "Club Annual Records"; archival only and normally does not need to be consulted |
| Database | `1BJp92Dc8CUu4jXK-nxbh6vPH3iK9VSQv` | Core data tables (player roster and event records); see below |
| Assets | `1adUoCZrXRfrtH5VxR5mYo7xvLzJqo8-V` | Jerseys/Team Logo/Sponsors logo/WebsiteAssets等品牌资产 |
| Website | `1vVkvy0Guemdv5x3ns1f94ZFYTfoYDBDU` | 俱乐部 Google Site + Contact Form + 队志Documents结构 |

---

## Personal Files `1MlCcOXJZ_jRS5yDj66TxBvL3lhTRl1g_`

- 按人名分子Folder,每个人名Folder下是个人照片/视频。

---

## Database `1BJp92Dc8CUu4jXK-nxbh6vPH3iK9VSQv`

**Since 2026-07-07, this has been split into two independent Google Sheets** (previously a single sheet called "database" containing `events` and `personal` tabs):

- **Personnel Roster**(Google Sheet)`1duAvDHOKOiE6ejOZPJOMl2Q5tACDguXu49IY93n1Dlg` — single sheet tab `Sheet1`,columns:`Name`/`Years on Team`/`Events Participated`/`Individual Honors`。**编辑这张表时只能追加/更新已有行的Contents(Years on Team延长、Events Participated追加、Individual Honors填入),不能删除已有的Name或已记录的Contents**,这是长期规则,不是一次性要求。
- **Team Events**(Google Sheet)`18Wi-d3Fra8Ikx7OlKg_qPjQ7XIpCReXmt8QNtokqDFc` — single sheet tab `sheet1`,columns:`Team Year`/`Time`/`Event`/`Result`/`Participants`/`Summary`/`Sources`。每个"Team Year"分组末尾会有一行"XX综合纪事"/"XX年度纪事"(Team Year/Time/Result列是empty的),是当年的整体总结文字,不是具体赛事,给球员定"Years on Team"时可以参考它的"Participants"名单,但不要当成一场具体比赛塞进"Events Participated"字段。赛事Summary/Sources文字里经常会写 MVP、最佳球员等Individual Honors,更新Personnel Roster时要留意提取。
- **Team Glossary** (Google Doc) `10jhdvBw77k6QKcGyrVvpsCJ6_HyOM6On5fOweJLbQIY` — Added 2026-08-01. Contains the event-name, tactical jargon, and personnel-nickname synonym glossary formerly stored in `Documents/GLOSSARY.md` (the repository copy has been deleted). **Before processing the Personnel Roster or Team Events sheets, read this Doc first.** Examples: 四校赛 = Penn Invitational; 藤杯 = Ivy Cup = IVY CUP. Different years or people may use different names for the same event. The glossary is on Drive because it needs ongoing maintenance and direct access by Slack conversations and team managers. Since 2026-08-27 it has two major sections: “General Vocabulary” (club names/event names/tactical jargon, shared across years) and “Annual Vocabulary” (currently personnel nicknames, organized by academic-year subheading). Because the same English name may refer to different people in different years, always consult the current academic-year subsection and do not hard-code a year in code/skills; use `findSectionsByHeading` before execution.

---

## Assets `1adUoCZrXRfrtH5VxR5mYo7xvLzJqo8-V`

| 子Folder | ID | Contents |
|---|---|---|
| Jerseys | `1ankMeHkofCqWqoHGazg1uXAxnbCtHGto` | jersey.png + 5 张Jerseys设计截图 |
| Website | `1XXnLkNaruooftS41pEKWUKkOWFBxcZ47` | ins.png、rednote.jpg/png(社交媒体图标/二维码) |
| Sponsors | `1EoWAvH3Vpc1VwybbuOqjfLwkTf73kQ38` | see below |
| Team Logo | `1lNBIdz5a4Hfn9HyWRitep0VC7MAHN9Hd` | JHU CFC logo(blue/outline/white 三色,各含 .eps/.jpg/.png,共 11 个文件) |

**Sponsors** `1EoWAvH3Vpc1VwybbuOqjfLwkTf73kQ38` 下按Sponsors分Folder:
- Allset `1UrqhPBLjvlm2wVN0E4iUKR5EXnjDFFDi` — AW_gw.png, AW_w.png + 子Folder AlimamaShuHeiTi(字体文件)
- Appliance4less `1OE5IuH9ql_fhWEhv4by581D9J8cy02T0` — new sponsor.PNG
- AsianBite `1Ts8tDts8SNQbttoP0FgXhaWriZGB2TIK` — AsianBite.png, AsianBite_white.png
- Baltimore Motorsports `13eZyu3iHkhkYqVS_4iqHER3KsmPd10M2` — logo.pdf, logo.png
- LunarNight `13Qd8QUO5d9k44xLJkTaXa56ZIlLpRy6j` — sponsor_black.png

---

## Website `1vVkvy0Guemdv5x3ns1f94ZFYTfoYDBDU`

- Johns Hopkins University Chinese Football Club (Google Site) `1QzQp-xko5RFFnctZNI3xB_Ugfql1nf8V`
- Contact Form (Google Form) `1sRo18Ml4X8IIVNYg1inkLatsykqZFvSn1hK7Wu53hUg`
- Website Content Proposal (Google Doc) `1GgSogg3PVhz0CRbaEyTNh3mSnIoqCNiO0mXB24uihd0` — **Verified on 2026-07-12:** the previously recorded `队志内容structure.docx` (ID `1K0cfvf6Rv3Js2JMR91kkBnA_V1_8NMdq`) is no longer at this location. It has been replaced by this Google Doc serving a similar purpose. Its exact content/use still needs confirmation; the new ID is recorded here for now.

---

## Club Annual Records `1zzyCUlC_2gX8q6_9Q8wjSoD6KFjSgdCo`

按Academic Year分Folder(**Note: 没有 2020-2021,大概率是疫情期间empty缺**)。每个Academic Year下视情况包含 `Documents`/`Media`/`Spreadsheets`/`Finance` 四类子Folder,不是每个Academic Year都四类齐全——**年份越新、结构越完整,年代越久的Academic Year往往缺Folder或Folder是empty的**(具体缺失情况see below表)。

### 各Academic YearFolder ID

| Academic Year | Academic YearFolder ID | Documents | Media | Spreadsheets | Finance |
|---|---|---|---|---|---|
| 2010-2017 | `1kEC3bRQY35TPIbdOuZ-tZffpOKwzrRI3` | `1Cb9zhuO7bZveNTXMu8yxLWzglE5y5MRm`(empty) | `1Pht5gp2IDnNlpL_3s4MAsp912RB98Ybj` | — | — |
| 2017-2018 | `1Onw2DwkDBNRUUGD2gGAcJ_7q0N1eGShJ` | `1bQxJSAYaGo_IMn0t9M5y-oN5vtQi-3Un`(empty) | — | — | — |
| 2018-2019 | `1vEU8h-48bH6pXozPQyOmQt14iXbqOwRf` | `16PLVTZ_yTpqjmtLGUR8jXOVM1QdQA_vk`(empty) | `1RcAG4xGGYafyoMGWR0dMSKCnilofv7Yb` | — | — |
| 2019-2020 | `13TbKVaxXX4Zplgz1jlHglqDtDfaOdJi0` | `1ctMn1LxVCjrgRhgbXh-ZiaVdZh3QJ5Bq` | `1KqakGM7UQfcPdoYJ1La7fAPXF5PgTIpV` | — | — |
| 2021-2022 | `1XdLQUgX4XKl68I9H-HLOM9cwcAe_uIZW` | `14-lnLkKiAiFj5JrkWC9Vh_SmFDnKAbB8` | `1-WjG4UPlWYdROiUmMsaxoEJsDqNTZra4` | — | — |
| 2022-2023 | `1mFz9P20LmdlhusFHpk69K2QHuPNmf6-y` | `1QEK-UIAV6dRXjV85sLNF7hVwymWsswRH` | `1z1cKLry9ry827Qs1K2D9VTzkZ8cwRzut` | — | — |
| 2023-2024 | `1QOV4HtDt-YSpmsVon-b4yd9fhr7DARV_` | `1CO70NQ3ilLuU8Efs5Yvi_22OuMnfJQMK` | `142NEGy__La8jkZnYjtLBzTx7GJaaO-r4` | — | — |
| 2024-2025 | `1dvTCAo6pmooVz5mlA66ym-GqoEFZh3sr` | `1hDYXIz1ziOfgUlIfCgHk19ZLYEfcXwHP` | `10aFlsPdh02KcN75Yo4GK18Kf8d5pEXYs` | — | — |
| 2025-2026(当前Academic Year,Contents最全) | `1ibd2KGuA7Wazb2UIDN35U5qP1j8_pdqc` | `1_nClnHq-C0N6-6E1YFKGF3UdGYhRpIh3` | `19vK3IMDyE_pPj4BcowvfJK6XN8JXqL7u` | `1_F-L7YkOdjZ2lcK17HtrBO1Xn0mBh-yQ` | `18xd5jGMNdjrMOPPYv8KMK2L1BxWcjIbf` |
| 2026-2027 | `1pSIj7zaxAWI9erespUioQ01Bj48XLeci` | `1svSvT9Yh-B4FD6SoZcuK8YNEd8SEy2r4` | `1ynMedY39M9Fl2CsmsKsXkUJYSnLINaqd` | `1ENBwMTKbOm6dohGJZx75S-TLEbQhqCKi` | `1fBuq0bvz-__1MGyqD5DxtIWAh8F7nELU`(empty) |

The 2024-2025 and 2026-2027 **academic-year folder roots** originally each contained a `Management Bylaws.docx` file. **Since 2026-07-07, neither original is in its former location.** For 2024-2025, the original (ID `1zVrh9Sm5NmhyCESflZKCVgbXuP16nNcD`) was deleted and only a same-named native Google Doc `Management Bylaws` remains (ID `1dtgKp-OHLrywtIv1H8N_-1MIWa3k235y9OevuRptfz8`). For 2026-2027, the original (ID `1XXF6josN1K-AxdbywahuykpN7aPCt_nI`) still exists somewhere outside these two year folders and outside Original File Backups; its exact location must be checked live. The year folder now contains the corresponding Google Doc `Management Bylaws` (ID `1BXnlh-zkVkCTScxq5gpC9dvjTE7gEyfG6N3GR0I_dt4`). **For routine use, use the two native Google Docs.**

### 2026-07-07:Documents 原件归档说明

The user manually converted Word/Excel files in each academic year’s `Documents` folder into corresponding native Google Docs/Sheets (same filenames). The **original Word/Excel files were moved to the root-level Original File Backups folder** (some originals were directly deleted by the user, which is also normal). Therefore, most files now visible in each year’s `Documents` folder are native Google Docs/Sheets rather than `.docx`/`.xlsx`. **Open the native files directly when looking for content; do not search for Office originals.** If an original is specifically needed (for example, to verify original formatting/formulas), search Original File Backups (`1963oMtMVflJq477c8A0kqJY2ezXqfqSJ`) by filename.

### 各类子FolderContents模板(跨Academic Year取并集,不是每个Academic Year都全)

- **Documents:** Competition registration forms (`XX杯报名表`), team rosters, event manuals (PDF), and team statistics sheets. **Most are now native Google Docs/Sheets** following the 2026-07-07 conversion. Older years contain less material; some years (2017-2018, 2018-2019) have empty Documents folders.
- **Media:** Subfolders by event or date. Common naming patterns include `XXXXLOCAL` (routine training/local activities for that academic year), 四校赛 / 校友杯 / 藤杯 / 宾州杯 / 联盟杯 / 特拉华杯 / 美东杯 (external invitation tournaments; the calendar year may cross academic years), training, league, team portraits, annual photos by person, season summaries including aerial footage, and recruitment-event photos. Older years have simpler structures. 2025-2026 is the most complete. These album folders often contain dozens or hundreds of files, so do not enumerate everything with `listFolderContents`; retrieve items as needed or filter by filename/date.
- **Spreadsheets:** Recruitment forms, historical player-information collection forms, and associated response sheets/folders. Currently observed only in 2025-2026 and 2026-2027.
- **Finance:** Financial summary sheets (`Financial Summary`), sponsor ledgers, venue permits (PDF), and receipts (PDF). Currently observed in 2025-2026. The 2026-2027 Finance folder was empty during the day on 2026-07-12 but received its first invoice that evening. **Venue permits do not necessarily appear here**—the first 2026-2027 permit (R100872, 2026-07-27) appeared directly in `Documents`. The `permit-sync` skill should scan both folders.

### 2026-2027 Team Information Sheet(Training/League数据源)

`1B5bOM1dIqB4AOKCarZp68coushRbY7-3VOkKHixk1SM` (Google Sheet, in the 2026-2027 `Documents` subfolder) — **This is the primary source for recurring training/league schedules**, not a historical summary. Calendar synchronization and similar automation should read this sheet rather than infer dates from vague descriptions in management guides. **This sheet changes frequently; the notes below describe structure only. Always read the live sheet before using actual schedule data.**

As of the 2026-07-12 cleanup, tabs are: `大名单及个人信息` (player basics: name/birthday/ID/competition eligibility), `秋季Training` (summer/preseason informal training; currently two sessions, with dates/locations in column headers), `春季Training` (same structure, empty template), `秋季League`, `秋季杯赛`, `春季League`, `春季杯赛` (competition tables, mostly empty headers; official season dates have not yet been entered), and `财务表` (columns: item/change/balance/notes/reference file; starts with previous-year carryover; maintained by `finance-sync` since 2026-07-12).

### Finance同步 skill(2026-07-12 新增)

`.claude/skills/finance-sync/SKILL.md` — Scans newly uploaded invoices/receipts in the 2026-2027 `Finance` subfolder (`1fBuq0bvz-__1MGyqD5DxtIWAh8F7nELU`), extracts amount/purpose, and appends them to the `财务表` tab in the 2026-2027 Team Information Sheet. Processed files are renamed with `[已入账]` or `[待人工核对]` status prefixes. **Do not manually remove these prefixes or assume a renamed file was replaced.** Invoked manually, not via cron. Files whose names contain `Permit` are skipped and handled by `permit-sync`.

### Venue Permit Sync Skill(2026-07-27 新增)

`.claude/skills/permit-sync/SKILL.md` — Processes venue Permit PDFs exported by Howard County or similar park systems (e.g., `Permit#R100872.pdf`). Such files may appear directly in `Documents`, not only `Finance`. It parses venue/time/fees, then: (1) appends columns to the appropriate training/cup tab using the existing one-column-per-session format; (2) invokes `calendar-sync` to synchronize the session/game and related actions (signup, training-plan confirmation, squad release, attendance maintenance) to Calendar; (3) appends the venue fee to `财务表`; and (4) adds payment and renewal reminders to the Todo list when applicable. Processed files receive a `[已处理]` prefix. Invoked manually, not via cron.

**First real validation on 2026-07-27:** processed Permit R100872 (Howard County, Troy Park Field #2, every Wednesday 8-10pm from 2026-08-26 through 2026-11-18, 12 sessions, total fee $3,840.00, unpaid at processing time). The `秋季Training` tab received 12 new columns (sessions 3-14); Calendar received 60 events (12 training sessions plus four supporting events per session); `财务表` received a row for -3840.00, bringing the balance to **-2026.529935**; and the Todo list received two reminders (payment due 2026-08-12 and venue-renewal evaluation due 2026-11-18).

**A mismatch between the management guide and the actual schedule was discovered and noted in the relevant Calendar event descriptions.** The July-tab guidance for fall training says the training plan should ideally be finalized by Wednesday 10pm and squads published Thursday 3pm—wording that appears to assume Thursday training. The permit, however, fixes training on Wednesday evenings. Applying the weekday names literally would put plan confirmation after training and squad publication the next day. The implementation therefore interpreted the rules relative to training day: plan confirmation = 10pm the day before (Tuesday), squad publication = 3pm on training day (Wednesday). This was a judgment call rather than the guide’s literal wording. Managers should confirm this interpretation and ideally rewrite the guide using relative phrases such as “the day before training” and “training day.”

### calendar-sync skill(2026-07-27 从 todo-reminder 中抽取独立)

`.claude/skills/calendar-sync/SKILL.md` — Extracted from `todo-reminder` on 2026-07-27 so `permit-sync` can reuse the same Calendar synchronization logic after processing a new venue permit. Core design remains unchanged: `deleteEvent` is hard-blocked. Events judged no longer needed are only reported, never deleted, to prevent a repeat of the accidental bulk deletion on 2026-07-12 (see the “Known Limitations” section of `CLAUDE.md`).

### Attendance Sync Skill(2026-08-27 新增)

`.claude/skills/attendance-sync/SKILL.md` — Takes an actual attendance list plus a date, finds the closest matching training/game in the relevant fall/spring training, league, or cup tab, and writes `1` for attendees in the corresponding column without touching anyone else. **There is no separate attendance sheet**; attendance data is written directly into these tabs, which already include a total-attendance column. Match names first against `prefered name` in `大名单及个人信息`, then against the current academic year’s nickname section in Team Glossary. If still unmatched, ask the user—do not guess. Once clarified, append the nickname to the current academic-year glossary. Invoked manually, not via cron.

**First real validation on 2026-08-27** (local conversation, not Slack): the user provided a group message with “Wednesday training, shoes changed by 7:45, Troy Park” plus 23 names but no date. Following the rule to infer from the current date and message context, it was matched to “yesterday” (2026-08-26, Wednesday), exactly matching the first training session dated 8/26 in `秋季Training`. Sixteen of 23 people matched directly or indirectly. Seven nicknames required user clarification. Re-reading the roster after clarification prevented incorrect mappings: Jason and Joey (杨雨让) had already been added, and Will was actually 冯盛’s `prefered name`, not 唐威廉. The final update added 23 rows to `秋季Training` (name in column A, `1` in the session column, SUM formula in total attendance) and added five nickname records to Team Glossary. `appendMarkdown` was used because `appendTableRows` does not work on this Doc and returns “Spreadsheet not found.”

---

## Usage Recommendations

- **Finance/reimbursements:** Check the current academic year’s Finance subfolder first; it contains the core financial summary sheet.
- **Recruitment/player-information forms:** Check the current academic year’s Spreadsheets subfolder.
- **Structured player/event data:** Prefer the `Personnel Roster` and `Team Events` sheets under the root `Database` folder.
- **Specific training/league schedules (date/time/location):** Use the `2026-2027 Team Information Sheet` in the year folder’s `Documents` subfolder. Do not infer specific dates from the management guide; it contains approximate annual rules, while actual dates are in this sheet.
- **Photos/videos for a specific match:** Use the corresponding academic year’s Media folder and locate the event/date subfolder. These folders can contain dozens or hundreds of files, so retrieve/filter as needed instead of enumerating all contents.
- **Brand assets (logos/jerseys/sponsor graphics):** Use the root `Assets` folder.
- **Management bylaws:** The 2024-2025 and 2026-2027 year folders each contain a native Google Doc named `Management Bylaws`.
- **Original Word/Excel files** (rather than converted Google versions): Search the root `Original File Backups` folder by filename.