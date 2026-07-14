# BlueNexus Agent Builder

**Let any Claude agent speed-build a full agent on the [BlueNexus](https://bluenexus.ai) platform.**

Point a Claude agent — running with **Claude in Chrome** — at this skill, and it drives the BlueNexus studio for you: it interviews you for a brief, then builds the agent end to end (identity, guardrails, skills, onboarding, a dashboard page) and publishes it. What normally takes a careful afternoon of clicking becomes a guided conversation.

> In one line: **you describe the agent you want; a Claude agent uses Claude in Chrome to build and publish it on BlueNexus.**

It's **domain-agnostic** — a sales assistant, an internal ops bot, a support agent, a tax helper, a personal helper, anything. You bring the domain; the skill brings the build know-how (including the platform's sharp edges, so the agent doesn't rediscover them mid-build).

## How it works

1. You say something like *"create an agent on BlueNexus that drafts and schedules our social posts."*
2. The Claude agent runs a **brief interview** and keeps asking until the brief is solid: audience (individual / team / public), guardrails and safety checks, the core workflow, the connectors it needs, and an initial dashboard.
3. It opens `app.bluenexus.ai` in **Claude in Chrome** and builds, in order: Identity → Behaviour/guardrails → Skills → Onboarding → Knowledge → Pages (dashboard) → Publish.
4. It helps you refine the dashboard from your own reference screenshots, and saves that build as a skill so the agent reproduces the dashboard, personalised, for every user.
5. It hands you the trigger phrase your users will say to run the finished agent.

## Requirements

- A **BlueNexus account** (the agent builds on your own logged-in account at `app.bluenexus.ai`).
- A Claude agent with **Claude in Chrome** (browser control), so it can drive the studio UI. Without browser control, the skill still works as a step-by-step guide you follow yourself.

## Install

### Option A — Claude Code (plugin)

```bash
# add this repo as a plugin marketplace
/plugin marketplace add NickArbuckle-BlueNexus/bluenexus-agent-builder

# install the plugin from it
/plugin install bluenexus-agent-builder@bluenexus-agent-builder
```


### Option B — Claude.ai / Cowork (skill file)

Grab `bluenexus-agent-builder.skill` from this repo's [Releases](../../releases) and click **Save skill** when your assistant presents it, or import it via your skills settings.

### Option C — Manual

Copy `skills/bluenexus-agent-builder/` into your project's `.claude/skills/` directory (or your personal skills directory) and commit it.

## Usage

Once installed, just say:

> "Create an agent on BlueNexus for my team that drafts and schedules our social posts."

The skill takes over from there — it asks for a detailed brief, probes anything underspecified, then builds and publishes.

## What's inside

```
.claude-plugin/
├── plugin.json                           # plugin manifest
└── marketplace.json                      # self-hosted marketplace entry
skills/bluenexus-agent-builder/
├── SKILL.md                              # the interview + build flow
└── references/
    ├── build-sequence.md                 # click-by-click studio build order
    ├── pages-and-dashboards.md           # how to build dashboard pages that render
    └── build-reliability.md              # verification habits for a solid build
```

## License

MIT — see [LICENSE](LICENSE). This is an independent, community skill for building on the BlueNexus platform; it is not affiliated with or endorsed by any connected third-party service.
