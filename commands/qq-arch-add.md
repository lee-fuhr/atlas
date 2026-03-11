# Register new system component

Guides through placement (Atlas layer) and grooming note. Prevents components being added without architectural placement decisions documented.

**Arguments:** $ARGUMENTS

**Usage:**
- `/qq-arch-add [component name]` — start with known component
- `/qq-arch-add` — guided interview

---

## Steps

### Step 1: Collect component details

Use `AskUserQuestion` to gather:

1. **Name** — What is this component called?
2. **Type** — LaunchAgent / Python script / hook / skill / database / agent definition / rules file / CLAUDE.md / other
3. **What it does** — One sentence: what does it do when active?
4. **File path** — Where does it live?
5. **Trigger** — How does it activate? (scheduled / session-start / hook event / manual invocation / always-on / never auto-fires)

### Step 2: Run the 3 placement tests

Apply each test in order. Show reasoning.

**Test 1 — Consult-vs-fire (primary)**
> Does this component activate automatically or fire on a trigger?

- YES → **Capability** (Machinery or Protocols sub-layer)
- NO, it sits waiting to be read → **Knowledge** (Principles or Experience sub-layer)
- Special case: if it is currently doing something right now and is ephemeral → **Activity/Execution**

**Test 2 — Authority record**
> Is this the single source of truth for normative guidance (principles, decisions)?

- YES → **Knowledge/Principles**
> Is this the single source of truth for empirical facts (records, history)?
- YES → **Knowledge/Experience**

**Test 3 — Stability as property**
> Does this change rarely and govern many things? → Knowledge
> Does this change per-project or fire per-session? → Capability/Protocols or Machinery

**Sub-layer disambiguation (if Capability):**
- Is it infrastructure (databases, agents, LaunchAgents, Python scripts, CLAUDE.md)? → **Capability/Machinery**
- Is it behavioral governance (hooks, skills, rules files, sweeps, SOPs)? → **Capability/Protocols**

### Step 3: Confirm placement

State the result:
> "Based on the placement tests, [component] belongs in **[Layer/Sub-layer]** because [one-sentence rationale]."

If placement is ambiguous, name the ambiguity and ask for clarification before proceeding.

### Step 4: Add Atlas grooming note

🗺️ Adding grooming note to Atlas Section [N]...

Open `~/atlas/atlas-framework.md`. Navigate to the relevant section (the one that covers the component's layer). Add a brief note in the section's component list or examples, if the section has one.

### Step 5: Confirm

Report:
- Layer assigned: [KCA layer/sub-layer]
- Atlas: grooming note added at Section [N]
- Any follow-up needed (e.g., Bible principle to add, hook to write)

---

## Placement cheat sheet

| Component type | Default placement |
|---------------|------------------|
| LaunchAgent plist | Capability/Machinery |
| Python script (scheduled/called) | Capability/Machinery |
| Hook (PreToolUse, PostToolUse, etc.) | Capability/Protocols |
| Skill (SKILL.md) | Capability/Protocols |
| Rules file (auto-loaded) | Capability/Protocols |
| SOP / sweep | Capability/Protocols |
| CLAUDE.md | Capability/Machinery |
| Agent definition | Capability/Machinery |
| SQLite database | Capability/Machinery |
| Build Bible / principles | Knowledge/Principles |
| Architectural decision record | Knowledge/Principles |
| MEMORY.md / Memeta | Knowledge/Experience |
| Session index / decision journal | Knowledge/Experience |
| Live session / running process | Activity/Execution |
