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

On first session after install, the SessionStart hook detects no existing Bette setup and offers two paths:

```
Bette: "I'm Bette — your PM operating system. How do you want to get started?

        1. 'Set me up' — I'll ask a few questions and build your
           environment. Takes about 5 minutes.

        2. 'I'll configure myself' — I'll show you what's available
           and get out of the way."
```

#### Path 1: "Set me up" (Designers, non-technical PMs, "just make it work")

Full guided onboarding. Bette asks questions, generates everything, offers to connect integrations, enables all relevant modules.

```
Bette: "What's your role?"
User:  "VP Product at a SaaS company"

Bette: "Do you manage people directly?"
User:  "Yes, 4 direct reports"

Bette: "What tools does your team use?"
User:  "Slack, Notion, Linear"

Bette: "Do you write code, or just manage the people who do?"
User:  "I code sometimes on side projects"

...

Bette: "Here's what I've set up:
        ✓ Global CLAUDE.md (your preferences and working style)
        ✓ Project directory with reference files
        ✓ People reference (4 direct reports — fill in details)
        ✓ Domains reference (template ready for your product areas)
        ✓ Backlog and goals files
        ✓ PM framework skills enabled (think)
        ✓ Daily workflow skills enabled (operate)
        ✓ Coding guardrails enabled (work)
        ✓ Slack, Notion, Linear — ready to connect when you want

        Say 'catch me up' to start your day, or ask me anything."
```

Takes the user from Ad Hoc to Planned in 5 minutes.

#### Path 2: "I'll configure myself" (Engineers, technical PMs, power users)

Installs the plugin, shows the module map, and gets out of the way. Think skills are active by default (zero config). Everything else is opt-in.

```
Bette: "You've got 60 skills across 4 modules. Here's the map:

        architect — Context engineering (setup, health-check, reference-gen)
        think     — PM frameworks, 30 skills (active now)
        work      — Quality gates, session management, test-first
        operate   — Daily ops: inbox, meetings, backlog, comms

        Enable modules:  'bette enable operate'
        See all skills:  '/skills'
        Read the docs:   github.com/breethomas/bette-architect

        Or just start talking. I'll route to the right skill."
```

The underlying architecture is identical. The difference is how much the setup does for you.

#### The Repos as Documentation

The four repos (bette-architect, bette-think, bette-work, bette-os) stay published. They're the educational layer — they explain WHY things work the way they do. The cookbook. The `bette` plugin is the restaurant. Users who want to understand the architecture read the repos. Users who want to work install the plugin.

### Ongoing Experience

After setup, every session:
1. SessionStart hook loads the meta-skill
2. Meta-skill knows all available skills and routes automatically
3. User just talks — natural language triggers the right skill
4. Context files provide persistent memory across sessions

---

## Architecture

### Technical Constraints (Plugin System Beta)

The Claude Code plugin system is in public beta. Key findings from research:

**What works:**
- Skills auto-discover from `skills/` directories (no registration needed)
- Progressive disclosure: only skill descriptions load into context, full content on invoke
- Personal skills (`~/.claude/skills/`) shadow plugin skills by name
- `context: fork` supported for heavy/isolated skills
- Skills can direct Claude to write files anywhere (CLAUDE.md, reference files, etc.)
- Plugins can be enabled/disabled with `/plugin enable|disable`

**What's broken (as of March 2026):**
- **Plugin SessionStart hook output is silently discarded** (issue #12151) — hooks run but Claude never sees the output
- **First-run timing bug** (issue #10997) — marketplace plugin hooks fail on first session
- Non-plugin hooks (in `~/.claude/settings.json`) work reliably

**Architectural decision: Hybrid bootstrap.**
The `/bette-setup` skill generates a SessionStart hook in `~/.claude/settings.json` (not in the plugin) during onboarding. This gives users the auto-bootstrap experience using the reliable non-plugin hook path. When Anthropic fixes plugin hook bugs, the hook moves into the plugin and setup stops generating it externally.

### Plugin Structure

```
bette/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace config
├── plugins/
│   └── bette/
│       ├── .claude-plugin/
│       │   └── plugin.json       # Plugin manifest
│       ├── skills/
│       │   ├── bette-setup/SKILL.md    # First-use onboarding (generates external hook)
│       │   ├── bette-health/SKILL.md   # Audit context architecture
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
| `(root)` | new | 2 (bette-setup, bette-health) | Onboarding + architecture audit |
| `think/` | bette-think | 30 | PM frameworks, strategic sparring |
| `work/` | bette-work | 3 | Quality gates, session management, test-first |
| `operate/` | bette-os | 22 | Daily workflows: inbox, meetings, backlog, comms |

**Total: ~57 skills, 7 agents**

Modules are directory organization for maintainability. Users never reference modules — they reference skill names. Claude Code discovers all skills regardless of nesting depth.

### Bootstrap Flow

**Before setup (first install):**
```
User installs plugin → starts session → no hook exists yet
  │
  └── User runs /bette-setup (or types "set me up")
        │
        ├── Interactive onboarding (Path 1 or Path 2)
        ├── Generates CLAUDE.md, reference files, project structure
        ├── Generates SessionStart hook in ~/.claude/settings.json
        └── "Restart your session. Bette will be there."
```

**After setup (every subsequent session):**
```
Session starts
  │
  ├── SessionStart hook fires (from ~/.claude/settings.json — reliable path)
  │     └── Injects Bette meta-context into session
  │
  └── User speaks → Claude routes to correct skill via plugin
        ├── "catch me up" → bette:catchup
        ├── "prep for my 1:1 with Carol" → bette:prep-1on1
        ├── "help me think through this feature" → bette:strategy-session
        ├── "check my context setup" → bette:bette-health
        └── "quality gates before I push" → bette:quality-gates
```

### Routing

Claude Code's native skill system handles routing. Skills declare trigger patterns in their YAML frontmatter `description` field. Claude matches natural language to skill descriptions automatically — no custom router needed.

The SessionStart hook's job is simpler than originally planned:
- Inject Bette identity and working preferences into session context
- Load the user's CLAUDE.md (which references their reference files)
- No need for a custom meta-skill router — Claude Code does this natively

This is leaner and more aligned with Anthropic's official architecture. The plugin ships skills with good descriptions. Claude Code discovers and routes to them. The hook provides session-level context.

### Skill Namespacing

All skills are invocable as `/bette:skill-name` (plugin namespace). Claude also matches natural language without the prefix. Users can override any skill by placing a same-named skill in `~/.claude/skills/` (personal skills take priority over plugin skills).

**Module organization is internal, not user-facing.** The `skills/` directory is organized by module (think/, work/, operate/) for maintainability, but users don't need to know which module a skill belongs to. They just talk or type `/bette:skill-name`.

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

### Plugin system capabilities — RESOLVED
- **SessionStart hook in plugin is broken** (issue #12151). Workaround: `/bette-setup` generates the hook in `~/.claude/settings.json` where it works reliably.
- **First-use detection:** Hook checks for a marker file (e.g., `~/.claude/.bette-configured`). No marker = suggest running `/bette-setup`.
- **File writing:** Skills direct Claude to use normal tools (Write, Edit). No sandbox on file creation. Setup can write CLAUDE.md, reference files, anywhere.
- **Personal skill overrides:** `~/.claude/skills/skill-name/SKILL.md` shadows `bette:skill-name` automatically. User can force plugin version with `bette:skill-name` prefix.
- **Routing:** Claude Code's native skill discovery handles routing via YAML frontmatter descriptions. No custom meta-skill router needed.

### Skill organization — PARTIALLY RESOLVED
- Modular architecture: skills organized by module (architect, think, work, operate)
- Routing is flat — user says what they need, meta-skill finds the right skill regardless of module
- Think ships active by default (zero config, no integrations needed)
- Operate is opt-in (requires MCP integrations)
- Work is opt-in (relevant for PMs who code)
- Architect is always available (setup + health-check)
- Open: should users be able to enable/disable individual skills, or only whole modules?

### MCP integrations
- The operate skills depend on Slack, Gmail, Google Calendar, Notion, Linear MCPs
- These require user-specific API keys and auth
- Setup flow needs to handle: "Want to connect Slack? Here's how" without blocking users who don't have those tools

### Scope of onboarding — RESOLVED
- Two paths: "set me up" (full generation) and "I'll configure myself" (modules + docs)
- "Set me up" asks about integrations, generates opinionated CLAUDE.md
- "I'll configure myself" activates think by default, everything else opt-in
- Both paths produce the same underlying architecture

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
