---
name: calendar-sync
description: Synchronize training, league, cup, and scrimmage schedules from the current team spreadsheet, plus publishable social-media tasks from the Todo list, to the Baltimore Origin Team Calendar. Create or update events; never delete obsolete events. Instead, prefix obsolete titles with the established Chinese void marker and report them for manual cleanup. Called by todo-reminder and permit-sync, and available for manual testing.
---

# Team Calendar Sync

This reusable calendar workflow was separated from `todo-reminder` on 2026-07-27 so weekly reminders, permit processing, and manual runs use the same logic.

Every invocation performs a complete reconciliation. The caller decides when to invoke it; this skill does not attempt partial or incremental synchronization.

## Workflow

1. Synchronize two source categories to Google Calendar `jhu.cssa.soccerclub@gmail.com`. Always reread the live sources; never rely on conversation memory.

   a. Check [Documents/DRIVE_STRUCTURE.md](../../../Documents/DRIVE_STRUCTURE.md) for the current-year spreadsheet ID, then use `list_sheets` on `2026-2027 球队信息表` (“2026–2027 Team Information”; current known ID `1B5bOM1dIqB4AOKCarZp68coushRbY7-3VOkKHixk1SM`). Identify the current semester’s training, league, and cup tabs. Their names may change. Parse every session or match, including scrimmages, for date, time when available, and location. Training and cup structures use one column or variable column group per event, so parse the live headers rather than assuming a fixed count.

   b. Read the relevant seasonal sections in the management guide, document ID `1xRkUw6-DVUDVe_5kVmElv5UVeiR_ETVxIhIeVU5ln_A`. Extract each supporting action’s name, weekday or relative offset, exact time when provided, and owner. Users may revise these rules at any time, so reload them every run.

   c. For each training session, calculate the supporting events specified by the live guide, such as training sign-up, training-plan confirmation, team assignment publication, and attendance maintenance.

   d. For each match or scrimmage, calculate the supporting events specified by the live guide, such as match sign-up, sign-up deadline, roster publication, post-match data maintenance and MVP selection, and match report plus media collection.

   e. Read `Sheet1` of the Todo list, spreadsheet ID `1VlysVi6CLT7BT4GbUaVgur_NZlcWfMo9hp1hIQx9a7w`. Select rows whose status is not `待讨论`, `已完成`, or `不再需要` (“To Discuss,” “Completed,” or “No Longer Needed”) and that clearly represent social-media content to publish or produce. Do not include unrelated administrative or maintenance work merely assigned to the social-media manager. If the notes provide a precise weekday or time, use it. Otherwise schedule the reminder for 8:00 p.m. `America/New_York` on Friday of the due-date week.

2. Reconcile calculated events with existing calendar events by title and date.

   `mcp__google-docs__deleteEvent` is intentionally blocked by the deny rule in `.claude/settings.local.json`. Never call it or try to bypass that restriction.

   - If an event exists and matches the source, leave it unchanged.
   - If it is missing, create it with `createEvent`.
   - If it exists but date, time, location, or description differs, update it in place with `updateEvent`.
   - If a previously synchronized event no longer qualifies, do not delete it. Change only its `summary` to `【已作废,待删除】<original title>` (“Void—Pending Deletion”) and record its new title, date, and `htmlLink` for the caller. Do not prefix it again if already marked. When matching, compare against the title after removing this exact prefix.

   **Required verification:** After all writes, independently assemble the complete expected-event list from the live sources, with title, date, and time. Call `listEvents` again for the entire relevant date range; do not reuse earlier results. Compare every expected event with the fresh calendar state, including key description details. Correct missing or incorrect events immediately. Similar tasks within one batch are especially prone to crossed dates or notes.

   The no-delete rule is a deliberate safety boundary. Previous runs showed that even increasingly thorough reconciliation logic could misassociate similar tasks or incorrectly classify many valid events as obsolete. Human review is therefore required before deletion.

3. Event construction rules:

   - Whenever a time is available, create a timed event using `dateTime` and `America/New_York`, not an all-day `date` event.
   - Supporting-action and social-media reminders last 15 minutes.
   - Training sessions and matches use their actual duration. If a match has no end time, estimate 1.5 hours and state in the description that the duration is estimated.
   - Use an all-day event only when no time information exists, and explain that reason in the description.

4. If the source rows are visibly labeled as test data or clearly fictitious, prefix generated event titles with the established marker `[测试]` (“Test”). Stop using the prefix after real data replaces the test data.

5. Report to the caller, not directly to the final user:

   - counts and identities of created and updated events, grouped by training session, match, scrimmage, or social-media task; and
   - every event newly marked `【已作废,待删除】`, including title, date, and `htmlLink`, even if the count is zero.

The caller, such as `todo-reminder` or `permit-sync`, is responsible for the final Slack summary and manual-cleanup notice.
