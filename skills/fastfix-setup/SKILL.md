---
name: fastfix-setup
description: Use when the user says "set up FastFix", or right after the FastFix plugin is installed (or to re-check an install). Connects the fastfix-dashboard MCP, applies connector rules, creates the 4 routines, and runs each once with proof.
---
# FastFix dashboard install playbook

Trigger: the user says "set up FastFix" (or the plugin was just installed). Goal: every install is identical. Do the steps in order. Do not skip verification.

## 1. Connect the MCP
- Server name: `fastfix-dashboard`
- URL (current, from the plugin's mcp.json): `https://fastfix-dashboard.vercel.app/dashboard/mcp`
- PLACEHOLDER: the URL will move to `https://fastfix.ai/dashboard/mcp`. Update this line (and every install) when it does.
- Auth: OAuth on first tool call (owner signs in / starts free trial). No API keys.
- Confirm: the connector lists its tools (add_event, list_events, update_event, add_task, update_task, complete_task, list_tasks, add_task_comment, list_task_comments, attach_proof, update_proof, delete_proof, report_routine_run, list_subscriptions, upsert_subscription, update_subscription_status, list_connectors, install_connector).

## 2. Connector instructions (paste exactly)
- Gmail: "Draft only. Never send unless the owner said sí to that exact draft. At most 2 drafts per pass."
- Google Calendar: "Never create, move, or delete an event unless the owner said sí to that exact change."
- FastFix dashboard: "Write only real data with its source (source, source_url, why). Never invent tasks, events, or subscriptions. Every routine run ends with report_routine_run plus one proof image."

## 3. Create the 4 routines (owner's local time)
For each: create the routine on the agent's scheduler AND a matching dashboard routine with add_task(routine=true, title=<routine name>, list="Routines", source=<your bot's name>). Save the returned task id; that is the routine id for report_routine_run.

### Schedule sync — daily 6:42 AM
Prompt:
"Read all of the owner's calendars for today through the next 7 days. For each event not already in list_events, call add_event with title, start/end, location or meeting_url, with, and short whats_going_to_happen bullets from the invite notes. Update changed events with update_event. Read-only on the calendar. End with report_routine_run + one proof image."

### TODO task sweep — daily 7:15 AM
Prompt:
"Scan new emails since the last sweep across the owner's connected inboxes. For each real action item, add_task with title, source ("Email from Name <addr>"), source_type=email, source_url, why, area. Skip anything already in list_tasks. No fake or guessed tasks. Up to 2 Gmail reply drafts if clearly needed. End with report_routine_run + one proof image."

### Ask Grok Bot queue — every 3h, 8:17 AM to 8:17 PM (8:17, 11:17, 2:17, 5:17, 8:17)
Prompt:
"Call list_tasks(delegated=true). For each task: if it has an owner comment (needs_reply), read list_task_comments and answer. Otherwise update_task(stage working / assignee bot) and do it. Done: attach_proof (image + one-line caption) then complete_task(summary, proof_url). Need the owner (decision, login, payment): add_task_comment with one plain question (hands it back). Never send email or change the calendar without the owner's sí. End with report_routine_run + one proof image."

### Subscriptions triage — Monday 8:55 AM
Prompt:
"Find subscription renewals, receipts and invoices in email since last week; upsert_subscription each (name, kind, amount, status, note, domain; flag renews_soon / unused). Then list_subscriptions(cancel_requested=true): for each, draft the cancellation or refund request (Gmail draft only, max 2), and update_subscription_status (canceling / canceled / refund_asked / refunded) with a short note and proof_url when available. End with report_routine_run + one proof image."

## 4. Proof rules (every run)
- Every run ends with: report_routine_run(id, did, result, status "ok" | "stuck", proof_url).
- status "stuck" = owner must act (shows "Your turn").
- ONE proof image per run, ONE line caption/title. Example: "Task sweep: 21 open, 3 added today".
- Keep it cheap: first run = scope of work once (2-3 shots allowed); later runs = one final-result image only.
- How to make the image:
  1. Build a small HTML card from real dashboard data (list_tasks / list_subscriptions / list_events).
  2. Render headless: `google-chrome --headless=new --no-sandbox --hide-scrollbars --window-size=800,760 --screenshot=/tmp/x.png file:///path/card.html`; crop, save JPG q80 (<100 KB).
  3. Host at a public https URL (the deployment's proof site, path `/proof/<routine-slug>-<YYYY-MM-DD>.jpg`). The dashboard only accepts https URLs, no upload.
  4. Pass it as proof_url (report_routine_run) or attach_proof(task_id, url, caption).

## 5. Naming conventions
- Routines: exactly "Schedule sync", "TODO task sweep", "Ask Grok Bot queue", "Subscriptions triage" (same on scheduler and dashboard). Keep "Ask Grok Bot queue" as the name even if your bot has another name; the dashboard expects it.
- Tasks: "<Who/Vendor>: <verb> <thing>" e.g. "Namecheap: renew fastfix.ai". Always set source + why.
- Proof files: `<routine-slug>-<YYYY-MM-DD>.jpg` (schedule-sync, task-sweep, ask-queue, subs-triage).
- Proof captions: "<Short routine>: <count/result>" e.g. "Task sweep: 21 open, 3 added today".
- Run logs: did = what was done with a number; result = the outcome in a few words.

## 6. Fresh-install verification checklist
- [ ] Connector connected; tool list shows all dashboard tools.
- [ ] Connector instructions pasted for Gmail, Google Calendar, dashboard.
- [ ] Test: add_task "Install test: delete me" -> appears on dashboard -> complete_task with proof_url -> shows Proof ✓.
- [ ] 4 routines created with exact names + schedules + owner time zone.
- [ ] Each routine run once manually; each logged a report_routine_run with one proof image.
- [ ] Dashboard shows data: Schedule (events), TODO (tasks + routines), Subscriptions.
