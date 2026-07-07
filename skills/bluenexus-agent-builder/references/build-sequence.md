# Studio build sequence (click-by-click)

Read this before building. The studio lives at `app.bluenexus.ai`. Open the agent, then click **Customize** (or create a new agent) to enter the studio. The left rail has the sections below in roughly this order: Identity, Behaviour, Intelligence, Onboarding, Skills, Built-in tools, Custom tools, Knowledge, Pages, Email, Workflows, Scheduled tasks, Events, Heartbeats, Improve, Interview, Suggestions. Top-right has **Publish** and **Exit studio**; the right panel is a live **Test chat**.

Build in this order — each section feeds the next.

## 0. Create the agent
Use the agent-builder wizard (or "New agent"). Pick the audience path that matches the brief (Individual / Team / "For anyone" = Public). The wizard can pre-create skills and onboarding from a description — useful, but **verify afterward**: review every section and confirm each skill you intended actually got created; re-create anything missing. If you gave a rich description, don't assume — check.

## 1. Identity
- **Name**, **Title** (one-line role), **Description** (what it's for — shown in the picker/marketplace).
- **Welcome message** — the first thing users see; keep it short, warm, and repeatable. If the agent has a headline workflow, invite it here ("…just say 'do X' and I'll get started").
- **Public handle** (Team/Public) — sets the public web address `{handle}.bluenexus.bot` and email. Choose something clean.

## 2. Behaviour → guardrails
Set both layers:
- **Response constraint** — the screening prompt describing what to allow/block (topics, actions). Be specific: name the allowed domain and explicitly list disallowed categories (coding, relationship advice, general chit-chat, etc. — whatever's off-scope).
- **Blocked keywords** + **rejection message** (in the agent's voice) + repeat-violator settings.
- **Rules field** — put the authoritative scope here: "You only help with {domain}. Politely refuse anything else." This is model-level and the most dependable place for scope, so always set it. (Set the response constraint too, for defense in depth.)
- After saving, **re-open the page and confirm the text persisted** before moving on.

## 3. Skills
Skills are reusable instruction blocks the agent pulls in when a trigger matches. Each skill has a **Trigger** (when to fire — be specific and include example phrasings) and **Prompt Instructions** (what to do). Skills can have sub-skills.
- Build the **core workflow skill**: trigger on the user's intent phrases; instructions = the step-by-step workflow from the brief, including any safety checks (stop-and-confirm points).
- If there's a dashboard, build a **dashboard-builder skill** whose trigger includes both the workflow intent and "build/update the dashboard", and whose instructions contain the page template + the sandbox output rules (see `pages-and-dashboards.md`). This is what lets the agent regenerate the dashboard, personalized, for each user.
- Editing long prompt bodies: click into the rich-text editor, select-all, replace, save. Watch for auto-numbering turning your "1." lines into lists — use plain lines or accept the list.

## 4. Onboarding
Drives the first conversation for Team/Public agents.
- **Connections** — one per data source the workflow needs. For each: **What to connect** (describe the capability, e.g. "the user's cloud file storage for their documents", not just a brand), optionally **limit to specific connectors**, and **Priority** (Required = can't work without it; Recommended = helps). Keep the set minimal. In the reason text and in chat, always pair it with a trust line: "We only read: {named items}. Nothing else."
- **First-conversation onboarding** toggle **on**.
- **Onboarding prompts** — the discovery questions derived from the workflow's inputs, plus a **privacy disclosure** prompt ("I only read what you connect, with your permission; I never write or change anything; I can't {irreversible action} for you"). One idea per prompt; they're woven in order.
- Use **Test first-run** to preview the new-user experience.

## 5. Knowledge (if relevant)
Add URL/file/text sources the agent should ground on. They compile into a searchable knowledge base. If a URL won't ingest, add the same document as a **file** source instead.

## 6. Pages (the dashboard)
Only after reading `pages-and-dashboards.md`. In short: build the page via a **full-page write**, inline CSS + hex colours only, no external assets, no reliance on in-page click JS, everything on one screen. Verify it renders (open the artifact, collapse the chat panel for full width). Prefer having the **dashboard-builder skill** own the page build so it reproduces per user.

## 7. Publish
- Open **Publish** (top-right). It shows unpublished-changes status and version history.
- **Publish new version** to lock in the current config — drafts don't reach users until you do.
- **Public access** toggle on; **Web chat** on (gives the `{handle}.bluenexus.bot` URL). Confirm the toggle stuck after publishing.
- **Usage limits** — set caps for public agents unless the user explicitly wants them off (e.g. a short-lived demo).
- Messaging channels (WhatsApp/Telegram) and API/remote-MCP access are optional, under the same modal.

## 8. Verify
In **Test chat**, send the workflow trigger phrase. Confirm: the right skill fires, the dashboard builds and renders, onboarding asks the right questions, and guardrails refuse an off-scope probe. Then hand to the user to review before you publish (or before they go public).