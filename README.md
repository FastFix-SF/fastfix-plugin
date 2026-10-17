# FastFix MCP

**FastFix lets you build your own tools around what you actually need, not what a software company decides you need.**

<img src="assets/logo.png" alt="FastFix logo" width="96" />

Connect FastFix to your AI agent, describe the tool you want, and you get a clickable preview to try. Once you say yes, it's saved to your dashboard with your real data. Change it as often as you like.

Your bots keep your tools up to date as they work. You can ask your bots anything about your data, and they answer from it. You see what every bot is doing, with proof screenshots, and you can see what your team members are working on too.

FastFix is a hosted MCP server. You connect it once and sign in with your phone number. There's no server to run and no API keys to copy.

- Server URL: `https://fastfix.ai/dashboard/mcp`
- Auth: OAuth 2.1 with a phone sign-in code
- Docs and screenshots: https://fastfix.ai/dashboard/connector

## What your agent can do

**Build your own tools**
- `request_tool`: describe a tool and get a clickable preview first.
- `edit_tool`: change one part of the preview.
- `approve_tool`: save it to your dashboard once you've said yes.
- `list_my_tools`: see the tools you've built.

**Tasks and routines**
- `list_tasks`, `add_task`, `update_task`, `complete_task`: see, add, change and finish to-dos and ongoing routines.
- `add_task_comment`, `list_task_comments`: leave a note for you, or ask you for a decision.
- `attach_proof`, `update_proof`, `delete_proof`: attach a screenshot showing the work was done.
- `report_routine_run`: log each run of a recurring job, and flag it if it's stuck.

**Calendar**
- `list_events`, `add_event`, `update_event`: read and plan your schedule, with in-person or online details and what to bring.

**Subscriptions and invoices**
- `list_subscriptions`, `upsert_subscription`: track monthly tool costs and invoices.
- `update_subscription_status`: track a cancellation or refund you asked for, step by step.

**Connectors**
- `list_connectors`, `install_connector`: see which apps your agents use and add verified ones.

Your agent only sees your own account. Nothing ships to your dashboard without your approval.

## Setup

**Claude Code** (one command):
```bash
claude mcp add --transport http fastfix https://fastfix.ai/dashboard/mcp
```

**Cursor and any other MCP client:** install this plugin from the marketplace, or add [`mcp.json`](mcp.json) to your config:
```json
{
  "mcpServers": {
    "fastfix-dashboard": { "url": "https://fastfix.ai/dashboard/mcp" }
  }
}
```

The first time your agent uses a FastFix tool, a sign-in window opens. Enter your phone number and the code we text you. Then tell your agent **"set up FastFix"**. It runs the bundled [`fastfix-setup`](skills/fastfix-setup/SKILL.md) skill and makes its first update with a proof screenshot.

Try asking:
- "Build me a tool that shows my open jobs by customer."
- "What did my bots finish this week?"
- "Add a task to renew the domain on Friday."

## Privacy and security

- FastFix never sells your data or uses it to train AI.
- Sign-in uses OAuth. This repo contains no keys or secrets.
- You can export or delete your account anytime at https://fastfix.ai/dashboard/account.
- [Privacy Policy](https://fastfix.ai/privacy), [Terms](https://fastfix.ai/terms) and [security contact](https://fastfix.ai/.well-known/security.txt).

## Pricing

Free trial, then $50/month.

## Support

[fastfix.ai/support](https://fastfix.ai/support) or sebastian@fastfix.ai

## License

MIT. See [LICENSE](LICENSE). This repo holds the plugin manifest, connector config, setup skill and legal pages. The FastFix server itself is hosted and is not part of this repo.

FastFix is operated by FASTRACKFIX, CORP., a California corporation, San Francisco, CA.
