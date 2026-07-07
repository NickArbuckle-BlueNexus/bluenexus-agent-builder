---
name: bluenexus-agent-builder
description: >-
  Guided end-to-end builder for standing up an agent on the BlueNexus platform (app.bluenexus.ai).
  Use this whenever the user wants to create, build, set up, spin up, launch, or "make" a new agent
  on BlueNexus — e.g. "build me a BlueNexus agent", "create an agent on BlueNexus", "help me make an
  agent", "set up a new assistant/bot", or when they describe an assistant they want to stand up on
  BlueNexus — even if they don't say the word "skill" or name every setting. It runs a structured
  brief interview (audience, guardrails, core workflow, connectors, dashboard) and then builds the
  agent in the BlueNexus studio: identity, guardrails, skills, onboarding, dashboard page, and publish.
  Do not start clicking in the studio until the brief interview is done.
compatibility: >-
  Requires a browser-automation tool (e.g. Claude-in-Chrome) and the user to be logged into their
  own BlueNexus account at app.bluenexus.ai. The builder drives the studio UI on the user's account.
---

# BlueNexus Agent Builder

You help someone go from "I want an agent that does X" to a **published, working BlueNexus agent** — by interviewing them for a proper brief, then building it in the studio the way an experienced builder would. This skill is platform knowledge, not domain knowledge: it works for a sales assistant, an internal ops bot, a support agent, a personal helper — anything. Stay domain-agnostic; the user supplies the domain.

## Operating model (read first)

- **You build on the user's own BlueNexus account**, in the studio at `app.bluenexus.ai`, using a browser-automation tool. You are not the account owner — you're driving their logged-in session, so treat account-changing and publish actions as things you confirm before doing.
- **Interview before you build.** A vague brief produces a vague agent. Get the brief first (Phase 1). Only then open the studio (Phase 2).
- **Ask one thing at a time.** Builders get overwhelmed by a wall of questions. Ask, listen, let the answer shape the next question. The audience answer branches much of the rest.
- **Everything you build stays in draft until you publish.** So you can build freely and only cut a public version at the end, once the user has seen it.

## Phase 1 — The brief interview

### Start here: get a detailed brief, and don't stop until it's enough

Open by asking the user for a **detailed brief** of the agent they want — in their own words: what it's for, who uses it, what it should do for them. Then **assess whether that brief is actually sufficient to build from**, across the five dimensions below. It usually won't be on the first pass — people describe the *idea* but leave out audience, guardrails, the exact workflow, the data it needs, or what they want to see. That's expected.

**When the brief is thin, keep asking.** Probe the specific gaps with concrete, one-at-a-time follow-ups until you could hand the brief to another builder and they'd build the same agent. Do not proceed to Phase 2 on a vague brief — an underspecified brief is the single biggest cause of a disappointing agent. A good rule of thumb: if you can't yet name the audience, write the guardrail scope in one sentence, list the workflow steps, and list the connectors, you don't have enough yet — ask more.

Capture these five things (explain each briefly in plain language — many builders are non-technical):

### 1. Audience — Individual, Team, or Public?
The first and most load-bearing answer; it changes onboarding, sharing, and guardrail defaults.
- **Individual** — just for the builder. Minimal onboarding; no need to prompt strangers to connect things.
- **Team** — colleagues use it. Needs onboarding (greeting, discovery questions, which connectors teammates should connect) and appropriate guardrails.
- **Public** — anyone can use it via a public URL. Needs the strongest onboarding and guardrails, a public handle, and a decision on usage caps. **Honesty:** a public visitor needs their **own** BlueNexus account to connect their own connectors, and connecting a connector from a fresh/separate account can depend on that account's entitlements — verify the connect flow on the real account before promising a frictionless "any stranger connects their own data" experience (see `references/build-reliability.md`).

### 2. Identity
Name, a one-line role/title, and a short description of what it's for. A warm first-conversation welcome message. For Public/Team, a public handle (sets the public web address).

### 3. Guardrails and safety checks
Ask what the agent should refuse or stay away from, and what it must never do (e.g. never give binding advice, never take an irreversible action for the user). Then set **two layers**:
- **Response constraint + blocked keywords + rejection message** (the screening layer).
- **The Rules field** (model-level scope) — put the core "only help with X; refuse everything else" scope here. This is the authoritative, most dependable place for scope, so always set it, and test an off-scope probe afterward (see `references/build-reliability.md`).
Also ask about **safety checks** — points in the workflow where the agent should stop and confirm with the user before proceeding (before sending, filing, spending, or anything irreversible).

### 4. The core initial workflow → onboarding + connectors
Get the user to **describe, step by step, the main job the agent runs** the first time someone uses it. This single answer drives two build decisions:
- **Onboarding discovery questions** (Team/Public): the questions the agent asks a new user up front to get the context it needs. Derive these from the workflow's inputs.
- **Which connectors to prompt for**: for each data source the workflow needs, add an onboarding "connection" with (a) a plain-English reason ("Connect X so I can do Y") and (b) a trust line naming exactly what it reads and nothing else. Describe the *capability* ("the user's cloud file storage for their documents"), not just a brand — the platform matches it to whatever the user has connected. Keep the connector set as small as the workflow truly needs.

### 5. The initial dashboard brief
Ask for a **basic brief** of a dashboard/page the agent should surface (what it shows, roughly how it's laid out). Then tell the user two things they'll do *with the agent* after the first build:
- **Paste reference screenshots** into the agent chat so you can refine the dashboard together ("make it look like this, mapped to my data") — screenshot-driven iteration is how you get it right.
- **Have the agent save the dashboard build as one of its own skills**, so it can regenerate that same template, personalized, for every future user. This is the key to a per-user dashboard: the agent rebuilds the page for each person rather than sharing one static page.

When you have all five and the brief is genuinely sufficient, **play it back** in a few lines and get a yes before building.

## Phase 2 — Build it in the studio

Open `app.bluenexus.ai`, go to the agent (create a new one for the chosen audience), then work through the studio sections in this order. Full click-by-click detail is in **`references/build-sequence.md`** — read it before you start.

1. **Identity** — name, title, description, welcome message, public handle.
2. **Behaviour → guardrails** — response constraint + blocked keywords + rejection message, AND the Rules field for authoritative scope.
3. **Skills** — build the core workflow skill(s). If there's a dashboard, build a **dashboard-builder skill** that templates the page (see below and the pages reference).
4. **Onboarding** — connections (each with reason + trust line + priority), first-conversation toggle on, discovery prompts, and a privacy disclosure prompt.
5. **Knowledge** — add any reference sources the agent should ground on.
6. **Pages** — the dashboard. Building pages has strict sandbox rules — read **`references/pages-and-dashboards.md`** first, or the page will look right but not work.
7. **Publish** — cut a version, set public access + web chat, decide usage caps. Drafts don't reach users until you publish. Verify the public toggle stuck.

After building, **test it yourself in the studio Test chat** with the trigger phrase the workflow implies (e.g. "help me do X"), confirm the skill fires and the dashboard builds, then hand back to the user to review before publishing.

## Phase 3 — Dashboards done right

Dashboards are where builds most often fail, because BlueNexus pages render in a **restricted sandbox**. The essentials (full detail in `references/pages-and-dashboards.md`):
- **Full-page writes only — never partial patches.** Partial edits can mis-render (markup may be escaped into visible text).
- **Inline CSS with hex colours; no Tailwind class names, no external CDN/font/image/script.**
- **Don't depend on in-page click JavaScript.** Treat the page as a rendered view you **repaint** whenever state changes — that's how you get "the dashboard fills in as the user connects data" without relying on interactivity the sandbox may not run.
- **Fit one screen; the embedded view may not scroll.** Use fixed, compact element heights.
- **Recreate-a-design method:** the user pastes screenshots into the agent chat → the agent maps the design to its own data and skills → builds via a full-page write → iterate. Then the agent saves that build as a skill so it reproduces per user.

## Guiding principles

- **Domain-agnostic.** Never bake in any one domain. Mirror back the user's domain in their words.
- **Honesty over hype.** If something the user wants isn't reliably supported yet (e.g. a true anonymous-stranger connector flow), say so and offer the closest workable path. `references/build-reliability.md` lists what to verify.
- **Confirm side-effecting and publish steps.** Publishing to a public URL and changing account settings are real actions — say what you're about to do and get a yes.
- **Leave the user able to self-serve.** End by telling them the trigger phrase that runs their agent, and the two dashboard follow-ups (screenshots to refine, save-as-skill to reproduce).

## References
- `references/build-sequence.md` — click-by-click studio build order for every section.
- `references/pages-and-dashboards.md` — the page sandbox rules, the screenshot-recreate method, and how to build a progressive/personalized dashboard.
- `references/build-reliability.md` — the verification habits that keep a build solid, so you don't get surprised mid-build.