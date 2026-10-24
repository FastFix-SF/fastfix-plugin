# FastFix MCP

**Run your company by saying only yes.**

<img src="assets/logo.svg" alt="FastFix logo" width="96" />

FastFix is for any business owner. Connect it to an AI agent, describe the tool the owner wants, and the agent returns a clickable preview. Once the owner says yes, it is saved to the dashboard with real data. Change it as often as needed.

Bots keep those tools up to date as they work. The owner can ask the bots about the data, and they answer from it. The dashboard shows what every bot is doing, with proof screenshots, and what team members are working on.

FastFix is a hosted MCP server. Connect it once and sign in with a phone number. There is no server to run and no API keys to copy.

- Server URL: `https://fastfix.ai/dashboard/mcp`
- Server version: 1.2.0+92c27d1.a0496a9ad263 (27 tools)
- Machine-readable tool list: https://fastfix.ai/dashboard/mcp/tools
- Auth: OAuth 2.1 with a phone sign-in code
- Docs and screenshots: https://fastfix.ai/dashboard/connector

## What the agent can do

Each line is that tool's purpose on the live server (tool list hash `a0496a9ad263`). The label is the live annotation: Read (`readOnlyHint`), Write, or Sensitive write (`destructiveHint`). The full description is on the tool list linked above. The agent only sees the owner's own account. New tools and layout changes ship only after the owner approves them. Daily data refreshes go straight to the bot's own tool. No tool on this server charges money.

**Build your own tools**

- `request_tool` (Write): Build a private dashboard tool. The owner describes it and gets a clickable HTML preview before anything is saved. Limit: 3 new tools per account per day.
- `edit_tool` (Write): Change one part of that preview. The layout stays the same. Limit: 5 edits per account per day.
- `approve_tool` (Sensitive write): Ship the preview to this account only, after the owner has seen it and said yes.
- `rate_tool` (Write): Record the owner's thumbs up or down. FastFix counts only the layout's shape, anonymously, to improve shared templates. Never the owner's words, numbers, or account details.
- `list_my_tools` (Read): List the tools on this account and their status.
- `update_tool_data` (Sensitive write): Save the latest real data into the bot's own tool. No model cost and no edit limit. Every bot tool has three tabs, in order: "Doing now", "To do" (anything waiting on the owner uses the status "Needs your sí"), and "Done" (every card needs proof). With nothing real yet, the tool shows "No data yet".

**Tasks and routines**

- `list_tasks` (Read): List one-time tasks and ongoing routines, with status, notes, comments, and proof.
- `add_task` (Write): Add a task or a routine, including where it came from and why it exists.
- `update_task` (Write): Change a task or routine, including whether the owner needs to act.
- `complete_task` (Write): Mark a one-time task done, or finish a routine. Tasks the owner handed to the bot need a proof screenshot.
- `add_task_comment` (Write): Comment on a task. By default this hands it back to the owner. A progress note can keep it with the bot.
- `list_task_comments` (Read): Read the comments on a task, oldest first.
- `attach_proof` (Write): Attach a proof screenshot. The owner can answer "Is this right?".
- `update_proof` (Write): Change a proof's caption or screenshot.
- `delete_proof` (Sensitive write): Remove a wrong or duplicate proof.
- `report_routine_run` (Write): Log one run of a routine. Status "stuck" means the owner must act ("Your turn").

**Calendar**

- `list_events` (Read): List schedule events, with place or meeting link and what is going to happen.
- `add_event` (Write): Add an in-person or online event.
- `update_event` (Write): Change an event's title, time, place, or plan.

**Subscriptions and invoices**

- `list_subscriptions` (Read): List renewals and invoices, including cancellations the owner asked for.
- `upsert_subscription` (Write): Add or update a subscription or an invoice.
- `update_subscription_status` (Write): Move a cancellation through canceling, canceled, refund asked, and refunded, with a note and an optional proof screenshot.

**Connectors**

- `list_connectors` (Read): List connector apps, whether each one is installed, and whether FastFix has verified it.
- `install_connector` (Write): Add a verified connector for this owner, or remove it.

**Setup**

- `setup_status` (Read): Check which of the owner's bots report to this dashboard: Reporting (a real report in the last 36 hours), Page ready (a page, no real report yet), or Waiting (the last report is older, or the bot has no page yet). Returns the "No data yet" page, the daily routine prompt "Report to FastFix dashboard", and any open owner prompts.

**Owner prompts**

- `request_owner_prompt` (Write): Ask the owner to paste a prompt back into this same bot when only they can reconnect, sign in, allow a permission, or turn a routine back on. The prompt never carries a password, code, or key.
- `resolve_owner_prompt` (Write): Close an owner prompt once that action is done. `setup_status` lists the open ones.

## Setup

**Claude Code** (one command):

```bash
claude mcp add --transport http fastfix https://fastfix.ai/dashboard/mcp
```

**Cursor and any other MCP client:** install this plugin from the marketplace, or add [`mcp.json`](mcp.json) to the config:

```json
{
  "mcpServers": {
    "fastfix-dashboard": { "url": "https://fastfix.ai/dashboard/mcp" }
  }
}
```

The first time the agent uses a FastFix tool, a sign-in window opens. The owner enters a phone number and the code sent by text. Then say **"set up FastFix"**. The agent runs the bundled [`fastfix-setup`](skills/fastfix-setup/SKILL.md) skill: connect with OAuth, call `setup_status`, and show one useful tool. It offers scheduled routines and creates one only after the owner says yes to that routine.

Try asking:

- "Build me a tool that shows subscriptions by renewal date."
- "What did the bots finish this week?"
- "Add a task to renew the domain on Friday."

## Privacy and security

- FastFix never sells data or uses it to train AI.
- Sign-in uses OAuth. This repo contains no keys or secrets.
- The owner can export or delete the account anytime at https://fastfix.ai/dashboard/account.
- [Privacy Policy](https://fastfix.ai/privacy), [Terms](https://fastfix.ai/terms) and [security contact](https://fastfix.ai/.well-known/security.txt).

## Pricing

Free for 7 days, then $50/month. Cancel anytime before the trial ends and you won't be charged. Billing is handled by Stripe.

## Support

[fastfix.ai/support](https://fastfix.ai/support) or sebastian@fastfix.ai

## License

MIT. See [LICENSE](LICENSE). This repo holds the plugin manifest ([`.cursor-plugin/plugin.json`](.cursor-plugin/plugin.json)), connector config ([`mcp.json`](mcp.json)), setup skill and copies of the [privacy policy](legal/privacy.md) and [terms](legal/terms.md). The FastFix server itself is hosted and is not part of this repo.

FastFix is operated by FasTrackFix, Corp., a Delaware corporation, San Francisco, CA. Privacy: privacy@fastfix.ai.
