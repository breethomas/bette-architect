# Bette: Unified PM Operating System

> "Attempt the impossible in order to improve your work." — Bette Davis

**Status:** Planning
**Author:** Bree Thomas
**Last Updated:** 2026-03-08

---

## Vision

One plugin. One install. A complete AI operating system for product managers.

Today, Bette is four separate repos that a user has to discover, clone, install, and wire together manually. That's a parts catalog, not a product. The vision: `bette` becomes a single Claude Code plugin that bootstraps an entire PM working environment — from context architecture to daily workflows — in one install.

---

## The Problem

### For new users:
- Four repos with no clear entry point
- Manual setup: create CLAUDE.md, build reference files, install plugin, clone OS template
- No onboarding — you have to read docs and figure it out yourself
- High barrier to "working like Bree works"

### For Bree (maintainer):
- Four repos to maintain, version, and keep in sync
- Changes that span modules require commits to multiple repos
- Open source contributions are scattered across repos
- Brand is diluted across four URLs instead of one

---

## The Product

### Install Experience

```
/plugin marketplace add breethomas/bette
/plugin install bette@breethomas
```

That's it. Start a new session. Bette is there.

### First-Use Experience

On first session after install, the SessionStart hook detects no existing Bette setup and triggers interactive onboarding:

```
Bette: "I'm Bette — your PM operating system. Let's get you set up.
        This takes about 5 minutes. I'll ask questions, you answer,
        and I'll generate your context files.

        First: What's your role?"

User:   "VP Product at a SaaS company"

Bette: "Got it. Do you manage people directly?"

User:   "Yes, 4 direct reports"

Bette: "What tools does your team use? (Slack, Notion, Linear, GitHub, etc.)"

...

Bette: "Here's what I've set up:
        ✓ Global CLAUDE.md (your preferences and working style)
        ✓ Project directory with reference files
        ✓ People reference (4 direct reports — fill in details)
        ✓ Domains reference (template ready for your product areas)
        ✓ Backlog and goals files
        ✓ 52 skills loaded (PM frameworks, daily ops, quality gates)

        Say 'catch me up' to start your day, or ask me anything."
```

### Ongoing Experience

After setup, every session:
1. SessionStart hook loads the meta-skill
2. Meta-skill knows all available skills and routes automatically
3. User just talks — natural language triggers the right skill
4. Context files provide persistent memory across sessions

---

## Architecture

### Plugin Structure

```
bette/
├── .claude-plugin/
│   ├── marketplace.json          # Marketplace config
│   └── hooks/
│       └── session-start.js      # Bootstrap hook — loads meta-skill
├── plugins/
│   └── bette/
│       ├── .claude-plugin/
│       │   └── plugin.json       # Plugin manifest
│       ├── meta/
│       │   ├── SKILL.md          # Meta-skill: routing + skill discovery
│       │   └── setup.md          # First-use onboarding flow
│       ├── skills/
│       │   ├── architect/        # Context engineering skills
│       │   │   ├── setup/SKILL.md        # Interactive environment setup
│       │   │   ├── health-check/SKILL.md # Audit context architecture
│       │   │   └── reference-gen/SKILL.md # Generate/update reference files
│       │   ├── think/            # PM framework skills (from bette-think)
│       │   │   ├── strategy-session/SKILL.md
│       │   │   ├── four-risks/SKILL.md
│       │   │   ├── shape-up/SKILL.md
│       │   │   ├── spec/SKILL.md
│       │   │   └── ... (30 skills)
│       │   ├── work/             # Quality + session management skills
│       │   │   ├── quality-gates/SKILL.md
│       │   │   ├── test-first/SKILL.md
│       │   │   └── session-check/SKILL.md
│       │   └── operate/          # Daily workflow skills (from bette-os)
│       │       ├── catchup/SKILL.md
│       │       ├── digest/SKILL.md
│       │       ├── prep-1on1/SKILL.md
│       │       ├── backlog/SKILL.md
│       │       ├── focus/SKILL.md
│       │       └── ... (22 skills)
│       ├── agents/               # Specialized analysis agents
│       │   ├── project-health-checker.md
│       │   ├── competitor-researcher.md
│       │   └── ... (7 agents)
│       ├── frameworks/           # Framework documentation
│       ├── templates/            # Scaffolding templates
│       │   ├── global-claude-md.md
│       │   ├── project-claude-md.md
│       │   ├── people-reference.md
│       │   ├── domains-reference.md
│       │   └── company-reference.md
│       ├── thought-leaders/      # PM thinker profiles
│       └── docs/                 # Architecture docs (current bette-architect content)
│           ├── claude-md-architecture.md
│           ├── reference-file-design.md
│           ├── memory-systems.md
│           ├── context-hygiene.md
│           └── skill-architecture.md
```

### Module Mapping

| Module | Source Repo | Skills | Purpose |
|--------|-----------|--------|---------|
| `architect` | bette-architect | 3 (setup, health-check, reference-gen) | Build and maintain context architecture |
| `think` | bette-think | 30 | PM frameworks, strategic sparring |
| `work` | bette-work | 3 | Quality gates, session management, test-first |
| `operate` | bette-os | 22 | Daily workflows: inbox, meetings, backlog, comms |
| `meta` | new | 2 | Routing + first-use onboarding |

**Total: ~60 skills, 7 agents**

### Bootstrap Flow

```
Session starts
  │
  ├── SessionStart hook fires
  │     └── Loads meta/SKILL.md into context
  │
  ├── Meta-skill checks: has user run setup?
  │     ├── No → Trigger meta/setup.md (onboarding)
  │     └── Yes → Ready to route
  │
  └── User speaks → meta-skill routes to correct skill
        ├── "catch me up" → operate/catchup
        ├── "prep for my 1:1 with Carol" → operate/prep-1on1
        ├── "help me think through this feature" → think/strategy-session
        ├── "check my context architecture" → architect/health-check
        └── "quality gates before I push" → work/quality-gates
```

### The Meta-Skill

The meta-skill is the brain. Loaded every session via hook, it:
- Knows every available skill (name + one-line description + trigger patterns)
- Routes natural language to the right skill
- Handles ambiguity ("did you mean X or Y?")
- Provides `/skills` command to list everything
- Detects when no skill matches and falls back to general conversation

Inspired by superpowers' `using-superpowers` pattern but adapted for PM workflows, not just coding.

---

## Migration Plan

### Phase 1: Consolidate (Build the unified repo)
- Create `breethomas/bette` repo
- Merge skills from bette-think, bette-work, bette-os into unified structure
- Move bette-architect docs into `docs/`
- Build meta-skill and setup flow
- Build SessionStart hook
- Write templates for scaffolding

### Phase 2: Ship (Make it installable)
- Plugin manifest and marketplace config
- Test install on clean machine
- Test first-use onboarding flow end-to-end
- Test skill routing across all modules
- Write README with install instructions

### Phase 3: Sunset individual repos
- Update bette-architect, bette-think, bette-work, bette-os READMEs
- Point all four to the unified `bette` repo
- Keep repos alive (redirects, stars, forks) but mark as "moved to bette"
- Update GitHub profile README

### Phase 4: Iterate
- Community feedback on onboarding
- Add skills based on usage patterns
- Evolve templates based on what people actually set up

---

## Open Questions

### Plugin system capabilities
- Can a SessionStart hook reliably detect "first use" vs "returning user"?
- Can the setup skill write files outside the plugin directory (to create user's CLAUDE.md, reference files)?
- How do personal skill overrides work with a unified plugin? (superpowers has this — need to verify)

### Skill organization
- Do users need to know which module a skill belongs to? Or is flat routing (just skill names) better?
- Should `operate` skills ship by default or be opt-in? (Not every PM needs inbox triage — some just want frameworks)

### MCP integrations
- The operate skills depend on Slack, Gmail, Google Calendar, Notion, Linear MCPs
- These require user-specific API keys and auth
- Setup flow needs to handle: "Want to connect Slack? Here's how" without blocking users who don't have those tools

### Scope of onboarding
- How much should setup generate vs. leave as templates?
- Should setup ask about tool integrations (Slack, Linear) or defer that?
- How opinionated should the generated CLAUDE.md be?

### Brand and existing users
- bette-think has 2 forks, 5 stars — small but nonzero community
- Plugin rename from `pm-thought-partner` already happened today
- Need clean migration messaging for existing users

---

## What This Unlocks

### For users:
- 5-minute setup to a working PM operating system
- One install, not four
- Skills that get better as the system evolves (one repo to update)
- Building-in-public community around one product, not scattered repos

### For Bree:
- One repo to maintain
- Contribution flow is simple: PR to bette
- Brand consolidation: bette IS the product
- Platform for building in public — every improvement is visible in one place

### For the open source community:
- Clear entry point for PMs exploring AI-assisted workflows
- Modular enough to use pieces (just frameworks, just ops) without the whole system
- Templates and examples that teach context engineering by doing, not reading

---

## Inspiration

- **[superpowers](https://github.com/obra/superpowers)** — Zero-config discipline layer for coding agents. SessionStart hook bootstrap, skill routing, rationalization-proofing. The architectural pattern Bette should adopt.
- **Bette Davis** — Sued a studio because they wouldn't give her better roles. The namesake for a system that refuses to let PMs settle for undisciplined AI.

---

*One plugin. One install. Fasten your seatbelts.*
