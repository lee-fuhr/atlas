# Atlas

**An architectural framework for AI-augmented systems. One model, three layers, six feedback arrows, and a precedence hierarchy — all battle-tested across real projects.**

---

## What this is

A framework for understanding and organizing AI-augmented systems — specifically systems built around Claude Code (or similar AI coding assistants) with agents, hooks, skills, background automation, and accumulated knowledge.

The core model is **KCA** (Knowledge / Capability / Activity):

```
Knowledge   <- what the system KNOWS (consultable, pulled, never auto-fires)
  |- Principles  -> methodology, architectural decisions, normative rules
  '- Experience  -> memory systems, session history, decision journals

Capability  <- what the system is SET UP TO DO (standing apparatus, fires on trigger)
  |- Machinery   -> config files, agent definitions, background jobs, databases, scripts
  '- Protocols   -> hooks, skills, rules files, verification procedures

Activity    <- what the system is DOING RIGHT NOW (ephemeral, born from Capability)
  '- Execution   -> live sessions, running agents, active processes
```

Every component in your system belongs in exactly one sub-layer. The **3 placement tests** (consult-vs-fire, authority-record, stability-as-property) tell you which one.

The **6 arrows** describe how information flows between layers — from principles constraining capability, to experience feeding adaptation, to activity capturing knowledge. Together they form the feedback loops that keep the system alive and improving.

## What's inside

| File | What it covers |
|------|---------------|
| [`atlas-framework.md`](atlas-framework.md) | The universal framework: the KCA model, placement tests, precedence hierarchy, 6 arrows, cross-cutting concerns, evolution protocol |
| [`atlas-example.md`](atlas-example.md) | A real-world applied example: how one AI-augmented consulting practice organized their system using KCA |
| [`ADAPTING.md`](ADAPTING.md) | How to create your own Atlas for your system |

## Who this is for

Anyone building a non-trivial system with Claude Code (or similar) who needs:
- A shared model for "where does this component live?"
- Consistent placement decisions across sessions
- A precedence hierarchy for resolving conflicting directives
- Understanding of how knowledge, tools, and live processes interact

## How to use it

**Option 1: Read the framework.** Start with `atlas-framework.md`. Understand the KCA model, the 3 placement tests, and the 6 arrows. That's the core — everything else builds on it.

**Option 2: Study the example.** Read `atlas-example.md` to see how one real system maps onto the framework. This shows the kinds of decisions you'll make when applying KCA to your own setup.

**Option 3: Build your own.** Follow `ADAPTING.md` to create an Atlas for your system. Start with the framework sections, then fill in your own applied sections (3-7, 10) with your actual components.

## Related projects

- **[Build Bible](https://github.com/lee-fuhr/build-bible)** — Methodology for building with AI agents (the "how we build" companion to the Atlas)
- **[Memeta](https://github.com/lee-fuhr/memeta)** — FSRS-6 memory system for Claude Code (one of the Knowledge/Experience components referenced in the Atlas)

## License

MIT — see [LICENSE](LICENSE)

---

*A framework for making sense of complex AI-augmented systems.*
