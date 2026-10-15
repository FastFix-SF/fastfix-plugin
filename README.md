# FastFix Dashboard plugin

FastFix is the hardware store for AI agent tools. This plugin connects your agent to the **FastFix dashboard** (https://fastfix-dashboard.vercel.app): one place for your calendar, tasks, subscriptions, and recurring routines.

## What it adds
A hosted MCP server (`fastfix-dashboard`) with tools such as:
- `list_events`, `add_event` – calendar
- `list_tasks`, `add_task`, `update_task`, `complete_task`, `attach_proof` – tasks
- `list_subscriptions`, `upsert_subscription` – subscriptions
- `report_routine_run` – routine/automation run logs
- `list_connectors`, `install_connector` – connectors

## Setup
1. Install the plugin from the Cursor Marketplace.
2. The first time the agent calls a FastFix tool, Cursor opens the FastFix sign-in (OAuth). Sign in or start your free trial.
3. Ask your agent, e.g. "What's on my FastFix calendar today?" or "Add a task to renew the domain on Friday."

No API keys are stored in this repo; authentication is OAuth against `https://fastfix-dashboard.vercel.app`.

## Pricing
- Free trial, then **$50/month**.
- Paid setup clients get premium customization of their dashboard.

## Links
- Privacy: https://fastfix.ai/privacy
- Terms: https://fastfix.ai/terms

## License
MIT – see [LICENSE](LICENSE).
