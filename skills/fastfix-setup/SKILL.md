---
name: fastfix-setup
description: Use when the user says "set up FastFix", or right after the FastFix plugin is installed (or to re-check an install). Connects the fastfix-dashboard MCP, applies connector rules, creates the 4 routines, and runs each once with proof.
---
# FastFix dashboard install playbook

Trigger: the user says "set up FastFix" (or the plugin was just installed). Goal: every install is identical. Do the steps in order. Do not skip verification.

## 1. Connect the MCP
- Server name: `fastfix-dashboard`
- URL (primary, also in the plugin's mcp.json): `https://fastfix.ai/dashboard/mcp`
- Tools list (public, 22 tools): https://fastfix.ai/dashboard/mcp/tools
- Legacy: existing connections to `https://fastfix-dashboard.vercel.app/dashboard/mcp` still work; every NEW install must use `https://fastfix.ai/dashboard/mcp`.
- Auth: OAuth on first tool call (the owner signs in with a phone number and a one-time code, then approves the connection). No API keys.
- Confirm: the connector lists its tools (add_event, list_events, update_event, add_task, update_task, complete_task, list_tasks, add_task_comment, list_task_comments, attach_proof, update_proof, delete_proof, report_routine_run, list_subscriptions, upsert_subscription, update_subscription_status, list_connectors, install_connector, request_tool, edit_tool, approve_tool, list_my_tools).

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

### Ask Grok Bot queue — recurring; self-scheduling (about every 15 min while work is queued, about every 5h when empty)
Source: `docs/bot-routine.md` in FastFix-SF/fastfix-dashboard (verbatim). Needs the FastFix dashboard MCP connector. Nothing calls the bot; it polls.
Prompt:
```text
FastFix queue routine. You are connected to my FastFix dashboard (MCP). Work the tasks I hand you.

1. Call list_tasks with delegated=true. That is your queue: open tasks I handed you (status "waiting"
   or "working") and tasks where I commented after your last reply (needs_reply=true). Read each
   item's recent_comments; call list_task_comments if you need the whole thread.

2. For each task, one at a time:
   a. If it is "waiting", call update_task with stage="working" so I see "Grok Bot is on it".
   b. Do the work with your tools and connectors. Use the title, note, source, why and my comments.
      If I answered a question of yours, use the answer.
   c. Finish it in one of two ways:
      - Done: take a screenshot of the result, call attach_proof (task_id, https image url, and a
        caption: one plain line saying what it shows; it is the title under the picture), then
        complete_task (id, one-line summary). Never complete without proof. attach_proof returns a
        proof_id: fix a wrong caption or screenshot with update_proof, remove a wrong or duplicate one
        with delete_proof.
      - You need me (a decision, a login, a payment card, a signature, something only a person can
        do, or anything you are not sure about): call add_task_comment with one short, plain
        question or instruction. That hands the task back to me with your note on the card. Do not
        guess and do not pay, send or sign anything I did not ask for.
      For a progress update that keeps the task with you, use add_task_comment with needs_you=false.
   d. If a task has needs_reply but is not assigned to you, I replied to your question: if my answer
      lets you do it, call update_task with assignee="bot" and do it; otherwise answer with
      add_task_comment.
   e. If a routine has needs_reply, I answered "No, fix it" on one of its proofs: my comment says what
      to fix. Fix it, then attach_proof with a new caption and report_routine_run (or reply with
      add_task_comment if you need more from me).

3. Cancellations: call list_subscriptions with cancel_requested=true. For each subscription:
   a. If its cancel_stage is "requested", call update_subscription_status with stage="canceling".
   b. Cancel it on the company's site or app (use my logins and connectors; if you need me for a
      login, a code or a phone call, say so in the note and keep the stage "canceling").
   c. When it is canceled, take a screenshot of the confirmation and call update_subscription_status
      with stage="canceled", a short note (e.g. "Canceled; access ends Oct 31") and the proof_url.
   d. Ask for a refund when it is fair: it renewed in the last few days, it went unused since the last
      charge, it was a trial that charged, or the company's policy offers one. Then call
      update_subscription_status with stage="refund_asked" and a note saying what you asked for.
      When the refund arrives, set stage="refunded" with the refund email or statement as proof_url.
      Never threaten, never pay a fee to cancel without asking me, and skip refunds that are not fair.

4. Schedule the next run:
   - If anything is still in your queue or in progress (tasks or cancellations), check again in
     about 15 minutes.
   - If the queue is empty, check again in about 5 hours.

Keep comments short and specific. Report only what you actually did.
```

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

## 7. Build the owner's first tool (optional, at the end of setup)
FastFix is a tool builder: the owner describes a tool and gets a clickable preview before anything is saved.
1. Ask the owner what they want to see or track (for example "my open jobs by customer" or "subscriptions by renewal date").
2. Call request_tool with that description. It returns a clickable HTML preview. Show it to the owner and let them click through it.
3. If the owner wants changes, call edit_tool with one change at a time.
4. Call approve_tool only after the owner says yes to that preview. The tool is then saved to their dashboard with their real data. list_my_tools shows what is already built.
Limits: 3 new tools and 5 edits per account per day.
