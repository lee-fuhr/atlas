# Atlas section router

Translates natural-language questions into the right Atlas section to load. Never loads the full Atlas — always routes to the minimal section that answers the question.

**Arguments:** $ARGUMENTS

**Usage:**
- `/qq-arch-load [question]` — route from argument
- `/qq-arch-load` — guided: ask for the question, then route

---

## Routing table

| If the question is about... | Load section |
|-----------------------------|-------------|
| Placing a new component, "where does X live?", KCA model | **1** — The model |
| Conflicting rules, which instruction wins, precedence | **2** — Precedence |
| Build Bible, principles, architectural decisions, ADRs | **3** — Knowledge / Principles |
| Memeta, MEMORY.md, session index, empirical records | **4** — Knowledge / Experience |
| CLAUDE.md, agents, LaunchAgents, databases, Python scripts | **5** — Capability / Machinery |
| Hooks, skills, sweeps, rules files, SOPs | **6** — Capability / Protocols |
| Live sessions, background tasks, failure modes, debugging | **7** — Activity / Execution |
| How something propagates, feedback loops, inter-layer flows | **8** — The arrows |
| System health, boot sequence, drift, health monitoring | **9** — Cross-cutting |
| Finding a specific document | **10** — Document index |
| Changing the Atlas structure itself | **11** — Evolution |
| Understanding the Atlas itself | **0** — How to use |

---

## Steps

### Step 1: Identify the question

If invoked without an argument, ask:
> "What do you need to find in the Atlas?"

### Step 2: Route

Match the question to the routing table above. If genuinely ambiguous between two sections, name both and ask which the user needs.

### Step 3: Load the section

🗺️ Reading Atlas section [N] — [title]...

Read the Atlas from `~/atlas/atlas-framework.md`. Extract only the target section (from its `## Section N` heading to the next `## Section` heading). Present the section content.

### Step 4: Confirm or drill

After presenting the section, ask: "Does this answer it, or do you need a different section?"

---

## Notes

- Never load more than 2 sections in a single `/qq-arch-load` call
- If the question requires multiple sections, name them all and load the most relevant first
- Check `~/.claude/rules/atlas.md` first if it exists — it's a fast-path index before opening the full Atlas
