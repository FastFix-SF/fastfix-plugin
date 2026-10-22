# Privacy Policy

**Effective date:** October 17, 2026 · **Last updated:** October 22, 2026

This policy explains how FASTRACKFIX, CORP. (doing business as FastFix and fastfix.ai), a California corporation ("FastFix", "we") handles information across FastFix.ai. **Part A** covers FastFix.ai in general. **Part B** covers the FastFix dashboard ([fastfix.ai/dashboard](https://fastfix.ai/dashboard/)) and the FastFix dashboard connector, which lets AI assistants you choose read and update your dashboard.

## Part A. FastFix.ai in general

### A1. Information We Collect

We collect information you provide directly to us, such as:

- Contact information (name, email, phone number)
- Business information (company name, industry, location)
- Usage data and preferences
- Communication records

### A2. How We Use Your Information

We use the information we collect to:

- Provide and improve our services
- Communicate with you about our services
- Process transactions and send confirmations
- Respond to your requests and provide customer support
- Comply with legal obligations

### A3. Information Sharing

We do not sell, trade, or otherwise transfer your personal information to third parties without your consent, except:

- To trusted service providers who assist us in operating our services
- When required by law or to protect our rights
- In connection with a business transfer or acquisition

### A4. Data Security

We implement appropriate security measures to protect your personal information against unauthorized access, alteration, disclosure, or destruction.

### A5. Your Rights

You have the right to:

- Access and update your personal information
- Request deletion of your data
- Opt-out of marketing communications
- File a complaint with relevant authorities

### A6. Cookies and Tracking

We use cookies and similar technologies to enhance your experience, analyze usage patterns, and improve our services.

### A7. Google User Data (Google Calendar)

When you choose to connect your Google account in FastFix (Settings → Integrations → Connect Google Calendar), we request access to your Google Calendar using the `calendar` and `calendar.events` scopes. We access this data only for the Google account you explicitly connect, and only to synchronize your assigned work:

- Create an event on your calendar when a job is scheduled for you.
- Update the event when the job is rescheduled or edited.
- Remove the event when the job is cancelled or you are unassigned.
- Read your primary calendar's busy times to help schedule around conflicts.

We store only the minimum needed to keep your calendar in sync: OAuth tokens and, per job, the identifier of the calendar event we created (so we can update or delete it). This is retained while your account is connected and is **deleted when you disconnect** (Settings → Integrations → Disconnect) or revoke access at [myaccount.google.com/permissions](https://myaccount.google.com/permissions). We do not use Google Calendar data for advertising, and we do not sell it.

**Limited Use disclosure.** FastFix's use and transfer of information received from Google APIs adheres to the [Google API Services User Data Policy](https://developers.google.com/terms/api-services-user-data-policy), including the Limited Use requirements. We only use the data to provide and improve the calendar-sync feature you enabled, do not transfer it except as necessary to provide that feature, do not use it for ads, and allow no humans to read it except with your consent, for security, or as required by law.

## Part B. FastFix dashboard and AI-assistant connector

### B1. What the dashboard and connector are

The FastFix dashboard is a private workspace for a business owner: a to-do list and routines, a schedule, subscriptions and invoices, connectors, and small "dashboard tools" built for you. The connector is a Model Context Protocol (MCP) server at `https://fastfix.ai/dashboard/mcp`. When you connect an AI assistant (for example Claude, ChatGPT, Cursor, Grok Bot, or Meta (Muse)), that assistant can read and change your dashboard on your behalf, using only the tools described on the [connector page](https://fastfix.ai/dashboard/connector).

### B2. How AI assistants get access

- You sign in with your phone number and a one-time code, then approve the connection. The assistant receives a FastFix access token, never your password or one-time code.
- A connection stays active until you disconnect it or we revoke it (B6). An assistant's access token expires 30 days after it is issued and its refresh token 90 days after it is issued. Each refresh replaces both, and a replaced token stops working within 10 minutes. An assistant in regular use stays connected without you signing in again; one unused for 90 days must be reconnected.
- The assistant can only reach data in your own account. It can't see other FastFix customers' data.
- The AI assistant you connect is provided by a third party (such as Anthropic, OpenAI, Cursor, xAI, or Meta (Muse)) under its own terms and privacy policy. Information the assistant reads from your dashboard is handled by that provider under those terms. FastFix doesn't control how that provider stores or uses it.

### B3. Data we store

| Category | Examples | Why |
| --- | --- | --- |
| Account | Phone number, sign-in timestamps, time zone and display settings | To sign you in and show your dashboard |
| Dashboard content | Tasks, routines, comments, events (title, time, place, meeting link, notes), subscriptions and invoices, connector choices, proof screenshots you or your assistant attach, dashboard tools and their data | The service itself |
| Connection records | Registered assistant apps (name, redirect address), hashed access and refresh tokens, when a connection was last used | To keep connected assistants working and to let you revoke them |
| Tool-builder requests | The description you give when asking for a dashboard tool, the generated tool, and usage and cost counters | To build tools and enforce daily and monthly limits |
| Security and abuse prevention | Short-lived sign-up tickets; rate-limit counters keyed on your phone number and a one-way hash of your IP address | To prevent SMS fraud and brute-force attempts |

**Anonymous tool layout ratings:** when you approve a dashboard tool or rate it thumbs up or down, FastFix counts that rating against the tool's layout shape only (which kinds of sections and fields it has), never your words, numbers, names, phone number or data, to improve the shared tool templates. The shared counts hold no link to your account, and FastFix never sells this data or uses it to train AI.

We don't ask for payment card details in the dashboard, and the dashboard doesn't use advertising or analytics cookies. It keeps your sign-in session in your browser's local storage.

### B4. Retention

- Dashboard content and account data: kept while your account is active, and deleted when you delete your account, or within 30 days of a deletion request (B6). Database backups kept by our database provider roll off on that provider's schedule.
- One-time sign-in codes are handled by our SMS provider and expire within minutes. OAuth authorization codes expire after 5 minutes.
- Connection tokens (stored only as one-way hashes): kept while the connection is active. Replaced tokens are deleted shortly after they are replaced. When you revoke one connection, the records for that connection are deleted. Your account data stays yours until you delete the account.
- Sign-up tickets: 15 minutes. Rate-limit counters: deleted when their window ends (at most 24 hours).
- Tool-builder usage and cost records: kept while your account is active, to enforce limits and prevent abuse, and deleted with your account.
- Server logs kept by our hosting provider: kept for the limited period that provider's service allows, then deleted automatically.

### B5. Subprocessors

| Provider | What it does for FastFix | Data involved |
| --- | --- | --- |
| Supabase | Database, authentication and file storage | All data in B3 |
| Vercel | Hosting and content delivery for fastfix.ai | Requests passing through, including IP address and request logs |
| Twilio | Sending and checking one-time sign-in codes by SMS (Twilio Verify) | Phone number, code delivery status |
| Anthropic | AI model that builds and edits dashboard tools, and turns notes into bullet points | The text of your tool request, the tool being edited, and the text of notes you ask to turn into bullets |

**FastFix never sells your data or uses it to train AI.** We share it with the providers above only so they can do the work listed.

### B6. Disconnecting and deletion

- **Disconnect an assistant:** remove the FastFix connector in that assistant's settings. You can also contact us to revoke every connection. Revoked tokens stop working immediately. When you revoke one connection, the records for that connection are deleted. Your account data stays yours until you delete the account.
- **Access or export:** sign in and open [fastfix.ai/dashboard/account](https://fastfix.ai/dashboard/account) (Settings → Your data and account), then choose *Download my data* for a copy of your dashboard data as a JSON file. You can also email [privacy@fastfix.ai](mailto:privacy@fastfix.ai).
- **Delete your account and data:** on the same page, type DELETE and confirm. Your account, dashboard content and assistant connections are deleted immediately, and every connected assistant is disconnected. Accounts set up for you by FastFix are deleted on request instead: email [privacy@fastfix.ai](mailto:privacy@fastfix.ai) from your account's contact details, or ask from the phone number on the account, and we delete them within 30 days.
- Information an AI assistant already read stays with that assistant's provider. Ask that provider to delete it under its own policy.

### B7. Children

The dashboard is for businesses and isn't directed at children under 13, and we don't knowingly collect their information.

### B8. Changes

We'll post changes on this page and update the date above.

### B9. Security incidents

If FastFix confirms a security incident that affects personal data, we will notify affected users, and the platforms where FastFix is listed, within 48 hours of confirming the incident. Notice to Meta goes to [vendor-incident@meta.com](mailto:vendor-incident@meta.com).

## Contact

Privacy questions: [privacy@fastfix.ai](mailto:privacy@fastfix.ai) · Support: [support@fastfix.ai](mailto:support@fastfix.ai)  
Postal address: FASTRACKFIX, CORP., San Francisco, CA · privacy@fastfix.ai

*This is a copy of the policy published at <https://fastfix.ai/privacy>. If the two ever differ, the version at fastfix.ai is the one that applies.*
