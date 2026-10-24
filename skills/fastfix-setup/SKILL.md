---
name: fastfix-setup
description: Use when the user says "set up FastFix", or right after the FastFix plugin is installed (or to re-check an install). Connects the FastFix MCP with OAuth, calls setup_status, and shows one useful read-only tool. Offers optional routines only after the owner agrees. Does not email, cancel, refund, or charge on its own.
---

# Set up FastFix

Use this when the owner says "set up FastFix", or right after this plugin is installed. It works in any host that can call an MCP server. Do these steps in order, then stop unless the owner asks for more.

## 1. Connect

Connect the FastFix MCP server.

- Name: `fastfix-dashboard`
- URL: `https://fastfix.ai/dashboard/mcp`
- Auth: OAuth. The first tool call opens sign-in. The owner approves the connection. There is no API key.

## 2. Check status

Call `setup_status`. It is read-only. Show the owner the result: which of their connections are reporting, which have a page ready, and which are waiting. If it lists open owner prompts, show those too. Do not call `request_owner_prompt` or `resolve_owner_prompt` unless the owner said yes to that exact prompt.

## 3. Show one useful tool

Show one tool the owner can use right away. Call a single read-only tool and show what it returned. If they named what they care about, call that tool (`list_tasks`, `list_events`, `list_subscriptions`, or `list_my_tools`). If they did not, call `list_tasks`. Do not create, edit, or delete anything in this step.

## 4. Optional routines

Offer scheduled routines. Create one only after the owner says yes to that routine by name. Do not create any as part of setup unless they already said yes.

A routine may read data the owner has already connected and save it on their FastFix dashboard. It must not go further on its own:

- Do not sweep email, draft a message, or send a message unless the owner said yes to that exact action.
- Do not cancel a subscription, ask for a refund, or charge anything unless the owner said yes to that exact action.
- Do not create, move, or delete a calendar event unless the owner said yes to that exact change.

## 5. Private data

Proof images stay in the owner's private account. Never post them at a public URL. Never put private data where other people can open it.

Do not act on the owner's accounts, messages, payments, or files without an explicit yes for that exact action.
