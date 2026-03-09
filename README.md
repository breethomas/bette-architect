# bette-architect

> "I was just sure of myself. This is and always has been an unforgiving quality to the unsure." — Bette Davis

Context engineering for AI-assisted work. The foundation that makes everything else work.

**This repo is the architecture documentation for [Bette](https://github.com/breethomas/bette).** For the installable plugin, go there.

## Install Bette

```
/plugin marketplace add breethomas/bette
/plugin install bette@breethomas
```

## What's Here

Architecture docs explaining WHY Bette works the way it does:

- **[claude-md-architecture.md](claude-md-architecture.md)** — Layered context: global → project → directory
- **[reference-file-design.md](reference-file-design.md)** — One-line entries, deep context elsewhere
- **[memory-systems.md](memory-systems.md)** — What to persist, correction loops, the 200-line budget
- **[context-hygiene.md](context-hygiene.md)** — Read for editing, delegate for reference
- **[skill-architecture.md](skill-architecture.md)** — Forked vs interactive, output routing

Plus annotated examples in [examples/](examples/).

## Part of the Bette System

| Repo | Purpose |
|------|---------|
| **[bette](https://github.com/breethomas/bette)** | The plugin. Install this. |
| **[bette-architect](https://github.com/breethomas/bette-architect)** | How to architect — context engineering (you are here) |
| **[bette-think](https://github.com/breethomas/bette-think)** | How to think — PM frameworks |
| **[bette-work](https://github.com/breethomas/bette-work)** | How to work — quality gates and guardrails |
| **[bette-os](https://github.com/breethomas/bette-os)** | How to operate — daily PM workflows |

## License

CC BY-NC-SA 4.0

---

*The foundation. Everything else builds on this.*
