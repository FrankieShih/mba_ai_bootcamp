---
name: attendance-sync
description: Record an actual attendance list, including nicknames, in the correct training, league, or cup column of the current team spreadsheet. Match the event by date, resolve every person to a rostered player, mark attendees with 1, and leave everyone else unchanged. Ask about unknown nicknames instead of guessing, then save confirmed aliases in the current academic year's team glossary. Use for requests such as “record attendance for this training” or “here is this week’s attendance list.” Manual invocation only.
---

# Attendance Sync

Record the captain’s or manager’s final attendance list in the corresponding event column of the current team spreadsheet. Treat the submitted list as the verified record of who attended. Do not infer who should have attended, compare it with registrations, or independently judge absences.

## Data Sources

- **Team spreadsheet:** `2026-2027 球队信息表` (“2026–2027 Team Information”), spreadsheet ID `1B5bOM1dIqB4AOKCarZp68coushRbY7-3VOkKHixk1SM`. This ID changes each academic year. Before running, check [Documents/DRIVE_STRUCTURE.md](../../../Documents/DRIVE_STRUCTURE.md) for the current-year ID.
  - Training records are in `秋季训练` (“Fall Training”) or `春季训练` (“Spring Training”). Each training session occupies one column. Headers resemble `第N次训练\n时间：M.D H:MMam/pm - H:MMam/pm\n地点：XXX`. The final column is `总出勤` (“Total Attendance”).
  - League matches and scrimmages are in `秋季联赛`, `秋季杯赛`, `春季联赛`, or `春季杯赛` (“Fall/Spring League/Cup”). Each match spans a variable group of columns. Row 1 contains a merged match title such as `第N场，对手：XXX`; row 2 contains labels such as `最终比分：`, `时间：`, and `地点：`; row 3 contains fields such as `报名/出场`, `进球`, and `助攻`. Always read and parse the live headers; never assume fixed column numbers.
  - The player-identity column is normally column A and is labeled `姓名` (“Name”). Values written there must use the roster’s `prefered name` value, preserving the source’s existing spelling of that column name.
- **Roster:** the `大名单及个人信息` (“Roster and Personal Information”) sheet in the same spreadsheet. Relevant columns include `prefered name`, `中文全名` (“Full Chinese Name”), `First Name`, and `Last Name`.
- **Nickname reference:** `球队词汇表` (“Team Glossary”), Google Doc ID `10jhdvBw77k6QKcGyrVvpsCJ6_HyOM6On5fOweJLbQIY`. Player nicknames belong under the current academic year’s `年度词汇` (“Annual Glossary”), because the same nickname may identify different people in different years. The current section may be named `2026-2027学年 人员昵称` (“2026–2027 Player Nicknames”) and contains `昵称` / `对应中文全名` / `备注` (“Nickname” / “Corresponding Full Chinese Name” / “Notes”). A nickname cell may contain aliases separated by `/`; compare each alias separately. Do not hard-code the year-specific heading. Resolve it with `findSectionsByHeading` each time.

## Workflow

### 1. Resolve the List and Date

The user may provide names separated by commas, spaces, or line breaks and may describe the date informally, such as “Wednesday’s training” or “8/26.”

- If no date is stated, first infer the likely event type and date from the message, its timestamp, relative terms such as “today” or “yesterday,” weekday references, venue, and opponent. Use `America/New_York` for the current date. Ask only if the message provides too little information to infer even a plausible event.
- If the user supplied a date expression that cannot be interpreted or does not plausibly correspond to any event, ask for the exact date instead of guessing.
- Once a concrete date is available, continue to event matching. The user does not need to specify whether it was training or a match.

### 2. Identify the Training Session or Match

1. Determine the likely season from the date—roughly late August through December for fall and January through May for spring. If uncertain, inspect which semester tabs currently contain data.
2. Read the live headers of the relevant training, league, and cup tabs. Parse every event date from those headers.
3. Compare the supplied date with every parsed event date:
   - A unique exact match is accepted without asking.
   - A unique closest match within two days is accepted, but the final report must state both dates and the difference.
   - If the closest result is tied, or every event is more than two days away, present the candidates with date, opponent, and location and ask the user to choose or correct the date.
4. Record the exact training column or match-group `报名/出场` (“Registration/Appearance”) subcolumn to update.

### 3. Resolve Each Name to a Player

For each submitted name:

1. Match it against the roster’s `prefered name` column first. Ignore case, surrounding whitespace, decorative emoji or symbols, diacritics, and obvious decorative suffixes that do not create identity ambiguity. For example, `🤙Eddie🤙カン`, `Kevïn`, and `Shuo李` may normalize to `Eddie`, `Kevin`, and `Shuo`. Use the matched roster `prefered name` as the row identifier.
2. If unmatched, compare against `中文全名`, `First Name`, and `Last Name`, again ignoring case and spacing. On a match, use that player’s `prefered name`.
3. If still unmatched, check the current-year nickname table. Split aliases on `/`, resolve the corresponding Chinese full name, then retrieve the roster’s `prefered name`.
4. Do not guess unresolved identities. Ask about all unknown names in one message and briefly state which other names were already resolved. You may offer a clearly labeled hypothesis for confirmation.

After the user responds, reread the live roster; the user may have added or corrected someone. Repeat preferred-name and legal-name matching before deciding that a new alias is required.

- If the response confirms a genuinely new nickname, record `original submitted nickname → corresponding full Chinese name` in the current-year nickname section. Add a note such as `Confirmed by the user during attendance sync for <event> on YYYY-MM-DD`. Do not retry `appendTableRows`; it fails on this document with `Spreadsheet not found`. Use `appendMarkdown` after the nickname table and follow the existing supplemental-entry format: `Nickname | Corresponding Full Chinese Name | Notes`.
- If the person is not in the roster, confirm that they are a new or unregistered player. Use the confirmed name as the attendance row identifier and tell the manager to add the person to the roster manually. Do not add roster rows from this skill.

### 4. Write Attendance

Immediately before writing, reread the selected tab’s current name column and row count. Do not rely on row numbers read earlier in the conversation.

- If the player already has a row, write `1` only in the selected event column. Do not touch any other cell, including summary fields.
- If the player has no row, append one using the roster `prefered name`, write `1` in the event column, and add the appropriate summary formula only for this new row. For training, sum the dynamically detected training columns. For league or cup tabs, count `1` values across the dynamically detected `报名/出场` columns. Never hard-code column ranges.
- For anyone absent from the submitted list, write nothing. Do not write `0`, blank strings, or clear existing cells.

### 5. Report

Use plain language suitable for a nontechnical Slack user. Report only the outcome:

- the matched training session or match, date, and opponent where relevant;
- the number of attendees recorded;
- any names that required confirmation and any nickname mappings saved; and
- any attendees missing from the roster who require manual follow-up.

If the submitted and matched dates differed, state the difference explicitly.
