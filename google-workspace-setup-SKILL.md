---
name: google-workspace-setup
description: Set up or repair this project’s Google Drive, Sheets, Docs, and Calendar access for Claude Code. Register the MCP servers using the committed service-account credentials in configs/, verify connectivity, and explain any remaining user action, such as restarting Claude Code or sharing the calendar with the service account. Use for /install or requests to set up, repair, or reconnect this repository’s Google Workspace access.
---

# Google Workspace Setup

Read `Documents/INSTALL.md` in the repository root and execute its instructions in order. That document is written for the agent, not the user. Do not merely summarize it: perform the checks and commands, including verifying `uv` and Node, registering the `google-sheets` and `google-docs` MCP servers, and confirming their status with `claude mcp list`.

If both servers already show `✔ Connected` when the workflow begins, report that briefly and make no unnecessary changes.

After verifying the servers, test Calendar access. The `google-docs` server already includes Calendar capabilities, so do not register another server. Call `mcp__google-docs__listEvents` with:

```text
calendarId: "jhu.cssa.soccerclub@gmail.com"
```

If the response is `Calendar not found`, tell the user to share the **Baltimore Origin Team Calendar** with `manager-agent@project-cea6686e-275d-4e81-8fd.iam.gserviceaccount.com` and grant at least **See all event details** permission.

Report concisely:

- which components were already configured and which were configured during this run;
- the final `claude mcp list` status for both servers;
- whether Calendar access works; and
- if any server was newly registered, that the user must fully exit and reopen Claude Code because MCP tools load when a session starts.
