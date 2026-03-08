# bette-architect

> "I was just sure of myself. This is and always has been an unforgiving quality to the unsure." — Bette Davis

Context engineering for AI-assisted work. The foundation that makes everything else work.

---

## The Problem

Everyone's talking about prompt engineering. But a great prompt in a bad context produces mediocre results. The difference between "I use AI sometimes" and "AI is my operating system" isn't better prompts — it's better context architecture.

Context engineering is how you structure persistent information so AI assistants are effective across sessions, not just in a single conversation.

## What's Inside

### CLAUDE.md Architecture
How to design layered context files that give AI the right information at the right time.
- **Global context** — who you are, how you work, what never changes
- **Project context** — what this project is, how it's structured, what matters
- **Directory context** — local rules for specific areas of a codebase or project
- When to use each layer, what goes where, and what to keep out

### Reference File Design
How to structure persistent knowledge files that AI can consume efficiently.
- One line per entry, deep context lives elsewhere
- People, domains, systems, terminology — patterns that scale
- The bitter lesson applied: don't over-structure, let AI do the pattern matching

### Memory Systems
What to persist across sessions and what to let die.
- Auto-memory patterns — what's worth remembering vs. session-specific noise
- Memory file organization — semantic topics, not chronological logs
- Correction loops — when AI gets something wrong, fix it at the source

### Context Hygiene
How to keep AI sessions productive as context grows.
- Read for editing, delegate for reference
- When to use sub-agents for heavy reads
- What bloats context and what doesn't
- Hook patterns for automatic guardrails

### Skill Architecture
How to design AI workflows that stay coherent.
- Forked vs. non-forked skills — when to isolate, when to keep interactive
- Sub-agent delegation patterns — what to send out, what to keep in main context
- Output routing — where results land so they survive context compression

### The Maturity Model

| Level | Context Practice |
|-------|-----------------|
| **Ad Hoc** | No persistent context. Every session starts from scratch. |
| **Planned** | CLAUDE.md exists. Basic project instructions. AI knows the basics. |
| **Systematic** | Layered context architecture. Reference files. Session management. Patterns captured and reused. |
| **Optimized** | Living system. AI manages its own memory. Context hygiene is automated. Skills route to the right depth of context automatically. |

## Who This Is For

- **Anyone using Claude Code** (or similar AI coding tools) who wants sessions that build on each other
- **PMs building AI workflows** who need their context architecture to scale
- **Founders and builders** who use AI as a daily operating system, not just a chatbot
- **Teams adopting AI tools** who need a shared context standard

## Part of the Bette System

Bette is a suite of open source tools for PMs who build with AI, named after Bette Davis — who sued a studio because they wouldn't give her better roles.

| Repo | Purpose |
|------|---------|
| **[bette-architect](https://github.com/breethomas/bette-architect)** | How to architect — context engineering (you are here) |
| **[bette-think](https://github.com/breethomas/bette-think)** | How to think — PM frameworks and strategic sparring |
| **[bette-work](https://github.com/breethomas/bette-work)** | How to work — quality gates, session management, maturity model |
| **[bette-os](https://github.com/breethomas/bette-os)** | How to operate — daily PM operating system |

## Getting Started

Start with the layer that matches where you are:

1. **No context files yet?** Start with [CLAUDE.md Architecture](#claudemd-architecture) — one file transforms your AI experience
2. **Have basic context?** Move to [Reference File Design](#reference-file-design) — structure your knowledge so AI can use it
3. **Context is getting unwieldy?** Read [Context Hygiene](#context-hygiene) — learn what to delegate and what to keep
4. **Ready to scale?** Design your [Skill Architecture](#skill-architecture) — build workflows that stay coherent

## Philosophy

> "To fulfill a dream, to be allowed to sweat over lonely labor, to be given a chance to create, is the meat and potatoes of life. The money is the gravy." — Bette Davis

Context engineering isn't glamorous. It's the infrastructure that makes AI actually useful day after day. No one sees your CLAUDE.md files. No one admires your reference file naming conventions. But without them, every AI session is a cold start.

Do the unglamorous work. Build the foundation. Everything else gets better.

## Contributing

This repo will grow as patterns are validated across real workflows. PRs welcome — especially patterns from your own context engineering practice.

## License

CC BY-NC-SA 4.0 — Free to use for personal and internal business use with attribution.

---

*The foundation. Everything else builds on this.*
