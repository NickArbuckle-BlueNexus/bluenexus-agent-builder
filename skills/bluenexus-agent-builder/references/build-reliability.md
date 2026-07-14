# Build reliably: what to verify

BlueNexus, like any evolving platform, rewards a **build-then-verify** habit. These are the checks that keep a build solid. The theme is simple: after you set something, confirm it took — don't assume.

## Guardrails — make the Rules field your authoritative layer
Set a response constraint (the screening prompt) **and** put the core scope in the **Rules field**. Scope by **subject matter, not by tool**: "You help with {domain}; politely decline unrelated topics." The Rules field is model-level and the most dependable place for scope, so set it every time — but avoid a blanket "refuse everything else": the model can over-apply it and refuse a legitimate in-domain request just because it uses a connector/tool (e.g. a marketing agent refusing a video-generation connector). State plainly that the agent **may use any connector or tool the user has connected** in service of the domain. **Verify BOTH:** (1) a genuinely off-topic probe is refused, and (2) an in-domain request that relies on a less-obvious connector still proceeds — in Test chat, and again on the published surface if it matters (Test chat and the live bot can behave differently).

## Pages — full-page writes, one screen
Build and refresh pages with a **full-page write** (replace), not partial patches — partial edits can mis-render. Keep everything on one screen with fixed element heights, since the embedded view may not scroll. Don't depend on in-page click scripts; **repaint** the page on state change instead. (Full detail in `pages-and-dashboards.md`.)

## Connectors — verify cross-account access before relying on it
A visitor connects their own data using their **own** BlueNexus account. Whether a given account can connect a given connector can depend on that account's entitlements/approval, so **verify the connect flow on the actual account you'll use** — especially a separate or public visitor — before building a demo or workflow around it. Don't promise a frictionless "any anonymous stranger connects their own data" experience until you've confirmed it on a real second account.

## Settings — re-verify after saving and publishing
Re-open and confirm guardrail text, onboarding fields, and the public-access / web-chat toggles after you save or publish. Changes stay in **draft** until you **publish a new version**; the public URL serves the last published version until then. For a per-user dashboard, remember the page is generated per conversation by the agent's skill — so publishing the **skill** (not a single static page) is what makes the dashboard reproduce for everyone.

## Skills & knowledge — confirm they took
After wizard creation, check the skill list matches what you intended and re-create anything missing. If a knowledge URL won't ingest, add the document as a **file** source instead. If you accept a suggested skill or heartbeat, confirm it actually attached (reload and check) rather than trusting the confirmation.

## Long text into studio fields (automation)
When entering long prompt bodies via automation, the rich-text editor may auto-number lines (turning "1." into an ordered list), and a very long single input can appear to time out even though the text lands. Enter text in chunks, avoid leading "N." numerals if you don't want lists, and screenshot to confirm rather than trusting an error.
