# Adapting the Atlas to your system

This guide walks you through creating your own Atlas — an architectural reference document for your AI-augmented system using the KCA framework.

---

## Start with the framework

The framework (`atlas-framework.md`) is universal. You can use it as-is:

- **Section 0** — How to use the document (adapt the intro to describe YOUR system)
- **Section 1** — The KCA model and placement tests (universal — use directly)
- **Section 2** — Precedence hierarchy (universal structure — fill in your specific levels)
- **Section 8** — The 6 arrows (universal — the feedback loops apply to any system)
- **Section 9** — Cross-cutting concerns (adapt maintenance cadences to your rhythm)
- **Section 11** — Evolution protocol (universal — use directly)

## Build your applied sections

The example (`atlas-example.md`) shows what these look like filled in. Create your own versions:

### Section 3: Knowledge / Principles

Document your normative knowledge sources:
- What methodology document(s) do you follow?
- What architectural decisions have you documented?
- What enforcement mechanisms connect principles to behavior?

### Section 4: Knowledge / Experience

Document your empirical knowledge stores:
- Memory system (if any)
- Session history
- Decision journals
- Meeting archives
- Any accumulated records consulted for context

### Section 5: Capability / Machinery

Inventory your standing infrastructure:
- Config files (CLAUDE.md tiers, settings)
- Agent definitions
- Background jobs (LaunchAgents, cron, etc.)
- Databases
- Scripts and utilities

### Section 6: Capability / Protocols

Inventory your behavioral governance:
- Hooks (what events trigger what behaviors?)
- Skills (what on-demand capabilities exist?)
- Rules files
- Verification procedures
- Any automated quality gates

### Section 7: Activity / Execution

Document your runtime patterns:
- What types of sessions do you run?
- What agent spawning patterns do you use?
- What are your common failure modes?
- How do processes interact?

### Section 10: Document index

Create a master index of every significant document in your system with its Atlas layer classification.

## Tips

1. **Start small.** You don't need all sections on day one. The model (section 1) and arrows (section 8) give you 80% of the value.

2. **Use the placement tests.** When you're unsure where something goes, apply the 3 tests in order: consult-vs-fire, authority-record, stability-as-property.

3. **Don't duplicate.** The Atlas references other documents — it doesn't restate their content. Point to your methodology doc, your component inventory, your memory system. One source of truth per topic.

4. **Keep it machine-readable.** The Atlas is optimized for AI consumption during sessions. Use precise paths, concrete examples, and deterministic rules rather than narrative.

5. **Evolve it.** Use the evolution protocol (section 11) to keep the Atlas accurate. A wrong Atlas is worse than no Atlas — it produces confidently incorrect placement decisions.
