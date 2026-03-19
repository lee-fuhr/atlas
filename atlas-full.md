<!-- Atlas v1.0.0 | Last updated: 2026-03-05 | Written for Claude Code sessions -->
<!-- Load by section. Never load the full document. Use /qq-arch-load for section routing. -->

# Atlas — system architecture

*Version 1.0.0 · 2026-03-04*

---

## Table of contents

- [Section 0: How to use this document](#section-0-how-to-use-this-document)
  - [Section index](#section-index)
  - [Relationship to other governing documents](#relationship-to-other-governing-documents)
  - [System at a glance](#system-at-a-glance)
- [Section 1: The model](#section-1-the-model)
  - [Three layers defined](#three-layers-defined)
  - [Five sub-layers defined](#five-sub-layers-defined)
  - [The 3 placement tests](#the-3-placement-tests)
  - [Quick-placement table](#quick-placement-table)
  - [Theoretical backing](#theoretical-backing)
  - [The dual-lens principle](#the-dual-lens-principle)
  - [What this model does not tell you](#what-this-model-does-not-tell-you)
- [Section 2: Precedence & conflict resolution](#section-2-precedence--conflict-resolution)
  - [The 7 levels](#the-7-levels)
  - [Conflict examples](#conflict-examples)
  - [Decision tree for genuine conflicts](#decision-tree-for-genuine-conflicts)
- [Section 3: Knowledge / Principles](#section-3-knowledge--principles)
  - [The Build Bible](#the-build-bible)
  - [Architectural decisions](#architectural-decisions)
  - [Enforcement philosophy](#enforcement-philosophy)
  - [Universal learnings](#universal-learnings)
  - [Bible evolution protocol](#bible-evolution-protocol)
  - [Maintenance](#maintenance)
- [Section 4: Knowledge / Experience](#section-4-knowledge--experience)
  - [Memeta — spaced repetition memory system](#memeta--spaced-repetition-memory-system)
  - [MEMORY.md — session memory](#memorymd--session-memory)
  - [Session index — searchable session history](#session-index--searchable-session-history)
  - [Decision journal](#decision-journal)
  - [Learnings queues — per-project observation capture](#learnings-queues--per-project-observation-capture)
  - [Mistake documentation](#mistake-documentation)
  - [Bible changelog](#bible-changelog)
  - [Debt inventory](#debt-inventory)
  - [Directory (system inventory)](#directory-system-inventory)
  - [Internal flows](#internal-flows)
- [Section 5: Capability / Machinery](#section-5-capability--machinery)
  - [CLAUDE.md — the 4-tier instruction system](#claudemd--the-4-tier-instruction-system)
  - [Agent definitions — 47 agents in 7 teams + specialists](#agent-definitions--47-agents-in-7-teams--specialists)
  - [Databases — 6 SQLite schemas](#databases--6-sqlite-schemas--2-referenced-in-backup)
  - [LaunchAgent plists — 47 processes](#launchagent-plists--47-scheduledbackground-processes)
  - [Python scripts — 317 scripts](#python-scripts--317-scripts-organized-by-function)
  - [Routing tables, templates, configuration](#routing-tables-templates-configuration)
- [Section 6: Capability / Protocols](#section-6-capability--protocols)
  - [SOPs — standing operating procedures](#sops--standing-operating-procedures)
  - [Sweeps — targeted systematic audits](#sweeps--targeted-systematic-audits)
  - [Capabilities — skills and plugins](#capabilities--skills-and-plugins)
  - [Event automation — hooks](#event-automation--hooks)
- [Section 7: Activity / Execution](#section-7-activity--execution)
  - [Component types](#component-types)
  - [Lifecycle](#lifecycle)
  - [Failure modes summary](#failure-modes-summary)
- [Section 8: The arrows — inter-layer flows](#section-8-the-arrows--inter-layer-flows)
  - [Arrow 1: Constraint](#arrow-1-constraint--knowledgeprinciples-to-capability)
  - [Arrow 2: Configuration](#arrow-2-configuration--capability-to-activityexecution)
  - [Arrow 3: Capture](#arrow-3-capture--activityexecution-to-knowledgeexperience)
  - [Arrow 4: Adaptation](#arrow-4-adaptation--knowledgeexperience-to-capabilityprotocols)
  - [Arrow 5: Codification](#arrow-5-codification--capabilityprotocols-to-knowledgeprinciples)
  - [Arrow 6: Escalation](#arrow-6-escalation--activityexecution-to-knowledgeprinciples-bypass)
  - [Arrow interaction analysis](#arrow-interaction-analysis)
- [Section 9: Cross-cutting concerns](#section-9-cross-cutting-concerns)
  - [Consistency maintenance](#consistency-maintenance)
  - [Drift detection](#drift-detection-1)
  - [System boot sequence](#system-boot-sequence)
  - [Health monitoring](#health-monitoring)
  - [Pace-layer table](#pace-layer-table)
  - [Synchronization cadence](#synchronization-cadence)
  - [Known architectural gaps](#known-architectural-gaps)
- [Section 10: Document index](#section-10-document-index)
- [Section 11: Evolution protocol](#section-11-evolution-protocol)
  - [Structural changes — full evolution protocol](#structural-changes----full-evolution-protocol)
  - [Grooming pass — lightweight maintenance](#grooming-pass----lightweight-maintenance)
  - [Anti-rot mechanisms](#anti-rot-mechanisms)

---

## Section 0: How to use this document

### What this is

The Atlas is the architectural reference for Lee Fuhr's AI-augmented consulting system. It answers one question: **how does it all fit together?** The model, every relationship, every flow, every precedence rule. It is an exhaustive index of the system, written for Claude Code sessions to load section-by-section on demand.

Primary audience: Claude. This document is optimized for machine consumption during active sessions. It uses precise file paths, concrete examples, and deterministic rules rather than narrative explanations.

### What this isn't

- **Not the Build Bible** — that answers HOW WE BUILD (principles, patterns, playbook). See `~/CC/Work/_ Infrastructure/Build Bible.md`
- **Not the Directory** — that answers WHAT EXISTS (component catalog with rationale). See `~/CC/Work/LFI/_ System/directory.md`
- **Not a design doc** — it doesn't argue for the architecture. It documents the architecture as it is.

### How to load

Load by section number using `/qq-arch-load [description]` or load the specific section file directly. The section index below tells you which section answers which question. **Never load the full document** — it is approximately 2,500 lines and you only need 50-300 lines at a time.

### Section index

| Section | Title | Load when... |
|---------|-------|-------------|
| 0 | How to use this document | You need to understand this document itself |
| 1 | The model | Placing a new component; answering "where does X live?"; understanding the architecture |
| 2 | Precedence & conflict resolution | Resolving a conflict between rules, protocols, or instructions |
| 3 | Knowledge / Principles | Working with the Bible; adding architectural decisions; understanding normative knowledge |
| 4 | Knowledge / Experience | Working with Memeta, MEMORY.md, session index, or empirical records |
| 5 | Capability / Machinery | Working with CLAUDE.md, agent definitions, LaunchAgents, databases, Python scripts |
| 6 | Capability / Protocols | Working with hooks, skills, sweeps, rules files, or behavioral governance |
| 7 | Activity / Execution | Debugging live sessions; understanding failure modes; working with background tasks |
| 8 | The arrows — inter-layer flows | Tracing how something propagates through the system; understanding feedback loops |
| 9 | Cross-cutting concerns | System health, consistency maintenance, session boot sequence, health monitoring |
| 10 | Document index | Finding a specific document in the system |
| 11 | Evolution protocol | Making structural changes to the Atlas itself |

### Relationship to other governing documents

```
Atlas (this)          → HOW IT FITS TOGETHER (architecture, flows, precedence)
Build Bible           → HOW WE BUILD (principles, patterns, playbook)
System Inventory      → WHAT EXISTS (component catalog, file paths, rationale)
```

The three documents form a triangle. The Atlas references the Bible by section number and the Directory by component name — it never restates their content. When you need principle details, load the Bible. When you need component details, load the Directory. When you need to understand relationships, flows, or placement, load the Atlas.

### Theoretical backing

The model is grounded in five compatible frameworks: Pace Layering (Brand) — slow layers govern fast ones; Viable System Model (Beer) — every viable system requires operations, management, and an audit channel; Panarchy (Holling) — systems cycle through growth, conservation, and creative destruction; Ashby's ultrastability — a system maintains viability by responding to perturbations through multiple feedback loops; near-decomposability (Simon) — complex systems are hierarchical, with high interaction within sub-systems and loose coupling between them. The 2-2-1 model (Knowledge / Capability / Activity) implements these principles as a concrete architecture. The 6 arrows implement the feedback loops that prevent the system from fossilizing.

### System at a glance

The full system in three tables. For component counts and file paths, load Section 1 or 5.

**The 4 governing documents:**

| Document | File | Answers |
|----------|------|---------|
| Build Bible | `Work/_ Infrastructure/Build Bible.md` | HOW WE BUILD — methodology, principles, patterns, playbook |
| Atlas (this) | `Work/_ Infrastructure/atlas.md` | HOW IT ALL FITS — model, flows, precedence, document index |
| Directory | `Work/LFI/_ System/directory.md` | WHAT EXISTS — component catalog with rationale |
| Memeta | `Work/_ Infrastructure/memory-system-v1/` | WHAT WE REMEMBER — FSRS spaced repetition, session memory |

**The 6 arrows (inter-layer flows):**

| Arrow | From | To | Timescale |
|-------|------|----|-----------|
| Constraint | Knowledge / Principles | Capability | Per-change (same session as Bible update) |
| Configuration | Capability | Activity | Per-session |
| Capture | Activity | Knowledge / Experience | Minutes-hours |
| Adaptation | Knowledge / Experience | Capability / Protocols | Weekly (behavior-audit + learnings-review) |
| Codification | Capability / Protocols | Knowledge / Principles | Days-weeks (human judgment required) |
| Escalation | Activity | Knowledge / Principles | On demand (bypass, human-only) |

**Placement cheat sheet:**

| If it... | Layer | Sub-layer |
|----------|-------|-----------|
| Is consulted as reference, never fires automatically | Knowledge | Principles or Experience |
| Is normative / prescriptive (principles, decisions) | Knowledge | Principles |
| Is empirical / descriptive (records, history, catalog) | Knowledge | Experience |
| Fires or loads on a trigger / schedule | Capability | Machinery or Protocols |
| Is standing infrastructure (agents, scripts, DBs, CLAUDE.md) | Capability | Machinery |
| Is behavioral governance (hooks, skills, rules, sweeps) | Capability | Protocols |
| Is running right now, will be gone when done | Activity | Execution |

---

## Section 1: The model

### Why a model?

Without a shared architectural model, every question about the system — "where should this live?", "what takes precedence?", "how does X affect Y?" — requires first-principles reasoning from scratch. First-principles reasoning from scratch produces inconsistent answers. Session A places a new component in one location; session B places a similar component somewhere else; session C doesn't know which was right. Inconsistency compounds. Within months the system becomes a maze of ad-hoc decisions that no single session can reason about.

A model gives Claude a shared map. When the map is accurate, navigation is fast: "this is a behavioral governance mechanism, it belongs in Capability/Protocols" takes one lookup instead of ten minutes of deliberation. When the map is wrong, the Atlas updates and the map improves — but between updates, every session navigates the same way. Consistency across sessions is the primary value.

The core insight is that the three layers aren't arbitrary divisions — they correspond to fundamentally different *kinds* of things. Knowledge is what the system knows (consultable reference). Capability is what the system can do (standing apparatus that loads and fires). Activity is what the system is doing right now (ephemeral processes). These three kinds have different stability characteristics, different maintenance rhythms, and different authority levels. Conflating them produces confusion about where to put things and why. A rules file that "feels like knowledge" but actually loads and fires every session belongs in Capability, not Knowledge — and misplacing it means misunderstanding its maintenance needs, its authority level, and its failure modes.

The model also prevents a common failure mode: treating the system as flat. Without layers, everything is "just files" and the only organization principle is folder structure. With layers, you can ask: "does this new component change what the system *knows*, what the system *can do*, or what the system *is doing right now*?" That question routes placement, determines maintenance cadence, and predicts failure modes — all from a single classification.

### Three layers defined

#### Knowledge — what the system knows

**Definition:** Knowledge is the body of reference material the system consults but does not automatically execute. It is the system's memory and judgment, consulted when needed, never fired on a trigger.

**Kind:** Consultable reference. A session loads Knowledge when it needs guidance, context, or empirical data. Knowledge does not load itself.

**What distinguishes it:** Knowledge is *pulled*, never *pushed*. No Knowledge component activates automatically on a schedule, on a hook event, or at session start. If something activates automatically, it belongs in Capability even if its content reads like knowledge. Knowledge has the highest authority (Principles sub-layer) and the longest time horizon.

**Examples:** Build Bible principles, architectural decisions, MEMORY.md, Memeta records, session history in sessions.db (when queried for reference), System Inventory, past meeting transcripts (when consulted).

#### Capability — what the system is set up to do

**Definition:** Capability is the standing apparatus and behavioral governance that exists between sessions — ready to activate, configured to fire, waiting for a trigger. It is the system's infrastructure and its rules of engagement.

**Kind:** Standing apparatus + governance. Capability persists across sessions and activates when conditions are met (session start, hook event, user invocation, schedule).

**What distinguishes it:** Capability *fires*. Every component in Capability either loads automatically (CLAUDE.md, hooks, LaunchAgents) or fires when invoked (skills, scripts, slash commands). The test is: does it DO something when activated, or does it just SIT there waiting to be read? If it does something, it's Capability.

**Examples:** CLAUDE.md files (load every session), agent definitions (spawn when invoked), hooks (fire on events), skills (fire when loaded), LaunchAgent plists (fire on schedule), Python scripts (fire when called), SQLite databases (active I/O machinery), rules files (load and govern behavior).

#### Activity — what the system is doing right now

**Definition:** Activity is the set of live, ephemeral processes currently executing. It is the system in motion — born from Capability's configuration, dead when the process ends.

**Kind:** Ephemeral live processes. Activity has no persistence of its own — when a session ends, that Activity ceases to exist. Its artifacts (files written, memories captured, databases updated) persist in Knowledge or Capability, but the Activity itself is gone.

**What distinguishes it:** Activity is *ephemeral*. It has a birth (session start, agent spawn, script invocation) and a death (session end, task completion, script exit). Nothing in Activity survives without being captured into another layer. If something persists between sessions, it is not Activity.

**Examples:** This Claude Code session, a running Task-spawned agent, an active LaunchAgent process, a hook script currently executing, a Python script in flight.

### Five sub-layers defined

#### Knowledge / Principles (normative — what should be)

**Definition:** The system's normative beliefs — deliberate, codified judgments about how things should work. Principles are prescriptive: they tell the system what to do and what not to do.

**Key characteristics:**
- Highest authority below human real-time command
- Slowest rate of change (weeks between updates; never without cross-session evidence)
- Requires human judgment to modify (the Codification arrow — see section 8)
- Small in volume, large in influence

**What belongs here:** Build Bible principles (14), Build Bible patterns (19), Build Bible anti-patterns (8), architectural decisions (documented in the Bible or in project decisions.md files), the Atlas model itself.

**What doesn't belong here:** CLAUDE.md directives (those are Machinery — they fire), rules files (those are Protocols — they fire), anything that loads automatically.

#### Knowledge / Experience (empirical — what has been)

**Definition:** The system's accumulated empirical record — what happened, what was observed, what was learned. Experience is descriptive: it records reality rather than prescribing behavior.

**Key characteristics:**
- Medium authority (informs but doesn't command)
- Grows continuously through the Capture arrow (Activity → Experience)
- Eventually consistent — there is always some lag between what happened and what's recorded
- Large in volume, valuable in aggregate

**What belongs here:** MEMORY.md (auto-memory), Memeta/FSRS records, session index (sessions.db queried as reference), meeting transcript archive (transcripts.db queried as reference), System Inventory (directory.md — canonical empirical record of what exists), contacts database (contacts.db queried as reference), ea_brain.db intelligence records, past session artifacts consulted for context.

**What doesn't belong here:** Running queries against these databases (that's Activity). The databases themselves as I/O machinery (that's Capability/Machinery). The distinction: the *data* is Experience; the *database engine and schema* is Machinery.

#### Capability / Machinery (standing apparatus — files, agents, scripts, configs that load and fire)

**Definition:** The standing technical infrastructure that loads, fires, and executes. Machinery is the system's skeleton and muscles — it doesn't decide what to do (that's Protocols), but it provides the apparatus to do it.

**Key characteristics:**
- Fires automatically or on invocation — never just sits there
- Persists between sessions
- Medium rate of change (days-weeks)
- Configuration-heavy — many Machinery components are configured by Protocols

**What belongs here:** CLAUDE.md files (4-tier — load every session and configure behavior), agent definitions (47 — spawn when invoked via Task tool), LaunchAgent plists (60 — fire on schedule), Python scripts (317 — fire when invoked), SQLite databases (6 — active I/O infrastructure), slash commands (fire when invoked), MCP server configurations, plugin installations.

**What doesn't belong here:** The Bible (consulted, not fired — Knowledge/Principles). MEMORY.md (empirical record, not apparatus — Knowledge/Experience). Hooks (behavioral governance — Capability/Protocols).

#### Capability / Protocols (behavioral governance — rules, hooks, skills, sweeps that govern behavior)

**Definition:** The behavioral rules and automated governance mechanisms that constrain and direct how Machinery and Activity operate. Protocols are the system's nervous system — they sense conditions and trigger responses.

**Key characteristics:**
- Fires automatically on triggers (hooks) or when loaded (rules files, skills)
- Governs behavior rather than providing infrastructure
- Medium-fast rate of change (days-weeks, faster than Machinery)
- The bridge between Knowledge (which says what SHOULD happen) and Activity (which is happening NOW)

**What belongs here:** Rules files (`~/.claude/rules/` — load and govern behavior), hooks (28 across 6 event types — fire on events), skills (100+ — invoked and fire), sweeps (14 types — invoked and fire), verification protocols, questioning protocol, steelman protocol, security audit protocol, hookify plugin (creates and manages hooks).

**What doesn't belong here:** CLAUDE.md (that's Machinery — it's infrastructure that loads, not governance that constrains). The Bible (that's Knowledge — it's consulted, Protocols enforce it). Agent definitions (that's Machinery — agents are apparatus, not governance).

**The distinguishing question between Machinery and Protocols:** Does this component *provide infrastructure* (Machinery) or *govern behavior* (Protocols)? A database is infrastructure. A hook that checks file size before writes is governance. CLAUDE.md is infrastructure (it loads context). A rules file is governance (it constrains behavior). When ambiguous, ask: "If I removed this, would the system lose a *capability* (Machinery) or lose a *constraint* (Protocols)?"

#### Activity / Execution (live processes — sessions, running agents, tool calls, scripts in flight)

**Definition:** Everything currently alive and running. Execution is the system's heartbeat — it exists only in the present tense.

**Key characteristics:**
- Fully ephemeral — born from Configuration, dies when process ends
- Fastest rate of change (seconds-hours)
- Lowest authority (Activity follows orders from Capability and Knowledge)
- The only layer where actual work happens — Knowledge and Capability are inert without Activity

**What belongs here:** Active Claude Code sessions, running Task-spawned agents, active LaunchAgent processes, hook scripts currently executing, Python scripts in flight, tool calls in progress, MCP server connections currently open.

**What doesn't belong here:** Anything that persists after the process ends. A file written by an agent is not Activity — it's an artifact that lives in whichever layer it belongs to. The agent *while running* is Activity. The agent definition *on disk* is Capability/Machinery.

### The 3 placement tests

When you need to determine where a component belongs, apply these tests in order. Most components are resolved by test 1 alone.

#### Test 1 — Consult-vs-fire

**Question:** "Would Claude consult this like a reference, or does it load and fire automatically?"

**How to apply:** Imagine a new Claude Code session starts. Does this component get pulled in and read when Claude needs it (consult → Knowledge)? Or does it load/activate/fire without Claude choosing to consult it (fire → Capability)? Or is it a currently-running process (Activity)?

**Correct placement example:** Rules files in `~/.claude/rules/` fire automatically — they load into every session and constrain behavior without Claude choosing to read them. → **Capability/Protocols.** The Build Bible is consulted — Claude loads it when it needs principle guidance, not automatically. → **Knowledge/Principles.**

**Counter-example where intuition misleads:** Agent definitions (47 `.md` files in `_ System/agents/`) feel like knowledge — you'd read them to understand the system's agent roster. But they *fire*: when a session spawns an agent via the Task tool, the agent definition loads and configures that agent's behavior. The definition is standing apparatus that activates on invocation. → **Capability/Machinery**, not Knowledge.

#### Test 2 — Authority-record

**Question:** "Is this the canonical record, or a derived copy?"

**How to apply:** Trace the information to its source. If this component IS the authoritative source — the place where this information is created and maintained — it lives where its nature dictates. If it's a summary, copy, or derivative of another source, the derivative lives downstream of the canonical source.

**Correct placement example:** Build Bible rule 1.1 (Orchestrate, don't execute) → canonical, lives in Knowledge/Principles. The CLAUDE.md summary of that same rule ("Orchestrate, don't execute — delegate to specialist agents") → derived copy that lives in Capability/Machinery. The CLAUDE.md entry isn't a principle; it's a piece of machinery that loads the principle's summary into each session.

**Counter-example where intuition misleads:** The System Inventory (`directory.md`) feels like it should live in Capability — it documents Capability components, after all. But the System Inventory IS the canonical empirical record of what exists in the system. It's not machinery that fires; it's a reference document that gets consulted. → **Knowledge/Experience**, because it's the authoritative empirical record, not a piece of standing apparatus.

#### Test 3 — Stability-as-property

**Question:** "Is this stable because it's foundational, or stable because it's battle-tested?"

**How to apply:** Some components are very stable. The question is *why* they're stable. If stable because they express foundational beliefs (principles, architectural decisions), they belong in Knowledge/Principles. If stable because they've been refined through use and their current form works well (mature scripts, proven agent definitions), they belong in Capability. Stability is a *property*, not a *location indicator*.

**Correct placement example:** The 47 agent definitions are very stable — they rarely change. But they're stable because they've been tested and refined across hundreds of sessions, not because they express a foundational belief about how things should work. → **Capability/Machinery.** A new Bible rule added via `/qq-bible-add` is foundational from day 1, even before it's proven. → **Knowledge/Principles.**

**Counter-example where intuition misleads:** The 6 SQLite database schemas are extremely stable and feel "foundational." But they're infrastructure — active I/O machinery that fires on every read/write. Their stability comes from maturity, not from expressing normative beliefs. → **Capability/Machinery**, not Knowledge/Principles. If you promoted a schema to Principles, you'd be saying "this schema is a belief about how things should work" rather than "this schema is a piece of infrastructure that works well." The Bible principle that governs databases is 1.5 (single source of truth) — *that's* the principle. The schema is the machinery that implements it.

### Quick-placement table

| Component | Layer | Sub-layer | Deciding test |
|-----------|-------|-----------|---------------|
| Build Bible (`Build Bible.md`) | Knowledge | Principles | Consult-vs-fire: consulted, not fired |
| Bible principles (14) | Knowledge | Principles | Authority-record: canonical normative source |
| Bible patterns (19) | Knowledge | Principles | Authority-record: canonical normative source |
| Bible anti-patterns (8) | Knowledge | Principles | Authority-record: canonical normative source |
| Atlas (this document) | Knowledge | Principles | Authority-record: canonical architectural model |
| Architectural decisions | Knowledge | Principles | Stability-as-property: foundational from day 1 |
| MEMORY.md | Knowledge | Experience | Authority-record: canonical empirical record of past sessions |
| Memeta FSRS records | Knowledge | Experience | Consult-vs-fire: consulted (reviewed for learning), not fired |
| Session index data (sessions.db content) | Knowledge | Experience | Authority-record: canonical empirical record of session history |
| Transcript archive (transcripts.db content) | Knowledge | Experience | Authority-record: canonical empirical record of meetings |
| System Inventory (`directory.md`) | Knowledge | Experience | Authority-record: canonical empirical record of what exists |
| Contacts data (contacts.db content) | Knowledge | Experience | Authority-record: canonical empirical record of contacts |
| EA Brain intelligence (ea_brain.db content) | Knowledge | Experience | Authority-record: canonical empirical record of relationships |
| CLAUDE.md (4-tier) | Capability | Machinery | Consult-vs-fire: loads and fires every session |
| Agent definitions (47) | Capability | Machinery | Consult-vs-fire: fires when agents spawn |
| LaunchAgent plists (63) | Capability | Machinery | Consult-vs-fire: fires on schedule |
| Python scripts (318) | Capability | Machinery | Consult-vs-fire: fires when invoked |
| SQLite databases (6 — as infrastructure) | Capability | Machinery | Consult-vs-fire: active I/O machinery |
| Slash commands (`~/.claude/commands/`) | Capability | Machinery | Consult-vs-fire: fires when invoked |
| MCP server configurations | Capability | Machinery | Consult-vs-fire: fires on connection |
| Plugin installations (6) | Capability | Machinery | Consult-vs-fire: fires when activated |
| Rules files (`~/.claude/rules/`) | Capability | Protocols | Consult-vs-fire: loads and governs behavior |
| Hooks (37 across 7 event types) | Capability | Protocols | Consult-vs-fire: fires automatically on events |
| Skills (100+) | Capability | Protocols | Consult-vs-fire: invoked and fires |
| Sweeps (14 types via `/church-*`) | Capability | Protocols | Consult-vs-fire: invoked and fires |
| Verification protocols | Capability | Protocols | Governs behavior: constrains completion claims |
| Questioning protocol | Capability | Protocols | Governs behavior: constrains action without clarification |
| Steelman protocol | Capability | Protocols | Governs behavior: constrains plan approval |
| Security audit protocol | Capability | Protocols | Governs behavior: constrains component installation |
| Hookify plugin | Capability | Protocols | Consult-vs-fire: fires to create/manage hooks |
| Claude Code sessions | Activity | Execution | Ephemeral: born from Configuration arrow |
| Running agents (Task-spawned) | Activity | Execution | Ephemeral: live processes |
| Active LaunchAgent processes | Activity | Execution | Ephemeral: running now |
| Hook scripts currently executing | Activity | Execution | Ephemeral: in-flight |
| Tool calls in progress | Activity | Execution | Ephemeral: in-flight |

### Theoretical backing

**Pace Layering (Brand).** Stewart Brand's "How Buildings Learn" identified that complex systems have layers that change at different rates — the faster layers innovate and the slower layers constrain and stabilize. In this system: Activity changes with every session (seconds-hours), Capability changes as tools and protocols mature (days-weeks), Knowledge/Experience grows continuously but slowly (days-months), and Knowledge/Principles changes only when well-tested insights warrant codification (weeks-months — faster than traditional orgs because AI reduces validation cost, but still slower than Protocols). The slow layers govern the fast ones through the Constraint and Configuration arrows. This is not a metaphor — it is the literal mechanism. The Build Bible (slow, Principles) constrains what hooks and skills (faster, Protocols) are allowed to do. CLAUDE.md (medium, Machinery) configures each session (fast, Execution). Violating pace layering — letting a fast layer modify a slow one without human judgment — is the definition of the "false codification" failure mode.

**Viable System Model (Beer).** Stafford Beer's VSM describes what every self-maintaining system needs. System 1 (operations) maps to Activity/Execution — where work actually happens. System 2 (coordination) maps to the Configuration arrow — how Capability configures each execution instance. System 3 (management) maps to Capability — machinery and protocols that govern and configure operations. System 4 (intelligence) maps to Knowledge/Experience — the empirical learning layer that scans the environment and feeds adaptation. System 5 (policy) maps to Knowledge/Principles — the normative layer that defines identity, values, and constraints. The Atlas gives Beer's abstract model concrete file paths: System 5 lives at `~/CC/Work/_ Infrastructure/Build Bible.md`. System 1 lives in whatever `claude` process is currently running.

**Panarchy (Holling).** Holling's adaptive cycle theory describes how complex systems cycle through growth (r), conservation (K), collapse (Omega), and reorganization (alpha). This system's feedback arrows map onto the panarchy cycle. Capture (Activity → Experience) is rapid accumulation (r phase) — every session deposits empirical data. Adaptation (Experience → Protocols) is consolidation (K phase) — accumulated experience tightens and tunes behavioral governance. Codification (Protocols → Principles) is creative destruction and reorganization (Omega/alpha) — a mature protocol earns promotion to a principle, which may force restructuring of other principles and protocols. Understanding panarchy explains why Codification is rare and requires human judgment: reorganizing Principles is a high-stakes Omega event, not a routine update. The `/qq-bible-add` command lives at the Codification arrow precisely because this transition demands deliberate human evaluation.

### The dual-lens principle

Functional classification (Knowledge / Capability / Activity) is the primary lens — it answers WHERE things live and WHY. Temporal classification (how fast things change) is the secondary lens — it answers WHEN things need maintenance and how urgently changes propagate.

Do not confuse them. Something can change slowly and still live in Capability (stable agent definitions that haven't been modified in months are still Machinery — they fire). Something can change quickly and still live in Knowledge (MEMORY.md updates frequently but is still Experience — it's consulted, not fired). Rate of change is a maintenance signal, not a location indicator.

When the two lenses disagree with your intuition, trust the functional lens. A component's *kind* (consultable vs. firing vs. ephemeral) determines its home. Its *rate of change* determines its maintenance schedule.

### What this model does not tell you

- **Change cadence and maintenance schedules** — use the pace-layer table in section 9
- **Component lifecycle** (born, mature, deprecated, archived) — that's the System Inventory's job (see `~/CC/Work/LFI/_ System/directory.md`)
- **What should exist** — only where things that DO exist should live. Gaps are tracked in the Bible's debt inventory (section 9)
- **Cost or performance** — those are orthogonal concerns tracked in the Bible (pattern 2.1, section 5.3)
- **Intra-layer conflicts** — those are handled by section 2 (precedence & conflict resolution)
- **Inter-layer flows** — those are handled by section 8 (the arrows)

---

## Section 2: Precedence & conflict resolution

### Why precedence matters

When two directives conflict, the system needs a deterministic resolution mechanism. Without one, Claude falls back to best-guess reasoning — which is inconsistent across sessions and produces unpredictable behavior. Session A resolves a conflict one way; session B resolves the same conflict differently; neither documents the reasoning; the inconsistency compounds. The precedence hierarchy provides a lookup table: given two conflicting directives, the resolution is mechanical, not interpretive.

The governing rule is: **broader scope wins over narrower scope; explicit wins over implicit.** A human real-time command is explicit and has maximum scope (this session, this moment, overrides everything). A system default is implicit and has minimum scope (only when nothing else applies). The seven levels below implement this rule as a concrete hierarchy.

### The 7 levels

**1. Human real-time command** — Lee speaking now takes absolute precedence. The system serves Lee, not the other way around. Any instruction from Lee in a live session overrides every level below. This is not a theoretical principle — it is the operational reality that the entire system exists to serve Lee's judgment. When Lee says "ignore that rule and do X," Claude does X.

**2. Principles (Bible)** — The Build Bible (`~/CC/Work/_ Infrastructure/Build Bible.md`) contains the system's normative knowledge: 14 critical rules, 19 reusable patterns, 8 anti-patterns. These represent well-tested, deliberately codified insights that have survived adversarial review and real-world application across dozens of projects. High stability, high authority. Principles override everything except Lee's direct command.

**3. Architectural decisions** — Scoped applications of principles to specific contexts. "We use SQLite for all databases in this system" is an architectural decision. "All session-index analysis uses Sonnet minimum for quality" is an architectural decision. These are documented in the Bible, in project `decisions.md` files, or in the Atlas itself. They override CLAUDE.md and protocols because architectural decisions represent deliberate, documented judgment calls — they are principles applied to a specific domain.

**4. CLAUDE.md directives** — The 4-tier CLAUDE.md system (Global → Rules → Project → Operations) contains standing behavioral instructions that load every session. More specific than principles (they tell Claude what to do in this project, not universally), less universal than architectural decisions. Within the CLAUDE.md hierarchy: operations-level overrides project-level; project-level overrides global-level. More specific scope wins.

**5. Protocols / Rules** — Rules files (`~/.claude/rules/`), verification protocols, questioning protocol, steelman protocol, security audit protocol. These are behavioral governance — they tell Claude WHAT to do in specific situations. More specific than CLAUDE.md (they govern particular behaviors, not overall session configuration), less authoritative because they are enforcement mechanisms for higher-level directives rather than directives themselves.

**6. Hooks / Skills** — Automated behaviors (35 hooks across 7 event types) and on-demand capabilities (100+ skills). Most specific, least authoritative among configured behaviors. A hook that fires on PostToolUse is a narrow, specific behavior. A skill loaded for a single task is narrower still. These implement protocols and CLAUDE.md directives — they don't override them.

**7. System defaults** — What happens when nothing else specifies behavior. Claude's training defaults, fallback behaviors, format conventions. The implicit baseline. Overridden by everything above.

### The precedence rule

> When two directives conflict: the one with broader scope wins. When scope is equal: the explicit directive wins over the implicit one. When both are explicit and equal in scope: the higher level in the hierarchy (lower number) wins.

### Conflict examples

**Example 1 — Hook contradicts CLAUDE.md:**
- Hook (PostToolUse): "Always add a timestamp comment to every file you edit"
- CLAUDE.md directive: "Only add comments where logic isn't self-evident"
- **Resolution:** CLAUDE.md (level 4) > Hook (level 6). Follow CLAUDE.md — add comments only where non-obvious. If the hook is producing wrong behavior, update it via the Adaptation arrow (see section 8).

**Example 2 — Skill behavior contradicts rules file:**
- Skill loaded: canvas-design skill outputs PNG files to `~/Desktop`
- Rules file (`~/.claude/rules/`): all output files go to `_ Inbox/` unless destination is clear
- **Resolution:** Rules/Protocols (level 5) > Skill behavior (level 6). Route output to `_ Inbox/`. The skill's default output path is a suggestion; the rules file is governance.

**Example 3 — Bible principle vs Lee's real-time command:**
- Bible rule 1.1: "Orchestrate, don't execute — delegate to specialist agents"
- Lee says: "Just write this function directly, don't spawn an agent"
- **Resolution:** Human command (level 1) > Principles (level 2). Write the function directly. This is correct behavior — the Bible serves Lee, not the reverse. Lee's override here does not change the principle; it exercises the human's absolute right to override any level below.

**Example 4 — Two rules files contradict each other:**
- `~/.claude/rules/build-bible.md` (global): "No files > 500 lines"
- A project-level `.claude/rules/` file: "This generated file is expected to be 1,200 lines — do not split it"
- **Resolution:** Both are level 5 (Protocols). Apply scope rule: more specific scope wins. The project-level rule overrides the global rule for that specific file. The global 500-line rule still applies to all other files in the project.

**Example 5 — CLAUDE.md directive vs architectural decision:**
- CLAUDE.md (global): "Use Haiku for simple tasks to minimize cost"
- Architectural decision (documented): "All session-index analysis uses Sonnet minimum for quality"
- **Resolution:** Architectural decision (level 3) > CLAUDE.md directive (level 4). Use Sonnet for session-index analysis. The cost optimization directive yields to the quality-specific architectural decision.

### Decision tree for genuine conflicts

When two directives conflict, walk this tree:

1. **Is one a live instruction from Lee?** → Follow Lee. Always. No exceptions.
2. **Is one a Bible principle and the other a derived directive** (CLAUDE.md, rules file, hook)? → Follow the Bible principle. Flag the derived directive as needing update — it should be brought into alignment with the principle it violates.
3. **Are both the same level** (e.g., two rules files, two CLAUDE.md tiers)? → More specific scope wins. Project-level overrides global. Domain-specific overrides general-purpose.
4. **Are both the same level and same scope?** → More explicit wins over implicit. A directive that says "do X in situation Y" beats a default behavior pattern.
5. **Still ambiguous after steps 1-4?** → **Stop. Ask Lee.** Document the conflict in `decisions.md` (if task notes exist) or in the session summary. Flag it for the next Atlas/Bible review.

🔖 **LEE:** When a conflict reaches step 5 — when the decision tree genuinely cannot resolve it — that signals a gap in the system. The resolution Lee provides should be documented as either a new architectural decision (level 3) or a CLAUDE.md update (level 4) so the same conflict never reaches step 5 again. Track these in the Atlas evolution protocol (section 11).

### Edge cases

**CLAUDE.md contains principle summaries.** The CLAUDE.md files include condensed versions of Bible principles (e.g., the `Build Bible.md` rules file summarizes all 14 principles). When the summary conflicts with the full Bible text, the Bible text wins — it is the canonical source (Authority-record test). The CLAUDE.md summary is a derived copy at level 4; the Bible principle is the authority at level 2.

**Hooks enforce principles.** Some hooks (like `delegation-check.py`) directly enforce Bible principles (1.1 — Orchestrate, don't execute). The hook is level 6 but it enforces a level 2 directive. If the hook's implementation diverges from the principle it enforces, the principle wins and the hook should be updated. The hook does not gain the principle's authority — it is still a level 6 mechanism.

**Protocols reference each other.** The steelman protocol references the questioning protocol. The verification protocol references both. When protocols reference each other, they operate as a chain, not a hierarchy — the full chain applies. A conflict between two protocols at the same level falls to step 3 of the decision tree: more specific scope wins.

---

## Section 3: Knowledge / Principles

Normative knowledge prescribes what SHOULD happen. It is the system's ought — the distilled, adversarially reviewed, evidence-backed rules that govern how every piece of downstream machinery behaves. Normative knowledge does not describe the world as it is; it declares how this system operates. When a CLAUDE.md directive tells an agent "never skip the red phase of TDD," that instruction traces back to a Principle. When a hook blocks solo execution after three strikes, the blocking rule traces back to a Principle. Principles are the authority layer — everything downstream inherits from them.

Why does normative knowledge need its own sub-layer, separate from empirical knowledge? Because authority demands stability. Empirical knowledge — Memeta reviews, MEMORY.md entries, session indices, mistake documentation — changes constantly. A new session generates new experience every few hours. But the Build Bible's 14 critical rules change on the order of months, through a deliberate 7-step codification process with steelman review and human sign-off. If Principles and Experience shared a layer, the most stable things in the system (rules that have survived adversarial review across dozens of projects) would coexist with the most volatile (a mistake documented ten minutes ago). The result is authority confusion: an agent consulting the layer cannot distinguish "this is a proven rule" from "this is a recent observation." Separate sub-layers make the distinction structural, not interpretive.

Everything downstream is shaped by what lives here. The Constraint arrow flows from Knowledge/Principles to all of Capability — machinery and protocols alike. Every CLAUDE.md directive, every rules file, every hook behavior, every agent definition, every skill's enforcement logic is downstream of Principles. When a Principle changes, the blast radius is system-wide. This is not a defect; it is the design. Principles are the slow layer that stabilizes the fast ones. A system where the fast layers can override the slow ones drifts. A system where the slow layers constrain the fast ones converges. The infrequency of Principle changes is the feature, not the limitation.

### The Build Bible

**Location:** `~/CC/Work/_ Infrastructure/Build Bible.md`
**Scale:** 1,922 lines, sections 0-10 plus changelog
**Version:** 1.5.0 (as of 2026-03-04)
**Authority:** The PRIMARY document of the Principles sub-layer. Everything else in Principles either feeds into it (via Codification) or is indexed by it.

The Bible is structured as principles first (section 1), patterns second (section 2), architecture third (section 3), playbook fourth (section 4), agent system fifth (section 5), anti-patterns sixth (section 6), operations seventh (section 7), evolution eighth (section 8), debt ninth (section 9), and credits tenth (section 10). The ordering is deliberate: principles constrain patterns, patterns implement principles, and everything else is downstream.

#### Critical rules (14)

These are the non-negotiable constraints on all work. Lower-numbered principles override higher when they conflict.

| # | Rule | Core constraint |
|---|------|-----------------|
| 1.1 | Orchestrate, don't execute | The conductor coordinates and quality-controls — never writes code, drafts copy, or does research directly |
| 1.2 | QA the design before writing code | Run adversarial review on the design before any implementation begins |
| 1.3 | Test first, then build (TDD) | RED then GREEN then REFACTOR — the red phase proves the test has teeth |
| 1.4 | Simplicity wins | When a feature isn't earning its complexity, delete it — don't optimize or refactor |
| 1.5 | Single source of truth | Every data domain has ONE canonical store — no sync jobs, no drift |
| 1.6 | Config drives behavior, code stays generic | Scale by adding data files, not code — new campaign = JSON entry, not source modification |
| 1.7 | Checkpoint gates with explicit failure plans | Every multi-step process has measurable checkpoints with predetermined failure response |
| 1.8 | Prevent, don't recover | Validate before attempting — layered pre-validation so bad data never reaches the expensive operation |
| 1.9 | Atomic operations for crash safety | Write to temp file, then atomic rename — operations complete fully or don't happen at all |
| 1.10 | Document decisions when they're fresh | Capture the WHY during the work, not after — rationale is clearest at the moment of decision |
| 1.11 | Measure for self-correction, not vanity | Every metric must trigger a specific action at a specific threshold — if it doesn't change behavior, delete it |
| 1.12 | Observe everything, alert on what matters | Every service gets structured logging, health checks, and tiered alerting (CRITICAL/IMPORTANT/ADVISORY) |
| 1.13 | Test the unhappy path first | Error paths, edge cases, and failure modes get tested before happy paths |
| 1.14 | Speed hides debt | Fast shipping without verification creates invisible technical debt — velocity is not progress |

The rules interact in named chains documented in Bible section 1: Planning chain (1.2 -> 1.7 -> 1.3), Simplicity chain (1.4 -> 1.6 -> 1.5), Reliability chain (1.8 -> 1.9 -> 1.11), Knowledge chain (1.10 -> 1.5 -> 1.11). The override rule is simple: lower numbers win conflicts.

#### Reusable patterns (19)

Proven solutions extracted from production systems. Each pattern links to the principle(s) it implements.

| # | Pattern | Implements |
|---|---------|------------|
| 2.1 | Hierarchical cost optimization (80/15/5) | 1.1 (conductor) |
| 2.2 | Config-driven scaling | 1.6 (config-driven) |
| 2.3 | Progressive disclosure for definitions | 1.5, 1.6 (single source, config-driven) |
| 2.4 | Importance scoring with decay and reinforcement | 1.11 (actionable metrics) |
| 2.5 | Guard chains (layered boolean validation) | 1.8, 1.4 (prevention, simplicity) |
| 2.6 | Fuzzy deduplication | 1.5 (single source) |
| 2.7 | Atomic writes for crash safety | 1.9 (atomic ops) |
| 2.8 | Three-tier enforcement (critical / important / advisory) | 1.12 (observability) |
| 2.9 | Async polling architecture | 1.7 (checkpoints) |
| 2.10 | AI + heuristic fallback | 1.8 (prevention) |
| 2.11 | Approval queue (async human-in-loop) | 1.1, 1.7 (conductor, checkpoints) |
| 2.12 | Randomization for pattern avoidance | 1.8 (prevention) |
| 2.13 | Ritualization (silent prep + notification + engagement) | 1.12 (observability) |
| 2.14 | Two-level learning with promotion | 1.10, 1.11 (live docs, actionable metrics) |
| 2.15 | Inverse scoring system (0-100 commodity scale) | 1.11 (actionable metrics) |
| 2.16 | Adversarial agent teams (structured debate) | 1.2 (QA-first) |
| 2.17 | Backup verification (test your safety nets) | 1.13 (unhappy path first) |
| 2.18 | Rate limiting and throttling | 1.8 (prevention) |
| 2.19 | Competitive generation (parallel solutions + objective ranking) | 1.2, 1.4 (QA-first, simplicity) |

The relationship between rules and patterns is structural: rules constrain, patterns implement. Pattern 2.5 (guard chains) is the implementation of rule 1.8 (prevention). Pattern 2.8 (three-tier enforcement) is the implementation of rule 1.12 (observability). When a new rule is added, it should have at least one pattern showing how to implement it. A rule without a pattern is an aspiration; a pattern without a rule is an orphan.

#### Anti-patterns (8)

Named failure modes to hunt and destroy. Each traces to the principle it violates. For full details: Bible section 6.

| # | Anti-pattern | Violates | Core signal |
|---|-------------|----------|-------------|
| 6.1 | The 49-day research agent | 1.2, 1.7 (QA-first, checkpoints) | Automated agent running for weeks with no human checkpoint |
| 6.2 | The premature learning engine | 1.4 (simplicity) | Sophisticated ML/scoring infrastructure for dozens of events per week |
| 6.3 | Solo execution | 1.1 (conductor) | Conductor writes code instead of delegating to specialists |
| 6.4 | The retrospective test | 1.3 (TDD) | All tests written after implementation, not before |
| 6.5 | Multiple sources of truth | 1.5 (single source) | Two or more stores tracking the same data domain with sync jobs |
| 6.6 | Validate-then-pray | 1.8 (prevention) | Expensive operation attempted first, failures handled after the fact |
| 6.7 | The god file | 1.4 (simplicity) | Any file past 500 lines, accumulating responsibilities |
| 6.8 | The silent service | 1.12 (observability) | Deployed service with no health monitoring, no alerting, no structured logs |

The rules-patterns-anti-patterns triad is the backbone of Principles. Rules declare the constraint. Patterns show the proven way to satisfy it. Anti-patterns name the specific failure mode when the constraint is violated. A session consulting Principles should be able to trace from any anti-pattern back to the rule it violates, and from that rule forward to the pattern that prevents the violation.

### Architectural decisions

Where rules say "do X in general," architectural decisions say "in this system, X means Y specifically." Architectural decisions are scoped applications of principles — they bind abstract rules to concrete system choices.

- **Where they live:** Documented in this Atlas and referenced in relevant CLAUDE.md files
- **Authority:** Level 3 in the precedence hierarchy (above CLAUDE.md directives, below Principles)
- **Format:** Each decision captures the question, the decision, the rationale, and the date

Key architectural decisions for this system include:

| Decision | Rationale | Traces to |
|----------|-----------|-----------|
| All databases use SQLite | Single-user system; SQLite eliminates server overhead while providing full SQL capability | 1.4 (simplicity) |
| Agent definitions follow 3-tier hierarchy (Director/Senior/Junior) | Maps to cost model; complexity determines tier assignment | 1.1 (conductor), pattern 2.1 |
| Cost model: 80% Haiku / 15% Sonnet / 5% Opus | Task complexity distribution matches model capability tiers | 1.1 (conductor), pattern 2.1 |
| 4-tier CLAUDE.md hierarchy (Global -> Rules -> Project -> Operations) | Progressive specificity; higher tiers override lower; prevents god-file CLAUDE.md | 1.5 (single source), 1.4 (simplicity) |
| Cross-model routing (Gemini, Codex, Perplexity alongside Claude) | Different models have different strengths; route to capability, not brand loyalty | 1.1 (conductor) |
| Hook system uses PreToolUse matchers for active enforcement | Passive rules rely on agent compliance; hooks execute on every matching tool call | 1.12 (observability) |

### Enforcement philosophy

From Bible section 3.6: the enforcement traceability matrix. The governing principle is direct: unenforced principles are aspirations, not architecture. The gap between stated principle and actual behavior is drift, and drift compounds silently.

The system enforces principles through five complementary mechanisms:

| Mechanism | Components | Enforcement level |
|-----------|-----------|-------------------|
| Auto-loaded rules | `~/.claude/rules/*.md` (5 files) | Passive — relies on agent compliance |
| PreToolUse hooks | delegation-check, bloat-watcher, questioning-nudge, skill-usage-tracker, component-audit | Active — executes on every matching tool call |
| On-demand crusades | `/church` orchestrator with 14 crusade types | Manual — user-triggered deep scans |
| On-demand skills | 36+ skills across two locations | Manual — loaded by conductor or user |
| Slash commands | ~65 qq-* and other commands | Manual — user-triggered workflows |

The enforcement traceability matrix (Bible section 3.6) maps each of the 14 rules to its enforcement mechanism(s) and rates the strength. Three enforcement gaps are documented in the debt inventory (Bible section 9): principles 1.6 (config-driven), 1.11 (actionable metrics), and the cluster of 1.5/1.9/1.10 that rely entirely on manual review with no automated enforcement.

### Universal learnings

Cross-project insights promoted from individual project learnings-queue files. These represent battle-tested observations that have been confirmed across multiple clients and contexts.

- **Location:** `~/CC/Work/LFI/_ System/universal-learnings.md`
- **Placement:** Sits in the Experience sub-layer as a source file; promoted entries become candidates for Principles via Codification
- **Examples:** VBF framework, human headlines pattern
- **Promotion path:** Session insight -> learnings queue -> universal learnings -> Bible (via `/qq-bible-add`)
- **Threshold:** An observation needs confirmation across 3+ projects before it qualifies for Bible promotion (Bible section 8.3)

Universal learnings occupy a liminal position between Experience and Principles. The file itself is empirical (it records observations), but its promoted contents flow into normative rules. It is the staging area for Codification — the last waypoint before an observation becomes a rule.

### Bible evolution protocol

The process by which the Bible itself updates. Lives in Bible section 8. The Bible is a living document that improves through a disciplined 7-step process, not casual edits.

**The 7-step update process** (Bible section 8.1):
1. Observation — pattern noticed across 3+ sessions or projects
2. Proposal — written as specific rule with enforcement mechanism and evidence
3. Steelman — critiqued adversarially: is this actually needed?
4. Trial — run for 2 weeks as advisory (no blocking enforcement)
5. Measurement — check if the rule improved outcomes
6. Promotion — if proven, move to appropriate tier with appropriate enforcement level
7. Documentation — update the Bible with rule, evidence, and rationale

**Entry gate:** `/qq-bible-add [source]` — ingests external sources, classifies against existing Bible, proposes additions with steelman review. This is the Codification arrow made executable.

**Deletion criteria** (Bible section 8.3): not referenced in 6 months, contradicted by evidence, superseded by better pattern, or failed trial period.

**Version control:** Semantic versioning (MAJOR.MINOR.PATCH). Structure changes bump MAJOR; new principles/patterns bump MINOR; evidence updates bump PATCH. Changelog at `~/CC/Work/LFI/_ System/Build bible changelog.md`.

**Review cadence:** Weekly learning review, monthly full Bible review, on-demand after project completions. The review cadence is separate from the update cadence — updates happen at AI/learning speed (potentially daily via `/qq-bible-add`), but structural review happens monthly.

### Internal structure — how Principles components relate

```
Rules (14) ──constrain──> Patterns (19) ──implement──> Anti-patterns (8) are the violations
    │                         │                              patterns prevent
    │                         │
    ▼                         ▼
Architectural decisions    Anti-patterns name
are scoped applications    the specific failure
of rules to THIS system    when constraints break
    │
    ▼
Universal learnings are
candidates for eventual
rule/pattern promotion
    │
    ▼
Evolution protocol is
the governance mechanism
for everything above
```

Rules constrain. Patterns implement. Anti-patterns name the violations that patterns prevent. Architectural decisions are scoped applications of rules to this specific system. Universal learnings are candidates for eventual rule/pattern promotion. The evolution protocol governs how all of the above changes over time. No component exists in isolation.

### Maintenance

- **Who updates:** Lee (final authority) + Claude via `/qq-bible-add` (proposes, executes after approval)
- **What gates changes:** Steelman protocol for all changes; adversarial review for structural changes; human sign-off for all promotions
- **Update cadence:** Not on a fixed schedule. Updates happen at AI/learning speed — potentially daily via `/qq-bible-add`. The review cadence (weekly learnings, monthly structural) is separate from the update cadence.
- **Cascade constraint:** Bible updates trigger downstream cascade. After any Bible change, trace the enforcement chain and update derived copies (CLAUDE.md, rules files, verification protocols). The enforcement traceability matrix (Bible section 3.6) maps the blast radius.

🔖 **LEE:** Changes here cascade everywhere. Every Bible rule change potentially requires updates to CLAUDE.md, rules files, and verification protocols. Use Bible section 3.6 (enforcement traceability matrix) to understand the blast radius before modifying anything in Principles. The `/qq-bible-add` skill handles cascades automatically when invoked correctly.

---

## Section 4: Knowledge / Experience

Empirical knowledge describes what HAS happened. Sessions completed, mistakes made, insights captured, patterns observed, decisions recorded, debt inventoried. It does not prescribe behavior; it records reality. When Memeta surfaces a flashcard about a client preference, that is Experience. When a session index entry records "we decided to use SQLite on 2026-01-15," that is Experience. When a mistake document captures "agent X hallucinated a file path because we didn't validate it," that is Experience. None of these observations are rules — they are the raw material from which rules are eventually forged.

Why Experience and Principles need separate homes. The capacity they serve is fundamentally different. Principles is normative: stable, high-authority, changes slowly through deliberate codification. Experience is empirical: volatile, low-authority-by-itself, changes constantly as new sessions generate new observations. Mixing them produces two dangerous problems. First, noise: the constant churn of empirical updates drowns out the signal of deliberate principle changes. A Principle added after months of adversarial review sits next to a MEMORY.md note from yesterday's session — the authority gap is invisible. Second, confusion: "is this a rule or an observation?" becomes unanswerable without checking the provenance of every item. Separate sub-layers make the distinction structural: if it lives in Principles, it prescribes. If it lives in Experience, it describes.

The capacitor property. Experience is the accumulation buffer between fast ephemeral Activity and slow normative Principles. Like a capacitor, it stores charge and releases it. The Capture arrow (Activity -> Experience) accumulates observations over time — low authority, automatic, eventually consistent, on the order of minutes-to-hours. The Adaptation arrow (Experience -> Protocols) and the Codification arrow (Experience -> Principles) discharge that accumulated charge upward when patterns emerge. But the charge does not flow continuously. It accumulates, then discharges through deliberate, authority-gated channels. Codification requires 3+ project confirmation, steelman review, and human sign-off. This asymmetry — easy to write into Experience, hard to promote out of it — is the fundamental quality gate of the knowledge system. Without it, every one-off observation becomes a system-wide rule.

### Memeta — spaced repetition memory system

The most systematic component of Experience. Memeta manages codified knowledge as flashcard decks, surfacing cards at intervals calibrated to retention.

- **Location:** `~/CC/Work/_ Infrastructure/memory-system-v1/`
- **Scale:** 102 features, 2,037 tests, Python 3.11+
- **Algorithm:** FSRS (Free Spaced Repetition Scheduler) — calculates optimal review intervals based on Lee's actual retention curve
- **What it stores:** Flashcard decks covering principles, patterns, client facts, system knowledge, domain expertise
- **Interface:** `memory-ts` CLI, `memeta` commands
- **Heartbeat role:** Memeta reviews are the most systematic way experience is retained and surfaced. The FSRS cycle is the pulse of the Experience layer — cards not reviewed drift to unreviewed, which is functionally equivalent to knowledge lost. Each individual review is small; the compounding effect across hundreds of cards is the difference between a system that remembers and one that forgets.

### MEMORY.md — session memory

Auto-loaded, per-project memory that persists across conversations within the same project scope.

- **Location pattern:** `~/.claude/projects/[project-id]/memory/MEMORY.md`
- **What it stores:** Patterns confirmed across sessions, user preferences, project context, active state (suspended GitHub accounts, known bugs, in-flight decisions)
- **Update mechanism:** Claude edits this file during sessions when directed or when capturing important context that would otherwise be lost at session end
- **Scope:** Per-project. Global learnings are promoted to universal learnings (see below), not duplicated into every project's MEMORY.md
- **Authority level:** Low — MEMORY.md records observations, not rules. An entry like "Lee prefers adversarial debates for brainstorming" is an observed preference, not a system principle. It becomes a principle only if it survives the Codification path.

### Session index — searchable session history

A structured log of past Claude Code sessions with metadata, making past work searchable.

- **Scale:** 2,900+ sessions indexed
- **Metadata per session:** Name, date, tags, summary, git state, duration, tools used
- **Query interface:** `sessions` CLI via Bash — `sessions "query"` for search, `sessions context <id> "term"` for reading actual conversation, `sessions analytics` for patterns
- **What it enables:** Context continuity across sessions ("when did we discuss X?"), decision archaeology ("what was the decision on Y?"), pattern detection ("how often do we use agent Z?")
- **Why it matters:** Without a session index, every new session starts from zero context. The session index transforms 2,900 past sessions from inaccessible history into queryable institutional memory.

### Decision journal

Records of key decisions — what was decided, what alternatives were considered, and why.

- **Format:** `context.md` + `decisions.md` + `open-questions.md` per complex task, in `_notes/[task-name]/`
- **Template source:** `~/CC/Work/LFI/_templates/task-notes/`
- **When created:** Initialized at task start for multi-session tasks, complex decisions, high-stakes work, or anything requiring an audit trail
- **Why it matters:** Decisions are expensive to re-derive. "Why did we decide X?" should be answerable by reading a file, not by re-running the analysis that produced the decision. The decision journal is the canonical record of rationale — consulted whenever a past decision is questioned or revisited.

### Learnings queues — per-project observation capture

The frontline capture mechanism for observations worth recording but not yet processed.

- **Location pattern:** `[project]/learnings-queue.md`
- **What they store:** Observations, patterns noticed, potential improvements, session insights — anything worth capturing but not yet reviewed or promoted
- **Entry threshold:** Low. The bias is toward capturing too much rather than too little. Promotion gates filter; the queue should not.
- **Promotion path:**

```
Session insight ----> learnings-queue.md  (immediate, low-threshold)
                          |
                          v
                     MEMORY.md            (reviewed, medium-threshold)
                          |
                          v
                  Universal learnings     (3+ project confirmation)
                          |
                          v
                     Build Bible          (Codification arrow, high-threshold, human judgment)
```

- **Review cadence:** Weekly adaptation review via `/review-learnings` command. Friday 5pm LaunchAgent (`learnings_review.py`) surfaces pending items.

### Mistake documentation

The system for capturing failures so they do not repeat.

- **Protocol:** `~/CC/Work/LFI/_ System/mistake-documentation-protocol.md`
- **Mechanism:** `memory-ts add --type mistake --content "[What] | [Why] | [How]" --intent high --tags [project],[domain],[type],[severity]`
- **What is captured:** What happened, why it happened, how to prevent it — structured as three fields to enforce completeness
- **Downstream effect:** Mistakes captured here feed the Adaptation arrow (Protocols adjust to prevent recurrence) and, for recurring patterns, the Codification arrow (repeated mistakes become named anti-patterns in the Bible)
- **Critical property:** Mistakes that are not captured repeat. The documentation overhead per mistake is small; the cost of repeating it is large. The `--intent high` flag ensures mistakes are surfaced prominently, not buried in low-priority queues.

### Bible changelog

The version history of the Principles sub-layer itself.

- **Location:** `~/CC/Work/LFI/_ System/Build bible changelog.md`
- **What it records:** Every Bible change — version, date, what changed, why, what source prompted it, how to evaluate whether the change improved outcomes
- **Why it matters:** Understanding WHY a principle exists often requires reading its changelog entry. The current state of the Bible tells you what the rules are; the changelog tells you how they got that way. When someone proposes changing a rule, the changelog reveals the original context and evidence — preventing the system from re-litigating settled decisions.

### Debt inventory

Known technical debt and architectural gaps, making the invisible visible.

- **Source:** Bible section 9
- **Scale:** 15 items — 2 critical, 3 high, 7 medium, 3 low
- **What it records:** Severity, issue description, impact assessment, mitigation status
- **Critical items (as of 2026-03-04):** Meeting transcript fragmentation (4 sources, no unified search) and CLAUDE.md hierarchy broken (8 duplications, 6 contradictions)
- **Purpose:** Technical debt that is not inventoried accumulates silently. The debt inventory makes it visible, prioritized, and reviewable. Items are resolved, re-prioritized, or added during each review cycle (Bible section 8.2). Resolved items move to a "Resolved" section — they are not deleted, because the resolution history is itself valuable experience.

### Directory (system inventory)

The canonical record of what exists in the system.

- **Location:** `~/CC/Work/LFI/_ System/directory.md`
- **Scale:** 41KB, covers all system components with a "why it exists" column tracing each to a Bible principle
- **Placement rationale:** The Directory is empirical — it IS the canonical record of what exists. This placement may surprise those who expect it to live in Capability. But the authority-record test is clear: the Directory is the canonical source of truth about the system's composition. You consult the Directory to understand what exists; you fire Capability components to do work. Consulting versus firing is the Knowledge/Capability boundary.
- **Update mechanism:** `/qq-arch-add` for architecture documentation; Directory updated in parallel with any system change that adds, removes, or modifies components

### Internal flows

**The FSRS consolidation cycle.** Memeta manages review intervals. Cards surface when due. Lee reviews. Retention data updates scheduling. The next review interval is calculated from this session's performance. This is the most systematic part of Experience — everything else in this sub-layer is more ad-hoc. The FSRS cycle runs daily; other Experience components update opportunistically.

**The memory promotion pipeline.** Observations flow upward through progressively higher authority gates:

```
Session insight ----Capture arrow----> MEMORY.md
    (immediate, low-threshold, automatic)

MEMORY.md pattern ----Review----> Universal learnings
    (weekly, medium-threshold, requires pattern across sessions)

Universal learning ----Codification arrow----> Build Bible
    (months, high-threshold, 3+ project confirmation,
     steelman review, human judgment required)
```

Each stage has higher entry requirements than the last. A session insight needs only to be worth recording. A MEMORY.md pattern needs confirmation across multiple sessions. A universal learning needs cross-project validation. A Bible entry needs adversarial review and human sign-off. This progressive filtering is the system's quality gate — it prevents premature codification while ensuring genuinely proven patterns eventually reach Principles.

**Session index as experience database.** Sessions leave structured records with metadata. The `sessions` CLI queries them. Patterns across sessions become visible in aggregate — which agents are used most, which clients generate the most sessions, which topics recur. This aggregate view feeds both Adaptation (Protocol adjustments) and Codification (Principle candidates).

### Relationship to Principles

Experience feeds Principles via the Codification arrow — but this is a deliberate, human-judgment-gated channel, not automatic sync. Experience does NOT automatically update Principles. The flow is: observe pattern in Experience -> confirm via multiple instances across projects -> propose codification via `/qq-bible-add` -> steelman review -> Lee approves -> Bible updates -> Constraint arrow propagates downstream to Capability.

This gate exists because premature codification is a serious failure mode. An observation after one session is not a principle. A pattern confirmed across dozens of sessions, in multiple contexts, under adversarial review — that is a candidate for codification. The Bible's contribution rules (section 8.3) require: used successfully in 3+ projects, confidence score >0.8, survived steelman review, has a specific enforcement mechanism, and human review approved.

The asymmetry is intentional. Writing to Experience is cheap, fast, and automatic (Capture arrow). Promoting from Experience to Principles is expensive, slow, and deliberate (Codification arrow). The system biases toward capturing too much experience (low threshold) and promoting too little of it (high threshold). This is correct. Lost experience can often be re-observed; a false principle actively damages the system by constraining downstream machinery based on insufficient evidence.

### Relationship to Protocols

Experience also feeds Protocols via the Adaptation arrow. This channel is faster than Codification (weeks, not months) and lower-authority (Protocol adjustments, not Principle creation). When Experience reveals that a workflow consistently produces poor results, Protocols adapt — a new verification step is added, a hook is tightened, an agent prompt is refined. This does not require a Bible change; it requires a machinery or protocol adjustment informed by empirical evidence.

The key distinction: Codification creates new rules (normative). Adaptation adjusts existing workflows (procedural). Both consume Experience. They differ in authority, latency, and scope.

🔖 **LEE:** The FSRS cycle is the heartbeat of this layer. Memeta reviews are the most systematic way experience accumulates and surfaces. Skipping reviews is like skipping heartbeats — each one is small, but the compounding effect is memory decay and knowledge loss. The `/qq-bible-add` process is the deliberate, high-quality path from observation to codified principle. The promotion pipeline's progressive thresholds exist specifically to prevent premature codification — resist the urge to shortcut it.

---

## Section 5: Capability / Machinery

Machinery is the standing apparatus — the durable components that configure each execution session. They exist before any session begins and persist after every session ends. A CLAUDE.md file, an agent definition, a database schema, a LaunchAgent plist: these are not ephemeral artifacts of work-in-progress. They are the infrastructure that work runs on. They load and fire. They shape what Claude can do before a single prompt is typed.

Machinery does not live in Knowledge, even though it may contain knowledge. CLAUDE.md files contain summaries of Bible principles, but their purpose is not to inform — it is to configure behavior. Agent definitions encode expertise, but their purpose is not to teach — it is to provide a ready-made specialist for delegation. The test is consult-vs-fire: if it loads automatically and shapes execution, it is Machinery.

Machinery is distinct from Protocols, its sibling sub-layer in Capability. Both configure the system, but they configure different things. Machinery is structural: the agents, the databases, the LaunchAgents, the scripts — the WHAT of the system. Protocols are behavioral: the rules, the hooks, the skills, the sweeps — the HOW of the system. An agent definition (Machinery) defines what the copywriter team can do. A voice enforcement rule (Protocol) specifies how they must behave when drafting as Lee.

The "mature machinery" distinction matters. Agent definitions (47 of them) are Machinery, not Principles, even though they are stable and important. Stability-as-property: they are stable because they have been battle-tested across hundreds of sessions, not because they are foundational normative knowledge. When a new agent is added, it enters through Adaptation (experience informs the definition, the definition gets refined through use) before reaching its current form. Agent definitions ARE the refined output of that process — they are codified practice, not first principles.

Machinery carries a derived-copies obligation. CLAUDE.md contains derived summaries of Bible principles. Rules files implement Bible rules in session-loadable form. This creates a maintenance chain: when canonical sources in Knowledge/Principles change, derived copies in Machinery must update. The nightly arch-sweep (`com.lfi.arch-sweep`, 2:00am daily) catches drift — results in `/tmp/arch-sweep-report.md` and the daily digest. The enforcement traceability matrix (Bible section 3.6) maps each principle to its enforcement mechanisms, making the dependency chain explicit.

### CLAUDE.md — the 4-tier instruction system

The CLAUDE.md system is the primary configuration vector for Claude Code behavior. Four tiers load in sequence, each more specific than the last. Together they total approximately 1,185 lines of standing instructions active in every session.

**Loading order and file paths:**

| Tier | File | Scope | Approx. lines | What it governs |
|------|------|-------|------:|-----------------|
| 1. Global | `~/.claude/CLAUDE.md` | All projects, all sessions | ~200 | Conductor mindset, questioning/verification/steelman protocols, response format, security audit, file naming rules, sentence case |
| 2. Rules | `~/.claude/rules/*.md` | All projects, auto-loaded modular | ~160 | Specific behavioral rules (see table below) |
| 3. Project | `~/CC/Work/LFI/CLAUDE.md` | LFI directory tree | ~290 | Agent sequencing, routing table, integration quick-reference, skills catalog, EA rituals, session maintenance, file organization |
| 4. Operations | `~/CC/Work/LFI/_ Operations/CLAUDE.md` | Operations scripts context | ~646 | Script inventory, integration API docs, folder structure, background services, EA Brain, session index, outreach system |

**Rules files** (auto-loaded every session from `~/.claude/rules/`):

| File | What it governs |
|------|----------------|
| `code-quality.md` | TDD mandate, 10 Commandments of code quality, bestiary hunting (named code smells), good code checklist before completion |
| `Build Bible.md` | Bible quick-reference summary — 14 critical rules, 8 anti-patterns, architecture quick facts, pointers for when to consult the full Bible |
| `steelman.md` | Adversarial plan review protocol — mandatory critique/defend/fold cycle on every plan, upgrade signals for QA swarm and adversarial team |
| `voice.md` | Voice enforcement for communication drafted as Lee — mandatory `personal-voice` skill load, banned words, warmth checklist |
| `integrations.md` | Tool-specific behaviors — Apple Reminders via `remindctl`, Python venvs outside synced folders, Todoist session resume links, background task bug workaround |
| `system-visibility.md` | Visibility prefixes for memory operations and Bible operations — forces explicit markers when reading/writing to memory or Bible files |

**Loading mechanism:** Tiers 1 and 2 load automatically on every session start. Tier 3 loads when the working directory is within the LFI project tree. Tier 4 loads when working within `_ Operations/`. Auto-compact triggers at 70% context usage (`CLAUDE_AUTOCOMPACT_PCT_OVERRIDE`).

**Derived copies note:** CLAUDE.md at all tiers contains summaries of Bible principles. These are derived copies — the canonical source is the Bible (`~/CC/Work/_ Infrastructure/Build Bible.md`). When the Bible changes, CLAUDE.md files must update. The enforcement traceability matrix (Bible section 3.6) maps this dependency chain. Known debt: 8 major duplications (~250 lines) exist across tiers 1-3, including conductor mindset (3x), response format (3x), and critical partner protocol (3x).

**Decision tree for new instructions:** Bible section 3.2 provides the routing logic — universal rules go to Global or Rules, LFI-specific goes to Project, operations/scripts go to Operations. Content belongs in exactly one tier. If it appears in two, one is wrong.

### Agent definitions — 47 agents in 7 teams + specialists

Full catalog: Directory (`~/CC/Work/LFI/_ System/directory.md`, Agents section). Agent roster reference: `~/CC/Work/LFI/_ System/agents/AGENT-REFERENCE.md`.

**3-tier hierarchical model:**

```
Director (Opus, ~5% of work) --> Senior (Sonnet, ~15% of work) --> Junior (Haiku, ~80% of work)
```

Directors create bulletproof task definitions so precise that juniors cannot fail. Seniors decompose director-level specs into junior-executable chunks and quality-check output. Juniors execute using formulas (~80 lines each, no guessing). This implements Bible pattern 2.1 (hierarchical cost optimization 80/15/5).

**7 hierarchical teams (30 agents):**

| Domain | Director | Senior | Junior | Team purpose |
|--------|----------|--------|--------|-------------|
| Development | `dev-director.md` | 3 seniors (frontend, backend, devops) | 8 juniors (python, typescript, shell, css, docker, api, react, database) | All coding work — architecture through execution |
| Product management | `pm-director.md` | `pm-senior.md` | `pm-junior.md` | Project timelines, phases, client care |
| Copywriting | `copywriter-director.md` | `copywriter-senior.md` | `copywriter-junior.md` | Marketing copy, CTAs, headlines |
| Visual design | `visual-designer-director.md` | `visual-designer-senior.md` | `visual-designer-junior.md` | Visual design direction and execution |
| UX architecture | `ux-architect-director.md` | `ux-architect-senior.md` | `ux-architect-junior.md` | Site structure, wireframes, user flows |
| Content strategy | `content-strategist-director.md` | `content-strategist-senior.md` | `content-strategist-junior.md` | Sitemap architecture, content planning |
| Market research | `market-researcher-director.md` | `market-researcher-senior.md` | `market-researcher-junior.md` | Competitive analysis, market positioning |

The Development team is larger than the others because it covers 8 stack specialties at the junior tier. All other teams follow the standard 3-agent Director/Senior/Junior structure.

**Specialist agents (17, no hierarchy):**

| Category | Agent | Definition format | Purpose |
|----------|-------|------------------|---------|
| Strategy | Conductor | Multi-file (`conductor/core.md` + 3) | Master orchestrator for complex multi-phase projects |
| Strategy | Emma Stratton | Single-file | Messaging framework specialist, 11-section methodology |
| Strategy | Brand Strategist | Multi-file (`brand-strategist/core.md` + 4) | Brand positioning and identity strategy |
| Strategy | Voice Analyst | Multi-file (`voice-analyst/core.md` + 3) | Extract voice patterns from transcripts and content |
| Strategy | Messaging Architect | Multi-file (`messaging-architect/core.md` + 4) | Deep messaging architecture, builds on Emma's frameworks |
| Creative | SEO/GEO Strategist | Multi-file (`seo-geo-strategist/core.md` + 4) | SEO and GEO strategy with industry context |
| Creative | Creative Provocateur | Single-file | Pattern-breaking ideas, challenge conventional thinking |
| Technical | Webflow Developer | Multi-file (`webflow-developer/core.md` + 4) | Webflow implementation from design specs |
| Technical | Google Docs Editor | Single-file | All Google Doc work delegated here — no exceptions |
| Technical | SysAdmin | Single-file | System maintenance, file organization, service health |
| Operations | PM Louder Than Ten | Multi-file (`pm-louder-than-ten/core.md` + 3) | Project management with Louder Than Ten methodology |
| Operations | CFO | Single-file | Financial analysis, pricing, profitability modeling |
| Operations | Project Manager | Single-file | Generic project management (distinct from PM LT10) |
| Operations | Sales/CRM | Single-file | Pipeline management, lead qualification, follow-ups |
| Operations | Client Success | Single-file | Client relationship health, retention, upsell identification |
| Operations | Learning Curator | Multi-file (`learning-curator/core.md` + 3) | Reviews all agent outputs for patterns, promotes learnings |
| Operations | Stats Viewer | Single-file | System statistics and analytics display |

Additionally, 3 target client personas exist as simulation agents for messaging testing (industrial manufacturer, service business owner, construction company).

**Agent definition formats:** 8 agents use progressive disclosure (multi-file: `core.md` + examples + templates + checklists per Bible pattern 2.3). This reduces context window consumption by ~60% — `core.md` provides orientation in ~200 tokens, deeper modules load on demand. The remaining agents use monolithic single-file definitions.

**Cost model and routing:** 80% Haiku / 15% Sonnet / 5% Opus within the Claude model family. Cross-model routing adds Gemini (1M+ context, web grounding), Codex (background autonomous tasks), and Perplexity (citation-backed research) as additive specialists — not replacements. Full routing table at Bible section 5.2. Selection decision tree at Bible section 5.3.1.

**Selection mechanism:** `/qq-agents` to browse and invoke the roster. The Conductor selects agents based on the 7-question framework (Bible section 5.1) and the agent routing table.

### Databases — 6 SQLite schemas (+ 2 referenced in backup)

| Database | Location | Records | Query interface | Purpose |
|----------|----------|--------:|----------------|---------|
| `transcripts.db` | `_ Operations/meeting-intelligence/` | 1,900+ | `transcript_intel.py`, SQL, FTS5 | Meeting transcripts and extracted insights. Core intelligence layer. NEVER use Granola MCP (returns empty). |
| `sessions.db` | `_ Operations/session-index/` | 2,900+ | `sessions` CLI, `session_search.py`, FTS5 | Claude Code session index — search past work by content, client, date. |
| `contacts.db` | `_ Operations/` | 2,000+ | `contacts_db.py`, SQL | Unified contacts and outreach log. Single source of truth for all lead/contact data. Aggregates touches from P2P, LinkedIn, email. |
| `ea_brain.db` | `_ Operations/ea_brain/` | — | `ea_brain.surfacer`, commitment tracker, relationship scorer | Commitments, personal intel, relationship scores. Penny's intelligence layer. |
| `fsrs.db` | `_ Operations/memory-system-v1/` | — | FSRS-6 algorithm module | Spaced repetition scheduling for the Memeta memory system. |
| `passive_income.db` | `_ Operations/passive-income/` | — | SQL | Passive income tracking. |

Two additional databases (`p2p_learning.db`, `p2p_outreach.db`) are referenced in the backup script but not inventoried as primary stores.

**Canonical domains (Bible principle 1.5):** Meetings = `transcripts.db`. Session history = `sessions.db`. People = `contacts.db`. Commitments/relationship intel = `ea_brain.db`. Each domain has exactly one canonical store. The `outreach_log` table in `contacts.db` aggregates touches from all outreach channels to prevent double-tapping contacts.

**Backup:** All databases backed up nightly at 3am via `db-backup.sh` using `sqlite3 .backup` (not file copy — corruption-safe). 7-day rotation to Google Drive. Pushover alert on failure. Full backup detail at Bible section 7.5.

**FTS5 indexing:** The three primary databases (transcripts, sessions, contacts) use SQLite FTS5 full-text search indexes, enabling sub-second keyword search across thousands of records without external search infrastructure.

### LaunchAgent plists — 47 scheduled/background processes

All plist files live in `~/Library/LaunchAgents/`. Organized by function category:

| Category | Count | Description | Key agents |
|----------|------:|-------------|------------|
| Meeting and intelligence | 6 | Meeting prep, transcript indexing, intelligence automation | `com.lfi.meeting-nudge` (every 5min), `com.lfi.daily-intelligence` (6:30am), `com.lfi.meeting-indexer`, `com.granola.checker`, `com.lfi.granola-api-sync`, `com.macwhisper.processor` |
| Notifications and rituals | 4 | Daily rituals, digest, EOD capture | `com.lfi.triage-nag` (9am M-F), `com.lfi.eod-nag` (5pm M-F), `com.lfi.daily-digest` (7am), `com.lfi.eod-capture` (5pm weekdays) |
| Pipeline and outreach | 5 | CRM health, outreach sync, prospect staging, commitment mining, CRM enrichment | `com.lfi.pipeline-health` (M/W/F 7:15am), `com.lfi.outreach-sync` (every 30min), `com.lfi.linkedin-staging` (weekly), `com.lfi.commitment-extractor` (daily), `com.lfi.crm-enricher` |
| Approval and execution | 1 | Execute approved items from Todoist queue | `com.lfi.poke-executor` (every 5min) |
| Session and learning | 5 | Session indexing, learning analysis, pattern detection, improvement | `com.lfi.session-indexer` (every 30min), `com.learning.pattern-analyzer` (Monday 7am), `com.learning.system-learner` (daily 8am), `com.lfi.learnings-review` (Friday 5pm), `com.lfi.weekly-improvement` (weekly) |
| Memory system | 2 | Memory maintenance, weekly synthesis | `com.lfi.memory-maintenance`, `com.lfi.memory-weekly-synthesis` (weekly) |
| Dashboards and monitoring | 2 | Dashboard data refresh, service health monitoring | `com.lfi.dashboard-fetcher` (every 5min), `com.lfi.unified-health-monitor` (continuous) |
| Backup and maintenance | 2 | Database backup, log rotation | `com.lfi.db-backup` (daily 3am), `com.lfi.log-rotation` |
| Continuous services | ~20 | Dashboard servers, memory services, monitoring daemons | Various utility agents not individually inventoried in Bible |

**Continuous services** (RunAtLoad + KeepAlive — always running): dashboard servers, memory API server, memory dashboard, total recall dashboard, outreach dashboard, unified health monitor, MacWhisper processor, Granola cache checker.

**Key LaunchAgents to understand:**

- **`com.lfi.unified-health-monitor`** — The system's immune system at the process level. Watches all services, auto-restarts crashed processes. Implements Bible principle 1.12 (observe everything). Without this, silent service death (anti-pattern 6.8) would be the norm.
- **`com.lfi.db-backup`** — Daily 3am, `sqlite3 .backup` to Google Drive. 7-day rotation. Pushover alert on failure. The safety net for all 8 SQLite databases.
- **`com.lfi.meeting-nudge`** — Every 5 minutes, checks for upcoming meetings and triggers dossier generation at 60min/30min marks. Implements Bible pattern 2.13 (ritualization: silent prep, then notification, then engagement).
- **`com.lfi.session-indexer`** — Incremental session index update every 30 minutes. Keeps `sessions.db` current so past-session search via `sessions` CLI returns recent results.
- **`com.lfi.daily-digest`** — 7am daily morning email digest of overnight system activity. The human-facing summary of what all the other LaunchAgents did while Lee was asleep.

### Python scripts — 317 scripts organized by function

This is a large and heterogeneous collection spanning operations, intelligence, integrations, and utilities. Organized by function category rather than exhaustive listing. Full inventory at `_ Operations/CLAUDE.md` (~646 lines).

| Category | Key scripts | Purpose |
|----------|-------------|---------|
| Integration layer | `lfi_integrations.py` | Unified API client for Todoist, Gmail, Calendar, Notion. Single source for all external service access. |
| Meeting intelligence | `dossier_generator.py`, `transcript_intel.py`, `daily_intelligence.py`, `commitment_extractor.py` | Pre-meeting dossiers, transcript querying, daily intelligence automation, commitment mining from transcripts |
| Session management | `session_indexer.py`, `session_search.py`, `session_topic_capture.py`, `rename_session.py` | Session indexing, search, live topic capture, intelligent session naming |
| CRM and outreach | `pipeline_health.py`, `outreach_sync.py`, `linkedin_staging.py`, `crm_enricher.py`, `contacts_db.py` | Pipeline monitoring, outreach synchronization, prospect staging, CRM enrichment, master contacts database |
| Notifications | `notification_launcher.py`, `poke/send_poke_pushover.py`, `poke/start_delayed_poke.sh`, `poke/cancel_poke.sh` | macOS notifications, Pushover push notifications, delayed notification management |
| Hooks | `questioning-nudge.py`, `delegation-check.py`, `bloat-watcher.py`, `component-audit.py`, `session-context.py`, `inbox-monitor.py` + 20 more | PreToolUse, PostToolUse, SessionStart, UserPromptSubmit, PreCompact, SessionEnd event handlers |
| Learning system | `learning-system/pattern-analyzer.py`, `learning-system/system-learner.py`, `learnings_review.py` | Pattern detection, self-learning analysis, weekly learnings review |
| Memory system | `session-memory-consolidation.py`, various memory-ts scripts | Session memory extraction, deduplication, memory-ts entry creation |
| Content engine | `linkedin-generator.py`, `proof_points_extractor.py` | LinkedIn post generation from voice patterns, marketing quote mining from transcripts |
| EA rituals | `triage_data.py`, `eod_capture.py` | Morning triage data consolidation, end-of-day session summary |
| Backup | `db-backup.sh`, `cc-backup.sh` | SQLite backup (3am), full codebase backup (2am) |
| Approval queue | `poke_bridge.py`, `poke_executor.py` | Todoist-based approval queue enqueue and execution. Pattern 2.11 (async human-in-loop). |

**Scripts Claude or Lee interact with directly:**
- `sessions` CLI — past session search, used when Lee asks "have we done X before?" or "when did we discuss Y?"
- `lfi_integrations.py` — imported by other scripts; also used directly for Calendar, Gmail, Notion, Todoist access
- `contacts_db.py` — contact lookup, import, and export
- `transcript_intel.py` — meeting transcript search and analysis

### Routing tables, templates, configuration

**Routing tables:** Define which agents handle which task types. Located in Bible section 5.2 (agent routing table) and section 5.4 (agent sequencing dependency table). The routing logic is the primary mechanism by which the Conductor delegates to specialists. Key constraint: agent sequencing dependencies are non-negotiable — Voice Analyst before Emma Stratton, Content Strategist before UX Architect, UX Architect before Visual Designer.

**Templates:** Task notes templates at `~/Work/LFI/_templates/task-notes/` (context.md, decisions.md, open-questions.md). When complex tasks warrant notes files, clone from templates. Agent templates at `_ System/agents/_templates/` include copywriting formulas, messaging frameworks, PM specs, design specs, UX specs, content specs, research templates, development patterns (7 stack-specific files), and code review template.

**Statusline configuration:** Current statusline config at `~/.claude/statusline-dark.sh`. Governs the visual display in the terminal — context usage percentage (color-coded), model name, working directory, session ID, and current topic (from `~/.claude/session-topics/{session_id}.txt`).

**Plugins (6 configured):**

| Plugin | Source | Purpose |
|--------|--------|---------|
| `hookify` | `claude-plugins-official` | Create hooks from natural language — "prevent this behavior" converts to a hook rule |
| `frontend-design` | `claude-plugins-official` | Frontend design patterns and UI component assistance |
| `orchestration` | `orchestration-marketplace` | Multi-agent workflow syntax with `.flow` files — adds `/orchestrate`, `/create`, `/run` commands |
| `code-review` | `claude-plugins-official` | Structured code review workflows |
| `vercel` | `claude-plugins-official` | Vercel deployment integration |
| `dx` | `ykdojo` | Developer experience enhancements |

**Dashboards (5):**

| Dashboard | Port | Type |
|-----------|-----:|------|
| Memory dashboard | 8766 | Static HTML, rebuilt on data change |
| LFI operations dashboard | 8701 | Static HTML, rebuilt on data change |
| Total recall dashboard | 7860 | Gradio-based (Memeta interface) |
| Memory API server | 8765 | Live Bun process (memory-ts backend, not visual) |
| Outreach dashboard | — | Static HTML, rebuilt on data change |

All visual dashboards use static HTML rebuilt on data change — no server-side state, no APIs, no framework. The memory API server is the exception: it is a live Bun process serving the memory-ts system.

**Environment configuration** (from `~/.claude/settings.json`):

| Setting | Value | Why |
|---------|-------|-----|
| `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` | `1` | Enable team-based multi-agent orchestration |
| `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` | `70` | Trigger auto-compaction at 70% context usage |
| `autoUpdatesChannel` | `latest` | Track latest release channel |
| `defaultMode` | `acceptEdits` | Auto-accept file edits without confirmation |

**Integrations (12 external services):**

| Service | Method | Single source location |
|---------|--------|----------------------|
| Calendar | `lfi_integrations.py` | `lfi_integrations.calendar` |
| Gmail | `lfi_integrations.py` | `lfi_integrations.gmail` (dual-account: work + personal) |
| Google Docs | MCP server | MCP tools |
| Transcripts | `transcripts.db` (SQLite) | `_ Operations/meeting-intelligence/` |
| Notion | `lfi_integrations.py` | `lfi_integrations.notion` |
| Todoist | `lfi_integrations.py` | `lfi_integrations.todoist` |
| Pushover | `poke/send_poke_pushover.py` | CLI script |
| 1Password | `op` CLI | `op read "op://Claude Secrets/[Item]/[field]"` |
| Webflow | MCP server | MCP tools |
| GitHub | `gh` CLI | Bash |
| Gemini | `/opt/homebrew/bin/gemini` | Homebrew |
| Memory-ts | Bun scripts | `/opt/homebrew/lib/node_modules/@rlabs-inc/memory/` |

**Decision tree for integration method:** Bible section 3.4 provides the full routing — MCP if available and working, `lfi_integrations.py` if supported, dedicated CLI if available, build into `lfi_integrations.py` as last resort (never create new standalone clients).

**Derived copies principle:** The consistency obligation for Machinery: every component in this sub-layer that summarizes or implements Principles content must be updated when the canonical source changes. This includes:
- CLAUDE.md at all 4 tiers (summarizes Bible rules)
- Rules files in `~/.claude/rules/` (implement Bible rules in auto-loadable form)
- Verification protocol files (implement Bible verification requirements)
- Agent definitions (implement Bible patterns like progressive disclosure and cost optimization)

When the Bible changes: trace the enforcement chain using Bible section 3.6 (enforcement traceability matrix), then update all derived copies. The nightly arch-sweep automates drift detection — it runs at 2:00am, checks key doc existence and component counts, and reports to the daily digest.

🔖 **LEE:** Agent definitions are mature machinery — stable because battle-tested, not because foundational. When you add a new agent, register it in the Directory (`_ System/directory.md`) and update the agent count here. Agent and LaunchAgent counts drift over time; the nightly arch-sweep catches discrepancies and reports them in the daily digest. Current counts: 47 agents, 63 LaunchAgents, 6 primary databases, 318 Python scripts, 12 integrations, 6 plugins, 5 dashboards.

---

---

## Section 6: Capability / Protocols

Protocols are the normative layer of Capability. They specify what SHOULD happen, not what always happens. The distinction from Machinery is ontological: Machinery IS the apparatus (agents, databases, scripts exist as structural facts), while Protocols OUGHT TO govern behavior (rules, hooks, skills prescribe expected conduct). A database schema is a fact about the system. A rule that says "never write to the database without a transaction" is a behavioral commitment about how the system should be used.

The is/ought distinction matters in practice. A hook is not just a piece of code — it is a behavioral commitment. "On PreToolUse, check whether the conductor should be delegating instead of executing" is not a description of what happens; it is a prescription of what must happen on every tool invocation. A skill is not just a prompt file on disk — it is a behavioral capability that changes what Claude can do when invoked. Skills and hooks are both code, but their architectural role is governance, not infrastructure.

Protocols organize into four categories, following a military taxonomy: SOPs (standing operating procedures — persistent behavioral rules that apply to every session), Sweeps (targeted systematic audits against specific quality concerns), Capabilities (skills and plugins that extend what the system can do), and Event automation (hooks that fire automatically on lifecycle events). SOPs are the standing orders. Sweeps are the inspections. Capabilities are the specialized equipment. Event automation is the immune system.

The immune system analogy deserves elaboration because hooks are the most powerful and the most dangerous part of Protocols. Hooks fire without being asked, detect conditions, and trigger responses — exactly like an immune response. A PostToolUse hook that validates output is the immune response to quality failure. A PreToolUse hook that blocks execution after three delegation strikes is the immune response to conductor laziness. But a broken hook that fires on every event and blocks all work is an autoimmune disorder — the immune system attacking the body it is supposed to protect. The 35 hooks in this system represent significant governance power, and that power demands careful testing before deployment.

### SOPs — standing operating procedures

Rules files and protocols that govern standing behavior across all sessions. These are persistent — they load at session start and shape behavior throughout.

**Rules files** (`~/.claude/rules/` directory, auto-loaded every session):

| File | What it governs | Key behaviors enforced |
|------|----------------|----------------------|
| `code-quality.md` | Code quality standards | TDD red/green/refactor mandate; 10 Commandments (no `any`, no silent catches, atomic commits, 500-line limit, etc.); bestiary hunting (named code smells: Any Hydra, Silent Catch Wraith, God Class Leviathan, useEffect Kraken, etc.); session-start ritual (run tests first); good code checklist before claiming completion |
| `Build Bible.md` | Bible quick-reference | 14 critical rules summary; 8 anti-patterns with detection signals; architecture quick facts; pointers for when to load the full Bible (~1,642 lines) |
| `steelman.md` | Plan review protocol | Mandatory adversarial review before execution on every plan; critique-defend-fold cycle; upgrade signals (steelman to QA swarm to adversarial team); integration with questioning, planning, and verification flow |
| `voice.md` | Communication style | Mandatory `personal-voice` skill load before any draft as Lee; banned words (align, touch base, circle back, bandwidth, actually, exactly); warmth checklist; no post-hoc voice checking — load skill first, then write |
| `integrations.md` | Tool-specific behaviors | Apple Reminders via `remindctl` CLI; Python venvs outside cloud-synced folders; Todoist task format for session resumption (include `claude --resume` command); background task bug workaround (never use `run_in_background: true`) |
| `system-visibility.md` | Operation visibility markers | Memory operations prefixed 🧠; Bible operations prefixed 🛐; Atlas operations prefixed 🗺️; Directory operations prefixed 🗂️. Distinguishes active reads from system-injected context. |
| `atlas.md` | Atlas section index | KCA model quick-ref, placement tests, 6 arrows, document triangle. Section routing table — "when to load" triggers for each of the 11 sections. Entry point into the Atlas without loading it in full. |

**Verification protocol:**
- File: `~/CC/Work/LFI/_ System/verification-protocols.md`
- What it governs: mandatory verification before claiming completion. "Never use completion language ('done', 'fixed', 'working', 'complete') without verification evidence."
- Mandatory for: code changes, build changes, behavioral changes, doc references, config changes, anything affecting functionality
- Integration chain: Questioning protocol BEFORE work -> Steelman DURING planning -> Verification AFTER work. All three mandatory, every time.

**Questioning protocol:**
- File: `~/CC/Work/LFI/_ System/questioning-protocol.md`
- What it governs: pre-work clarification. "Default to questioning. When in doubt, ask."
- Key behavior: 2-4 questions per ambiguous request; multiple-choice preferred over open-ended; "grill me" mode for major changes and risky decisions
- Target: ask BEFORE starting work, not after discovering ambiguity mid-execution

**Mistake documentation protocol:**
- File: `~/CC/Work/LFI/_ System/mistake-documentation-protocol.md`
- What it governs: mandatory capture when mistakes occur
- Mechanism: `memory-ts add --type mistake --content "[What] | [Why] | [How]" --intent high --tags [project],[domain],[type],[severity]`
- Pre-work: query mistakes for relevant domain/project and internalize prevention strategies before starting

**Security audit protocol:**
- Lives in: Global CLAUDE.md (tier 1)
- What it governs: mandatory security review before installing any new skill, agent, plugin, or hook
- Process: run `/skill-auditor [path-or-url]`; review audit report; only proceed if SAFE TO INSTALL; INSTALL WITH CAUTION requires Lee discussion; DO NOT INSTALL means full stop
- Risk levels: Skills (medium), Agents (high), Plugins (high), Hooks (critical — run automatically on events)

**Voice enforcement:**
- File: `~/.claude/rules/voice.md`
- Reference: `~/Work/LFI/_ System/reference/lees-voice.md`
- What it governs: all communication drafted as Lee must load `personal-voice` skill FIRST — no exceptions, no post-hoc checking
- Key constraint: skill loading happens before drafting, not after. The skill shapes the writing from the first word.

**Codify-agent-failures rule:**
- Lives in: Bible section 8.3
- What it governs: when an agent makes a mistake that wastes meaningful time, add a specific preventive NEVER rule to the relevant CLAUDE.md tier immediately
- Rationale: negative rules ("NEVER do X because Y happened") prevent the exact class of mistake that actually occurred; CLAUDE.md rules are active every session while notes are passive and get buried

### Sweeps — targeted systematic audits

A sweep is any targeted systematic quality audit against a specific concern, run across the codebase or content. The concept is the unit; specific sweeps are instances of that concept.

**What a sweep is:**

1. A specific quality concern is identified (e.g., "no files > 500 lines", "no `any` types", "no silent catch blocks")
2. The codebase is scanned systematically for violations
3. Violations are catalogued with location, severity, and remediation guidance
4. Violations are remediated (or documented with justification for exceptions)
5. Results are reported

Sweeps differ from one-off fixes in three ways: they are systematic (entire codebase, not one file), they are targeted (one specific concern per sweep, not general review), and they are reusable (the same sweep can run repeatedly as a regression check).

**The 14 code quality sweep types:**

Each sweep has a dedicated `/church-[name]` slash command. The `/church` command is the orchestrator that coordinates one or more specialist sweeps.

| Sweep | Command | What it audits | Bible principle |
|-------|---------|---------------|-----------------|
| Naming | `/church-naming` | Variable, function, file naming quality | Commandment VIII (names are documentation) |
| Type | `/church-type` | Type safety violations, `any` usage, unchecked casts | Commandment I (no `any` types) |
| Test | `/church-test` | Missing tests, test quality, coverage, red phase confirmation | Bible 1.3 (TDD) |
| Dead code | `/church-dead` | Unused code, zombie TODOs, commented-out blocks | Commandment IX (delete dead code) |
| Architecture | `/church-arch` | Architecture violations, SRP breaches, dependency direction | Bible 1.5 (single source of truth) |
| Dependencies | `/church-dep` | Unjustified packages, version issues, phantom dependencies | Commandment VII (justify dependencies) |
| File size | `/church-size` | Files approaching or exceeding 500 lines | Bible 1.4 (simplicity), anti-pattern 6.7 |
| Observability | `/church-observability` | Missing logging, silent failures, unmonitored services | Bible 1.12 (observe everything) |
| React | `/church-react` | React anti-patterns — useEffect Kraken, God Components, Infinite Re-render Ouroboros | Engineering bestiary |
| Accessibility | `/church-a11y` | Accessibility violations in UI code | Web standards |
| Copy | `/church-copy` | Writing quality, voice violations, banned words | Content quality, voice enforcement |
| Adaptive | `/church-adaptive` | Adaptive content quality, complexity calibration | Bible 1.4 (simplicity) |
| Git | `/church-git` | Git commit quality, message conventions, atomic commit violations | Commandment III (atomic commits) |
| Secret | `/church-secret` | Secrets in code or commits — API keys, tokens, credentials | Commandment X (no secrets in commits) |

**The sweep concept extends beyond code.** The same systematic-audit pattern applies to any domain:
- Design quality sweep: systematic review of UI/UX decisions against design principles
- Copy quality sweep: systematic review of all text in a product against voice guidelines
- Product quality sweep: systematic review against product principles and user needs
- Infrastructure sweep: systematic review of LaunchAgents, databases, integrations for health and alignment
- New sweep types enter through the evolution protocol (Bible section 8)

**Running sweeps:**
- `/church` — orchestrator that coordinates one or more sweep types
- Individual `/church-[name]` commands for targeted single-concern sweeps
- Output: catalogue of violations with location and severity -> remediation tasks
- Sweeps are idempotent — running the same sweep twice on clean code produces zero violations

**Sweeps vs hooks — complementary enforcement:**

| Dimension | Hooks | Sweeps |
|-----------|-------|--------|
| Timing | Real-time (fires on events) | On-demand (user invokes) |
| Scope | Single tool call / session event | Entire codebase |
| Coverage | Narrow (specific event type) | Broad (systematic scan) |
| Purpose | Prevent violations as they happen | Remediate accumulated violations |
| Analogy | Adaptive immune response | Periodic health checkup |

Both are necessary. Hooks catch violations in real time but can only see individual events. Sweeps catch accumulated violations that slipped past hooks or pre-date hook installation. A system with only hooks accumulates invisible debt. A system with only sweeps catches problems too late.

🔖 LEE: Hooks prevent violations in real time. Sweeps remediate accumulated violations. The immune system metaphor: hooks are the adaptive immune response (immediate, targeted), sweeps are the periodic health checkup (systematic, proactive). Both are necessary. Neither alone is sufficient.

### Capabilities — skills and plugins

**Skills (100+, two locations):**

Skills are specialized behaviors loaded on demand when invoked by Claude or Lee. They are not always-on like rules files — they load when needed and provide context-specific expertise for a particular task type.

Location 1 — Personal skills (`~/.claude/skills/`, 30 skills):

| Category | Skills | Purpose |
|----------|--------|---------|
| Engineering | `code-quality`, `component-hardener`, `debugging`, `python-best-practices`, `react-email`, `test-driven-development`, `datetime-context` | Code quality auditing, security hardening, debugging methodology, language-specific practices, TDD workflow, temporal context |
| Penny (EA brain) | `penny-prep`, `penny-followup`, `penny-dossier`, `penny-status` | Pre-meeting intelligence, post-meeting processing, full person dossier, background service health |
| Strategy | `competitive-analysis`, `gtm-pricing`, `launch-gtm-execution`, `marketing-strategy-pmm` | Competitive analysis, pricing strategy, launch execution, product marketing |
| Sales | `cold-email-sequence-generator`, `outbound-optimizer`, `sales-enablement`, `sales-methodology-implementer` | Email sequences, outbound optimization, sales enablement, methodology implementation |
| Integrations | `apple-reminders`, `reddit-fetch`, `resend` | Apple Reminders via remindctl, Reddit content via Gemini (WebFetch workaround), Resend email API |
| Meta | `find-skills`, `sequential-thinking`, `qq-groom-claude`, `qq-add-memory`, `qq-arch-add`, `qq-arch-load` | Skill discovery, step-by-step reasoning, CLAUDE.md grooming, memory addition, component registration (with placement tests), Atlas section routing |
| Creative | `canvas-design`, `pptx`, `slack-gif-creator` | Visual design assets, PowerPoint, animated GIFs |

Location 2 — External/installed skills (`~/CC/.agents/skills/`, 21 skills):

| Category | Skills | Source |
|----------|--------|--------|
| Marketing | `copywriting`, `copy-editing`, `marketing-psychology`, `seo-audit`, `competitor-alternatives` | coreyhaines31/marketingskills |
| Development | `vercel-react-best-practices`, `web-design-guidelines`, `build-production-grade` | vercel-labs |
| Tooling | `gemini`, `agent-md-refactor`, `qa-test-planner`, `verification-before-completion` | softaworks/agent-toolkit, obra/superpowers |
| Voice | `personal-voice` | Local — mandatory for all communication drafted as Lee |
| Other | `agent-browser`, `browser-automation`, `github-standards`, `writing-guidelines`, `pdf-guide-generator`, `chatgpt-share-link-creator`, `cover-variant-generator`, `promptbase-automation`, `overnight-runner`, `crm-lead-intake`, `review-learnings` | Various |

Location 3 — Local system skills (`_ System/skills/`, 2 skills):

| Skill | Purpose |
|-------|---------|
| `qq-docs-fix` | Assess and fix project documentation structure. Scores docs 0-10 and restructures. |
| `qq-project-file-cleanup` | Enforce SysAdmin file hygiene standards — categorize and clean up accumulated cruft. |

**How skills differ from hooks:**
- Skills are invoked — Claude or Lee must explicitly call them via `/[skill-name]` or the Skill tool. They load specialized behavior for a specific task, execute, and are done.
- Hooks are armed — they register at session configuration time and fire automatically when their trigger event occurs. No invocation needed.
- Skills can spawn hooks (via the hookify plugin). Hooks can reference skills. But they are architecturally distinct: skills are opt-in capabilities, hooks are always-on governance.

**Skill protocol** (from LFI CLAUDE.md): When a task matches a skill trigger, check the skills directories. If missing, install with `npx skills add [source] --skill "[name]"`, then read SKILL.md before executing. Reading the SKILL.md is mandatory — it contains the system prompt that shapes the skill's behavior.

**Slash commands (~65):**

Slash commands are user-invokable workflows defined as `.md` files in `~/.claude/commands/`. They overlap with but are distinct from skills: a command is a prompt template invoked by typing `/command-name`; a skill is a behavior module that loads specialized context. Some commands invoke skills; some are standalone.

| Category | Count | Key commands |
|----------|------:|-------------|
| Session management (`qq-*`) | 11 | `/qq-done` (clean exit), `/qq-handoff` (context save), `/qq-rename` (session naming), `/qq-search` (past sessions), `/qq-pause` (park with Todoist), `/qq-agents` (browse roster), `/qq-codereview` (quality audit) |
| Code quality crusade (`church-*`) | 15 | `/church` (orchestrator), `/church-test`, `/church-type`, `/church-size`, `/church-arch`, `/church-dead`, `/church-secret`, + 8 more |
| Meeting commands | 5 | `/meeting-search`, `/meeting-history`, `/meeting-people`, `/meeting-stats`, `/meeting-enrich-calendar` |
| Agent tier commands | 22 | `/emma`, `/conductor`, `/sales`, `/pm-{junior,senior,director}`, `/copywriter-{junior,senior,director}`, `/visual-designer-{...}`, `/ux-architect-{...}`, `/content-strategist-{...}`, `/market-researcher-{...}` |
| Other | ~12 | `/verify`, `/qa-swarm`, `/handoff`, `/review-learnings`, `/gemini`, `/triage`, `/eod-triage` |

**Plugins (6 configured in `~/.claude/settings.json`):**

| Plugin | What it adds | Architectural role |
|--------|-------------|-------------------|
| `hookify` | Hook creation from natural language — "prevent this behavior" becomes a hook | The Adaptation arrow in action: observed unwanted behavior -> Protocol change (new hook) without writing hook code |
| `orchestration` | Multi-agent workflow syntax with `.flow` files — `/orchestrate`, `/create`, `/run` | Complex workflow definition and execution beyond what individual slash commands support |
| `code-review` | Structured code review workflows | Codifies review process rather than relying on ad-hoc review |
| `frontend-design` | Frontend design patterns and UI component assistance | Design system knowledge for UI work |
| `vercel` | Vercel deployment integration | Deployment pipeline integration |
| `dx` | Developer experience enhancements | General DX improvements |

The `hookify` plugin deserves special attention because it is a meta-capability: it creates new Protocols from natural language. "I want to prevent Claude from making direct git pushes without confirmation" -> hookify generates a PreToolUse hook on Bash commands matching `git push`. This is the Adaptation mechanism in action: experience (observed unwanted behavior) -> Protocol change (new hook) -> without requiring Lee to write hook code manually. The plugin makes hook creation accessible but does not make it safe — review what hookify generates before approving it.

### Event automation — hooks

**37 hooks across 7 event types:**

The hook system is the immune system of the Protocols layer. Hooks fire without being asked, on specific lifecycle events, and implement behavioral governance that cannot rely on Claude remembering to do it. All hooks are configured in `~/.claude/settings.json`. Scripts live in `_ Operations/hooks/` unless otherwise noted.

**7 event types:**

| Event type | Hook count | When it fires | Worst-case timeout | Purpose |
|------------|----------:|---------------|--------------------:|---------|
| SessionStart | 4 | When a Claude Code session begins or resumes | 20s | Setup — load context, check inbox health, initialize memory |
| UserPromptSubmit | 8 | When Lee sends a message | 28s | Audit — track exchanges, capture topics, detect projects, cancel pending notifications, surface arch-sync reminders |
| PreToolUse | 7 | Before any tool call executes | 12s | Block + audit — delegation enforcement, file size warnings, questioning reminders, component security audits, arch-sync detection |
| PostToolUse | 1 | After a tool call completes | 5s | Notification — start delayed Pushover notification when Claude asks for input |
| PreCompact | 3 | Before context compaction | 128s | State preservation — capture session name, topic, and curate memories before context loss |
| SessionEnd | 8 | When session closes | 455s | Cleanup + capture — memory consolidation, session naming, topic capture, state capture, response format check, hook stats |

**Enforcement tiers in practice:** Only `delegation-check.py` blocks execution (3-strike escalation per Bible pattern 2.8: strikes 1-2 send macOS notifications, strike 3 blocks the tool call + creates Todoist review task, resets on successful delegation). `bloat-watcher.py` and `gdocs-agent-reminder.sh` are advisory (warn once, never block). All other hooks audit, capture, or enrich — they never prevent work.

**Hook inventory by category:**

| Category | Hooks | Purpose | Key hooks |
|----------|------:|---------|-----------|
| Quality gates | 3 | Prevent quality violations in real time | `delegation-check.py` (blocks after 3 solo-execution strikes), `bloat-watcher.py` (warns on >500-line files), `component-audit.py` (security audit on new components) |
| Behavioral nudges | 2 | Remind and guide behavior | `questioning-nudge.py` (rotating reminders to question before acting), `gdocs-agent-reminder.sh` (reminder to delegate Google Docs work) |
| Architecture sync | 2 | Keep Atlas and Directory in sync after edits | `arch-sync-reminder.py` (PreToolUse: detects atlas.md or directory.md edits, writes reminder to `/tmp`), `arch-sync-surface.py` (UserPromptSubmit: surfaces pending reminder in chat, clears file) |
| Capture automation | 9 | Automatic experience and context capture | `session_topic_capture.py` (3 event types), `periodic-rename.py` (2 event types), `session-memory-consolidation.py`, `session-state-capture.py`, `auto-rename-session.py`, `memory-ts curation.ts` (2 event types) |
| Session tracking | 4 | Track session state and exchanges | `session-exchange-counter.py`, `preview-rename-at-10.py`, `project-detector.py`, `skill-usage-tracker.py` |
| Context management | 2 | Smart context loading and inbox monitoring | `session-context.py` (session start intelligence), `inbox-monitor.py` (warns when inbox exceeds 5 files) |
| Notifications | 2 | Push notification management | `start_delayed_poke.sh` (start 5-min timer on AskUserQuestion), `cancel_poke.sh` (cancel timer when Lee responds) |
| Memory system | 3 | Memory-ts integration | `memory-ts session-start.ts`, `memory-ts user-prompt.ts`, `memory-ts curation.ts` |
| Reporting | 2 | Session-end statistics and format checking | `stats.py` (hook system stats for last 7 days), `response-summary-check.py` (check for required summary format) |

**Hook support infrastructure:**

| File | Purpose |
|------|---------|
| `hook_logger.py` | Centralized hook event logger — all hooks log triggers and outcomes |
| `behavior_metrics.py` | Behavior metrics collector for dashboard |
| `delegation-strike-tracker.py` | Tracks delegation violations per session for 3-strike enforcement |
| `hook_events.jsonl` | JSONL log of all hook events, max 5MB with rotation |
| `get-session-title.py` | Utility to extract session title, used by rename hooks |
| `test-hooks.sh` | Test harness for hooks |

**SessionEnd bottleneck:** 8 hooks with 455s worst-case total timeout. The two heaviest are `session-memory-consolidation.py` (180s) and `session_content_spotter.py` (120s — note: this script is referenced in settings.json but NOT FOUND ON DISK). Proposed fix (from Bible section 3.3): move both to async LaunchAgent processing, reducing SessionEnd to ~155s.

**Known fragility (from Bible section 3.3):**
- 3 hooks share a file marker approach (`~/.claude/tmp/`) for session identification. If markers collide or are not cleaned up, hooks misidentify sessions. Proposed fix: use `CLAUDE_SESSION_ID` environment variable instead.
- No circuit breaker exists for cascading hook failures. A broken hook in SessionEnd does not prevent subsequent hooks from running, but the total timeout accumulates.
- `session_content_spotter.py` is referenced in the SessionEnd hook configuration but does not exist on disk — a ghost reference.
- `learning-detector.py` exists as a support file but is not configured in settings.json — undocumented whether active or deprecated.

**Bypass mechanism:** Set `SKIP_HOOK_[NAME]=1` as an environment variable to disable a specific hook for a session. Example: `SKIP_HOOK_DELEGATION_CHECK=1`.

**The hookify meta-capability:**

The hookify plugin (see Capabilities section above) creates new hooks from natural language. "I want to prevent Claude from making direct git pushes without confirmation" -> hookify generates a PreToolUse hook on Bash commands matching `git push`. This is the Adaptation arrow: experience (observed unwanted behavior) -> Protocol change (new hook) -> without requiring Lee to write hook code manually.

**Hook creation safety protocol:**
1. hookify generates the hook definition
2. Lee reviews the generated hook before approving
3. New hooks start in advisory mode (warn, don't block) for a trial period
4. After trial, hooks can be promoted to blocking enforcement if the violations they catch are real
5. Hooks that never fire are candidates for deletion (Bible section 8.3 deletion criteria)

🔖 LEE: Hooks are the immune system. A broken hook is like an autoimmune disorder — it attacks the work it is supposed to protect. Every new hook should be tested via `test-hooks.sh` before deploying. The `hookify` plugin makes hook creation accessible but does not make it safe — always review what hookify generates before approving. The SessionEnd bottleneck (455s worst-case) is the most visible symptom of hook accumulation — monitor it and consider async migration for heavy hooks. The ghost reference to `session_content_spotter.py` should be resolved: either create the script or remove the hook configuration.

---

## Section 7: Activity / Execution

### What "right now" means architecturally

Activity is the only layer where actual work happens. Every file written, every agent dispatched, every client deliverable produced, every database row inserted — all of it occurs in Activity/Execution. Knowledge holds what the system knows. Capability holds what the system can do. Activity is where those two meet reality. Code gets written here. Proposals get drafted here. Meetings get prepped here. It is the sole site of value production.

And yet Activity is architecturally the lightest layer. Everything here is transient. A Claude Code session runs for minutes or hours, then ends. A LaunchAgent process fires, does its work, exits. A Task-spawned agent completes and its execution context evaporates. Nothing in Activity persists after the process ends — not unless the Capture arrow (section 8) carries it back to Knowledge/Experience. This is the fundamental paradox: the layer that produces all value has no memory of its own. A session that doesn't capture its insights, mistakes, or decisions leaves no trace. It happened, it produced artifacts (files, database writes, deliverables), and those artifacts persist in their respective layers — but the *execution itself* is gone.

Why model something so ephemeral? Because Activity is where Architecture meets Reality. The gap between what Knowledge says should happen (Build Bible: "orchestrate, don't execute") and what Activity actually does (Claude writing code solo for 20 minutes) — that gap is drift. Drift is invisible without a model of what Activity is and how it should behave. The 60 LaunchAgents define what *should* run in the background. But are they actually running? Are they succeeding? Are they capturing results? Those questions live in Activity, and answering them requires understanding Activity as an architectural layer, not just "stuff that's running."

How Execution is born and dies. Born: the Configuration arrow fires at session start — CLAUDE.md loads across all four tiers (global, rules, project, operations), hooks arm across 6 event types, skills become available for invocation, MEMORY.md loads empirical context, and the session has a configured execution environment. Each execution context (session, agent, script, LaunchAgent process) is spawned from Capability machinery. Dies: work completes, the session closes, the process exits, the script returns. The lifecycle is always: **Configure → Execute → Capture → End.** The Capture step is not guaranteed — it requires deliberate action (Claude writing to MEMORY.md, hooks firing PostToolUse, scripts logging results) or it simply doesn't happen. Uncaptured Activity is lost Activity.

The paradox of Activity's authority. Activity has the lowest architectural authority of any layer. What happens in execution does not change the Architecture — only the Capture, Adaptation, and Codification arrows can do that (section 8). A mistake made in a session is just a mistake until Capture sends it to Experience, where it may eventually influence Protocols (via Adaptation) or Principles (via Codification). A brilliant new workflow discovered during execution is just a one-off until it's captured and codified. Activity is where all value is produced, but Activity does not shape the system. The feedback arrows do that. This is by design — it prevents ephemeral execution from destabilizing slow-changing layers. A bad session cannot corrupt Principles. It can only corrupt Principles if someone lets the Codification arrow fire without proper human judgment.

---

### Component types

#### Claude Code sessions

The primary execution context. Each session is a fresh instantiation of the configured system: the 4-tier CLAUDE.md hierarchy loads (global `~/.claude/CLAUDE.md` → rules `~/.claude/rules/*.md` → project `.claude/rules/*.md` → operations-level CLAUDE.md), 35 hooks arm across 7 event types (PreToolUse, PostToolUse, UserPromptSubmit, SessionStart, SessionEnd, Stop, PreCompact), MEMORY.md loads empirical context from past sessions, and the full Capability layer becomes available. Lee is present (foreground interactive session) or absent (background session — rare but possible).

**Key properties:**

- Each session is stateless at the Execution level — no memory persists from previous sessions except via Knowledge/Experience (MEMORY.md, session index in sessions.db, task notes). Session N has no direct link to session N-1. Continuity is maintained through captured artifacts, not through execution state.
- Sessions are the primary channel for Lee-Claude interaction and work production. The 2,900+ sessions in the session index represent 2,900+ discrete execution contexts, each born and dead.
- Session transcript feeds the session index via the Capture arrow — `rename_session.py` fires at session end, and the session-index package catalogs the session for future reference.
- Context compaction occurs within long sessions — Claude's context window fills and earlier content is compressed. This is an intra-session lifecycle event, not a cross-session one.

**Failure mode:** Session crash. On restart, CLAUDE.md reloads, rules re-arm, MEMORY.md re-loads. Work-in-progress that was not captured is lost. Mitigation: capture frequently — update MEMORY.md, write task notes, commit code. The smaller the gap between "work done" and "work captured," the lower the blast radius of a crash.

#### Running agents (Task-spawned subagents)

Spawned via the Task tool within a session. These are the delegation mechanism — the Conductor dispatches work to specialist agents (47 defined in `_ System/agents/`) rather than executing solo (Bible rule 1.1: orchestrate, don't execute).

**Key properties:**

- Inherit the parent session's configured environment or receive explicit context via the Task tool prompt. Agent definitions in `_ System/agents/` configure their behavior, expertise, and constraints.
- Can run in parallel (independent tasks — multiple Task calls in a single message) or sequentially (dependent tasks — one agent's output feeds the next).
- Background agents run without Lee present; results return when complete. Foreground agents run with immediate feedback; Lee can observe and redirect.
- Agent sequencing matters — the LFI CLAUDE.md defines a dependency chain (e.g., Voice Analyst before Brand Strategist before Messaging Architect before Copywriter). Violating sequencing produces wasted work.

**Failure mode:** Silent background failure. An agent completes with an error but the parent session does not notice because it has moved on to other work. Mitigation: always check background agent outputs before proceeding. Do not treat background agent completion as guaranteed success. (Note: `run_in_background: true` on Bash commands is avoided entirely due to a known bug where background results crash sessions after context compaction. Use the Task tool for parallel work instead.)

#### Tool calls in progress

Individual tool invocations within a session: Read, Write, Edit, Bash, Grep, Glob, Task, WebFetch, WebSearch, NotebookEdit, and MCP tool calls (Google Docs, Granola, etc.).

**Key properties:**

- Atomic at the tool level — each call is a discrete operation that succeeds or fails.
- Not atomic at the session level — a multi-step task (read file, edit three sections, run tests) can fail partway through, leaving the codebase in an intermediate state.
- Hooks fire around tool calls: PreToolUse hooks can block or modify a tool call before it executes; PostToolUse hooks can act on the result after completion. These are the Protocols layer governing Activity in real time.

**Failure mode:** Partial state from a failed tool sequence. If a multi-step edit fails after the second of three edits, the file is in an intermediate state that neither matches the original nor the intended final state. Mitigation: the atomic operations pattern (Bible rule 1.9 — temp file + rename) prevents partial state for critical writes. For multi-file changes, commit or checkpoint before the sequence begins so rollback is possible.

#### Active scripts (Python scripts invoked via Bash)

288 Python scripts exist in the system; some subset is running at any given time — invoked by LaunchAgents on schedule, by Claude via the Bash tool, or directly by Lee in a terminal.

**Key properties:**

- Run in the shell context of the session or LaunchAgent that invoked them. They may use `lfi_integrations.py` for external service access (Todoist, Gmail, Calendar, Notion) or interact with one of the 6 SQLite databases directly.
- May write to databases, files, logs, or external services (Pushover notifications, email).
- Not always visible to Claude — a script invoked by a LaunchAgent runs entirely outside any Claude Code session. Its output appears only in logs or database records.

**Failure mode:** Script runs, fails silently, produces no output or wrong output. A database write that silently corrupts data is worse than a script that crashes loudly. Mitigation: structured logging (Bible rule 1.12 — observe everything), health monitoring via arch-sweep, and Pushover alerts for critical failures.

#### Active LaunchAgent processes

47 LaunchAgent plists define scheduled and persistent background processes. At any moment, several are actively executing.

**Key properties:**

- Run independently of Claude Code sessions — they fire even when Lee is not using Claude, even when no session is active. They are the system's autonomous nervous system.
- Scheduled (daily digest, nightly arch-sweep, morning dossier generation, `com.lee.claude-autoupdate` at 8am daily) or persistent (monitors, watchers).
- Write results to logs, databases, or notification channels (Pushover for alerts to Lee's phone).

**Failure mode:** LaunchAgent fails and stops firing. Unlike a session crash (which Lee notices immediately), a LaunchAgent failure can be silent for days. The morning digest doesn't arrive — is it broken, or just nothing to report? Mitigation: LaunchAgent health monitoring, arch-sweep checks, and the operational principle that absence of output from a scheduled process is itself a signal that requires investigation.

#### DB I/O operations

Active reads and writes to the 6 SQLite databases (sessions.db, transcripts.db, contacts.db, ea_brain.db, memory-system databases, and others). Multiple components interact with databases concurrently — scripts, LaunchAgents, and Claude sessions may all access the same database.

**Key properties:**

- SQLite enforces serialization at the file level via its locking mechanism — concurrent writes are handled, though long-running write transactions may block other writers.
- Read operations are safe to run concurrently and do not block.
- The data stored in these databases is Knowledge/Experience (section 4). The databases as active I/O infrastructure are Capability/Machinery (section 5). The act of reading from or writing to them right now is Activity/Execution (this section). Three layers, one component — this is the dual-lens principle in action.

**Failure mode:** Corrupted database state from an interrupted write (process killed mid-transaction, disk full, system crash). Mitigation: the atomic operations pattern applies — use transactions, never leave partial writes. SQLite's WAL mode provides crash recovery for most scenarios.

#### Background tasks

Claude Code background tasks (spawned via the Task tool), background Bash commands, and any work delegated to run without blocking the active session.

**Key properties:**

- Run without direct observation — Lee and Claude are doing other work while these execute.
- Results arrive asynchronously and may require integration with ongoing work. The context in which the result arrives may have shifted from the context in which the task was dispatched.
- **Known bug (as of 2026-03-04):** `run_in_background: true` on Bash commands is avoided. Background Bash results can crash sessions after context compaction (orphaned `tool_result` blocks cause unrecoverable 400 errors). Use foreground Glob/Grep for searches. Use the Task tool (Agent) for parallel work.

**Failure mode:** Context compaction loss. A background task's result arrives after compaction has discarded the context that spawned it. The result cannot be integrated and may crash the session. Mitigation: save important background outputs to files early — before compaction can discard the originating context.

---

### Lifecycle

```
Capability/Machinery ──(Configuration)──► Execution context created
                                          │
                                          ├── CLAUDE.md loads (4 tiers)
                                          ├── Hooks arm (28 across 6 events)
                                          ├── MEMORY.md loads
                                          ├── Skills become available
                                          │
                                          ▼
                              Work executes (sessions, agents,
                              scripts, LaunchAgents, tool calls,
                              DB I/O, background tasks)
                                          │
                                          ▼
Activity/Execution ──(Capture)──────────► Knowledge/Experience
                                          │
                                          ├── Session transcript → session index
                                          ├── Insights/mistakes → MEMORY.md
                                          ├── Artifacts → files, databases
                                          ├── Observations → memory-ts
                                          │
                                          ▼
                              Activity ends (session closes,
                              process exits, work completes)
```

The key insight: the middle arrow (Capture) is the only one that is not automatic. Configuration fires reliably at session start — CLAUDE.md always loads. But Capture requires deliberate action. A session that produces brilliant insights but does not write them to MEMORY.md, does not document mistakes via `memory-ts`, and does not update task notes — that session's value evaporates when it ends. The lifecycle is Configure → Execute → **Capture (if deliberate)** → End.

---

### Foreground vs background

This distinction is operationally significant but not architecturally load-bearing. Both foreground sessions and background LaunchAgent processes are Activity/Execution — they occupy the same sub-layer. But the operational differences affect failure modes, recovery, and capture reliability.

| Dimension | Foreground | Background |
|-----------|-----------|------------|
| Lee present? | Yes | No |
| Feedback loop | Immediate — Lee observes, redirects | Delayed — results appear later (or never) |
| Failure visibility | Immediate — Lee sees the error | May be silent — requires monitoring to detect |
| Capture mechanism | Active — Claude captures deliberately | Automatic — hooks, scripts, logging |
| Recovery on failure | Lee can redirect, retry, adjust | Requires monitoring infrastructure to detect and alert |
| Examples | Interactive Claude sessions, foreground agents | LaunchAgents, background Task agents, scheduled scripts |

Background Activity carries higher operational risk. Failures may be silent, capture depends on automated mechanisms rather than deliberate Claude action, and recovery requires monitoring infrastructure rather than human judgment. The system mitigates this with: LaunchAgent health monitoring, arch-sweep checks, Pushover alerting for critical failures, and the daily digest that surfaces anomalies.

---

### What Execution is not

**Not persistent.** Nothing stored here survives session end without the Capture arrow. If you are looking at something that persists between sessions, it is not Activity — it is Knowledge or Capability.

**Not authoritative.** What happens in Execution does not change Knowledge or Capability without feedback arrows (Capture → Adaptation → Codification, section 8). A session cannot modify the Bible. A session cannot change a hook's behavior permanently. A session can write to MEMORY.md (Capture) and can edit a hook file (which is a Capability change, not an Activity change — the edit persists because it modified Capability, not because Activity persisted).

**Not where the system's identity lives.** Identity — the system's beliefs, principles, and architectural model — lives in Knowledge/Principles. Activity expresses identity but does not define it.

**Not a design concern at the individual level.** This section models the *types* of execution (sessions, agents, scripts, LaunchAgents, tool calls, DB I/O, background tasks), not individual instances. Which specific Bash command is running right now is operational, not architectural. The architectural question is: what kinds of execution exist, how are they born, how do they fail, and how do they capture their results?

---

### Failure modes summary

| Failure | What happens | Detection | Mitigation |
|---------|-------------|-----------|------------|
| Session crash | Work-in-progress lost if uncaptured | Lee notices immediately | Capture frequently: MEMORY.md, task notes, git commits |
| Silent background failure | LaunchAgent or script fails without alerting anyone | Health monitoring, daily digest, arch-sweep | Pushover alerts, monitoring hooks, absence-of-output checks |
| Capture gap | Session completes successfully but writes nothing back to Experience | Pattern: same mistakes repeat across sessions; no session index entry | Deliberate capture habits; PostToolUse capture hooks; session-rename hook at session end |
| Partial state | Multi-step operation fails midway, leaving intermediate state | Manual inspection of files/database | Atomic operations (temp file + rename); transactions; checkpoint before multi-step sequences |
| Context compaction loss | Background task result arrives after compaction discards originating context | Unrecoverable session error (400) | Save background outputs to files before compaction risk; avoid `run_in_background: true` on Bash |
| Agent sequencing violation | Agents dispatched out of dependency order; downstream agent produces garbage from inadequate inputs | Output quality degradation; wasted tokens | Follow agent sequencing rules in LFI CLAUDE.md; Conductor verifies prerequisites before dispatch |
| Stale configuration | Session loads outdated CLAUDE.md or rules after a mid-session edit to those files | Behavior doesn't match recently updated rules | Restart session after Capability changes; edits to Machinery/Protocols take effect on next Configuration arrow |

---

### Drift detection

Activity is where drift becomes visible. Drift is the gap between what Knowledge/Capability says should happen and what Activity actually does. Three forms:

**Behavioral drift.** Claude's execution diverges from configured protocols. Example: the questioning protocol says "default to questioning — when in doubt, ask." If a session proceeds for 15 minutes on ambiguous requirements without asking a single clarifying question, that is behavioral drift. Detection: behavior audit (`comprehensive_analyzer.py`), correction-type tracking (1,231 frustration corrections in the last audit suggest persistent behavioral drift).

**Operational drift.** Background processes diverge from their intended schedule or output. Example: a LaunchAgent that should fire daily has been silently failing for a week. The daily digest that should surface anomalies has itself drifted. Detection: arch-sweep, LaunchAgent health checks, absence-of-output monitoring.

**Capture drift.** The Capture arrow weakens — sessions produce valuable work but capture less of it over time. Knowledge/Experience grows stale relative to what Activity has actually learned. Detection: session index gaps (sessions that ran but were not indexed), MEMORY.md staleness (last update date vs. session count since), repeated mistakes that should have been captured after the first occurrence.

When drift is detected, the correction path is: detect in Activity → capture the observation (Capture arrow to Experience) → adjust governance (Adaptation arrow to Protocols) → if systemic, codify (Codification arrow to Principles). Drift is never fixed by changing Activity directly — Activity is ephemeral. You fix drift by changing the layers that configure Activity.

---

🔖 LEE: Everything here is ephemeral. The only lasting effect of Activity is what Capture sends back. When you notice a pattern in execution — an insight, a mistake, a better approach — capture it immediately via `memory-ts add` or `/qq-add-memory`. Ephemeral becomes permanent only through deliberate capture. The Capture arrow is the only path from "what happened today" to "what the system knows." The biggest risk in Activity is not failure — failures are loud and recoverable. The biggest risk is the capture gap: sessions that succeed, produce value, and teach nothing because nobody wrote it down.

---

## Section 8: The arrows — inter-layer flows

**Load when:** Tracing how something propagates through the system. Understanding feedback loops. Diagnosing why behavior doesn't match stated architecture. Planning a change that crosses layer boundaries. Debugging "why didn't this improvement stick?" problems.

---

### Why flows matter as much as containers

A static box diagram of Knowledge / Capability / Activity is a snapshot of the system's structure. It tells you what exists and where it lives. But it tells you nothing about how the system behaves over time — how a mistake in a Tuesday session becomes a hook that fires on Thursday, or how an article Lee reads in January becomes a Bible principle that governs all work in March. The arrows are the system's behavior over time. Structure without flow is a museum exhibit — accurate but inert. Flow without structure is chaos — active but uncontrolled. The six arrows define how the system learns, adapts, and maintains coherence across the three layers.

An earlier model of this system had "Feedback" as a sub-layer of an "Operation" layer — a box that collected experience and sprayed it upward. That was a category error. Feedback is a flow, not a stock. The box metaphor suggests a location where experience accumulates before being broadcast, like a mailroom sorting observations into envelopes. The arrow metaphor reveals what's actually happening: three distinct feedback loops, each with different timescale, consistency requirement, and authority level. Capture writes observations to Experience. Adaptation tunes protocols when patterns emerge. Codification — rarely — elevates a mature protocol to a principle. These are different operations with different stakes, not stages in a pipeline.

Activity doesn't spray to all layers simultaneously. The feedback chain is sequential and gated. Experience accumulates via Capture, then tunes protocols when patterns emerge via Adaptation, then — rarely — becomes a principle when evidence is overwhelming via Codification. Three distinct loops, three distinct authority levels. Fast loop: Configuration fires at session start, Execution produces work, Capture writes observations, Adaptation tunes protocols (minutes to weeks). Slow loop: Adaptation matures protocols, Codification elevates proven ones to principles, Constraint propagates principles to Capability, Configuration delivers them to new sessions (months to quarters). Emergency bypass: Escalation moves a live insight directly to Principles, skipping the entire accumulation chain (immediate, but rare and human-triggered).

Each arrow has a consistency requirement that matches its blast radius. Capture is eventually consistent — asynchronous, may be incomplete, loses one session's learnings at worst. Adaptation is causally consistent — protocol changes must trace to specific evidence, because an untraceable protocol change is technical debt. Codification is strongly consistent — new principles constrain everything downstream via Constraint, so a false principle corrupts the entire system. This gradient exists because damage scales with proximity to Principles: a broken Capture loses observations; a broken Codification introduces a false principle that constrains all future work across all sessions.

Authority follows the same gradient. Capture is automatic — low authority, happens without human decision, because the cost of over-capturing is noise while the cost of under-capturing is amnesia. Adaptation is low-medium authority — Claude can propose protocol changes and execute minor ones, but significant changes need Lee's confirmation. Codification is human-judgment-required — wrong principles are expensive to fix and propagate through Constraint to every downstream component. Escalation is human-only — the most restricted arrow, because it bypasses the evidence accumulation that makes Principles trustworthy. The gradient protects Principles from noise: the closer an arrow gets to Principles, the more authority it requires to fire.

---

### Arrow 1: Constraint — Knowledge/Principles to Capability

**Direction:** Knowledge/Principles → Capability (both Machinery and Protocols)
**Type:** Downward governing arrow

**Mechanism:** Principles define the authority frame that makes Capability coherent. This is not enforcement — Principles don't fire checks or block operations. It's definition: Principles establish the constraints that CLAUDE.md files, rules files, agent definitions, hook behaviors, and skill behaviors must respect. The derivation relationship is the key: every component in Capability is downstream of Principles. The Bible's 14 rules establish what Machinery can be built and what Protocols can require. When a CLAUDE.md file is written, it derives from Bible principles. When a rules file is created, it implements specific Bible rules. When a hook is authored, it enforces a specific principle (traced via the enforcement traceability matrix at Bible section 3.6). Changes in Principles cascade — a Bible update requires tracing every derived copy and updating it.

The enforcement traceability matrix (Bible section 3.6) makes Constraint explicit. Each of the 14 principles maps to specific enforcement mechanisms:
- Principle 1.1 (Orchestrate) → `delegation-check.py` hook, `delegation-strike-tracker.py`
- Principle 1.4 (Simplicity) → `bloat-watcher.py` hook, `/church-size` crusade, code-quality.md Commandment IX
- Principle 1.12 (Observability) → deploy phase gate, `/church-observability` crusade

The matrix also reveals enforcement gaps: principles 1.6 (config-driven) and 1.11 (actionable metrics) have no automated enforcement. These gaps are documented in Bible section 9 as known debt.

**Timescale:** Always active — Constraint is not a periodic event but a standing relationship. However, the downstream effect (derived copies updating after a Bible change) happens at the cadence of Bible updates. Since `/qq-bible-add` can fire on any session, the practical cadence is potentially daily. The propagation delay — time between Bible update and all derived copies reflecting the change — is the window of vulnerability.

**Consistency model:** Strong consistency required. Derived copies MUST accurately reflect canonical sources. A CLAUDE.md that summarizes a Bible rule incorrectly is not just a documentation problem — it produces inconsistent behavior. Sessions where CLAUDE.md is consulted will behave differently from sessions where the Bible is consulted directly. The 4-tier CLAUDE.md hierarchy (Global → Rules → Project → Operations) amplifies this risk: a principle may be accurately stated at the Global tier but incorrectly summarized at the Project tier.

**Authority required:** Implicit. Principles don't actively enforce; they define the frame. Enforcement happens at the Protocols level (hooks, rules files) which are derived from Principles. The authority is structural, not operational.

**Concrete examples:**
1. Bible rule 1 ("Orchestrate, don't execute") → CLAUDE.md "Primary directive" section → `~/.claude/rules/build-bible.md` critical rule 1 → `delegation-check.py` hook → every session starts with Conductor mindset and is actively monitored for solo execution
2. Bible verification requirement (principle 1.2 + 1.14) → `verification-protocols.md` reference doc → `~/.claude/rules/steelman.md` auto-loaded rule → every plan receives adversarial review
3. Bible 14-rule set → `~/.claude/rules/build-bible.md` → auto-loaded every session as a compressed reference → sessions can consult the full Bible for depth but always have the summary active
4. Architectural decision (SQLite for all databases, 6 stores) → constrains all agent database designs → constrains `db-backup.sh` backup strategy → constrains dashboard data access patterns

**Failure mode: Drift.** The most dangerous silent failure in the system. Bible changes but derived copies don't update. Sessions running CLAUDE.md see one behavior; sessions consulting Bible directly see another. The system behaves inconsistently across sessions. Because the 4-tier CLAUDE.md hierarchy has ~1,185 lines across multiple files, drift can accumulate undetected. The Bible's own section 9 documents "CLAUDE.md hierarchy broken (8 major duplications, 6 contradictions)" as a CRITICAL debt item — evidence that drift is not theoretical but active.

**How to detect failure:** Currently, no automated mechanism detects derived-copy drift. Detection is manual: load a Bible section for a principle, then check its CLAUDE.md implementation for accuracy. Discrepancy = drift. The arch-sweep concept (section 9 of this Atlas) would automate this by comparing canonical sources against derived copies.

**Recovery:** Update all derived copies. Use Bible section 3.6 (enforcement traceability matrix) to identify which files derive from which principles. Update each. Version-bump the Bible if the update represents a structural change. The enforcement chain to trace:
```
Bible rule change → CLAUDE.md update → rules file update → hook behavior update → skill behavior update
```
Missing any step in this chain leaves a derived copy drifted.

---

### Arrow 2: Configuration — Capability to Activity/Execution

**Direction:** Capability (Machinery + Protocols) → Activity/Execution
**Type:** Session instantiation arrow

**Mechanism:** Every execution context — Claude Code session, running agent, LaunchAgent process — is born by loading the Configuration that Capability provides. At session start, the system boot sequence fires:

1. Global CLAUDE.md (`~/.claude/CLAUDE.md`) — universal behaviors, Conductor mindset, all protocols
2. Global rules files (`~/.claude/rules/`) — `code-quality.md`, `Build Bible.md`, `steelman.md`, `voice.md`, `integrations.md` — each auto-loads
3. Project CLAUDE.md (e.g., `~/CC/Work/LFI/CLAUDE.md`) — project-specific instructions
4. Project rules files — any rules in the project's `.claude/rules/` directory
5. Operations CLAUDE.md (if present) — operation-level overrides
6. Auto-memory (`~/.claude/projects/[id]/memory/MEMORY.md`) — project memory for this session
7. SessionStart hooks fire — `session-context.py` (smart context loading), `inbox-monitor.py` (file hygiene check), `memory-ts session-start.ts` (memory system initialization)

After boot: hooks arm (register for their trigger events across 6 event types: PreToolUse, PostToolUse, SessionStart, UserPromptSubmit, PreCompact, SessionEnd). Skills become invocable but are NOT loaded — they load on demand when invoked via `/slash-command` or Skill tool. The accumulated state of Capability is what each execution inherits.

Configuration is not gradual or negotiable. It happens at session instantiation. What loads is what governs. There is no mid-session Configuration update — if a rules file changes after session start, that session continues with its loaded version. The next session gets the updated version.

**Timescale:** Per-session, immediate. Configuration fires once at session start and produces a fixed execution context.

**Consistency model:** Immediate within a session. What's loaded at session start is what governs that session. There is no lag between "rules file updated" and "new session uses it." However, existing sessions see their loaded configuration, not updates made after session start. This creates a window where two concurrent sessions can have different configurations — one loaded before a rules update, one after.

**Authority required:** Automatic. No human decision required for Configuration to happen. It is the structural consequence of Capability existing and a session starting.

**Concrete examples:**
1. `~/.claude/CLAUDE.md` loads → Conductor mindset is active → "Before ANY work, ask: Is this a single atomic action?" governs session behavior
2. `~/.claude/rules/steelman.md` auto-loads → every plan gets adversarial review → critique, steelman, survive cycle is mandatory
3. SessionStart hooks arm → `session-context.py` surfaces project brain, recent topics, operations health → `inbox-monitor.py` warns if `_ Inbox/` exceeds 5 files
4. PreToolUse hooks arm → `delegation-check.py` monitors for solo execution → `bloat-watcher.py` watches for files approaching 500 lines → `questioning-nudge.py` reminds to question before acting
5. `/qq-bible-add` skill becomes invocable → Lee can trigger Bible knowledge ingestion at any point during the session

**Failure mode: Stale configuration** (CLAUDE.md references outdated principles, rules file not updated after Bible change) or **broken hook** (hook registers but fails to fire, or fires incorrectly). Either produces a session that thinks it's correctly configured but isn't.

A subtler failure: **configuration overload**. The system loads ~1,185 lines of CLAUDE.md across 4 tiers plus auto-loaded rules files. If instruction volume exceeds the model's effective attention window, later instructions may be deprioritized. The Bible documents this as "CLAUDE.md hierarchy broken (8 major duplications, 6 contradictions)" — a configuration that is both too large and internally inconsistent.

**How to detect failure:** Session behavior doesn't match expected. A planning task completes without steelman review → `steelman.md` not loaded or instructions lost in volume. A quality violation occurs without hook intervention → quality hook broken or not armed. A delegation violation goes undetected → `delegation-check.py` not firing.

**Recovery:** Restart session (reloads all CLAUDE.md and rules). For broken hooks: `/qq-restart` reloads all configuration. For persistent issues: inspect hook code directly — check `_ Operations/hooks/` for the relevant script. For stale configuration: trace back to Constraint arrow and verify derived copies match canonical source.

---

### Arrow 3: Capture — Activity/Execution to Knowledge/Experience

**Direction:** Activity/Execution → Knowledge/Experience
**Type:** Learning arrow

**Mechanism:** Capture is the only path from "what happened in a session" to "what the system knows." Without Capture, Activity is a black hole — work happens and disappears. Multiple capture mechanisms operate at different automation levels:

**Automatic capture** (fires without human intervention):
- SessionEnd hooks: `auto-rename-session.py` generates intelligent session name; `session_topic_capture.py` writes final topic + triggers session index update in `sessions.db` (2,900+ entries); `session-state-capture.py` generates `_state.md` for next-session continuity; `session-memory-consolidation.py` extracts memories and creates `memory-ts` entries; `stats.py` logs hook system statistics
- PreCompact hooks: `periodic-rename.py` captures session name before context loss; `session_topic_capture.py` captures topic; `memory-ts curation.ts` runs memory curation
- UserPromptSubmit hooks: `session-exchange-counter.py` tracks exchanges; `session_topic_capture.py` captures topic every 10 exchanges; `project-detector.py` detects project mentions

**Claude-initiated capture** (Claude writes during session):
- MEMORY.md updates — persistent project memory written during session
- `memory-ts add --type mistake --intent high` — mistake documentation with structured tags
- File outputs that become permanent records (task notes, decisions.md, open-questions.md)

**Lee-initiated capture** (human decides significance):
- `/qq-done` end-of-session ritual — summary, file cleanup, rename, index
- Direct MEMORY.md edits with elevated priority
- `learnings-queue.md` entries for later review
- Decision journal entries for complex choices

The session index (`sessions.db`) is the backbone of Capture. Every session produces an entry: session ID, name, topics, timestamps, exchange count. The `sessions` CLI provides search (`sessions "keyword"`), context retrieval (`sessions context <id> "term"`), and analytics (`sessions analytics --week`). This is how past work becomes findable.

**Timescale:** Minutes for automatic captures (hook-driven, fires at session milestones). Session-duration for Claude-initiated captures (can happen any time). Indefinite for Lee-initiated captures (Lee may capture days after a session, though this creates risk — see principle 1.10, document when fresh).

**Consistency model:** Eventually consistent. Captures happen asynchronously and may be incomplete. A session can end with no meaningful Capture at all if automatic mechanisms fail (the SessionEnd hook bottleneck — 8 hooks with 455s worst-case timeout — is documented as HIGH debt in Bible section 9) and no one initiates manual capture.

**Authority required:** Automatic (for system captures — very low threshold). Low (for Claude capturing to MEMORY.md — low friction). Human judgment (for significance filtering — what's worth capturing vs. noise).

**Concrete examples:**
1. Session completes → `session_topic_capture.py` fires → topic extracted → `sessions.db` updated → searchable via `sessions "keyword"` or `/qq-search`
2. Mistake occurs → `memory-ts add --type mistake --content "Forgot to check derived copies after Bible update | Assumed CLAUDE.md was current | Added to verification checklist" --intent high --tags system,drift,process,medium` → mistake database updated → future sessions load prevention context
3. Project insight captured → MEMORY.md updated with structured entry → next session loads this context automatically via Configuration arrow
4. Complex task completes → task notes written (`context.md`, `decisions.md`, `open-questions.md`) → future sessions can reconstruct decision rationale
5. LaunchAgent runs → writes completion status to `~/Library/Logs/lfi/` → included in daily operations health check

**Failure mode: Capture gap** — session completes, work happens, nothing is written back to Experience. Future sessions have no record of what happened. Patterns repeat. Mistakes recur. The same debugging approach gets retried. The same wrong approach gets attempted. Capture gap is the most common failure in the system, because most capture mechanisms require either deliberate action or automatic hooks to fire correctly.

**How to detect failure:** Sessions repeat the same mistakes → check mistake documentation completeness. "We discussed this in a previous session" but `sessions` CLI finds no record → Capture gap. MEMORY.md doesn't reflect work done → Claude-initiated capture missed. `sessions.db` has a gap in session entries → automatic SessionEnd hooks failed.

**Recovery:** There is no recovery for a missed capture — the moment and its context are gone. Principle 1.10 (document when fresh) exists because of this: rationale is clearest at the moment of decision, and degrades irreversibly with time.

**Prevention:** The `/qq-done` ritual at session end helps but isn't sufficient. The most important captures should happen immediately when insights occur, not retroactively. The SessionEnd hook chain provides a safety net, but its reliability is limited by the hook bottleneck issue. Multiple redundant capture mechanisms (automatic + Claude-initiated + Lee-initiated) create defense in depth — if one layer fails, another may catch it.

🔖 LEE: The Capture arrow is the most fragile in the system. Knowledge doesn't accumulate if Capture doesn't fire. Every significant insight, mistake, or decision should be captured before the session ends — but ideally at the moment it occurs, not during a rushed end-of-session ritual. The SessionEnd hook bottleneck (455s worst-case, HIGH debt) makes the automatic safety net unreliable. Until that's resolved, deliberate mid-session capture is the primary mitigation.

---

### Arrow 4: Adaptation — Knowledge/Experience to Capability/Protocols

**Direction:** Knowledge/Experience → Capability/Protocols
**Type:** Improvement arrow

**Mechanism:** Accumulated experience in Knowledge/Experience tunes the behavioral governance in Capability/Protocols. Adaptation is how the system gets better without requiring human architectural decisions for every small improvement. It is the bridge between "we noticed this pattern" and "we now prevent or encourage it."

The Adaptation flow works through several concrete pathways:

1. **Mistake patterns → hooks.** A mistake documented three times (via `memory-ts --type mistake`) suggests a PreToolUse hook should prevent it. The hookify process turns a recurring observation into an active prevention mechanism. Example: repeated solo execution → `delegation-check.py` created with 3-strike enforcement.

2. **Learnings queue → rules files.** `learnings-queue.md` accumulates observations from sessions. The `/review-learnings` command (and Friday 5pm LaunchAgent trigger) walks through pending items. Confirmed learnings move to `universal-learnings.md`. Patterns across learnings suggest rules file updates. Example: repeated observation "voice skill must load before writing as Lee" → explicit rule in `~/.claude/rules/voice.md`.

3. **MEMORY.md patterns → CLAUDE.md updates.** When MEMORY.md repeatedly records the same preference or workflow pattern, it becomes a candidate for CLAUDE.md codification. Example: "adversarial debates are the DEFAULT" observed across multiple sessions → added to MEMORY.md → potential CLAUDE.md rule.

4. **Session index patterns → process improvements.** Patterns in `sessions.db` (2,900+ sessions) reveal workflow issues. Example: sessions frequently end without rename → `auto-rename-session.py` SessionEnd hook created. Sessions often have stale names → `periodic-rename.py` added to update name every 10 exchanges.

5. **Hook event logs → hook tuning.** `hook_events.jsonl` records every hook firing. Weekly review identifies hooks that never fire (candidates for removal per Bible section 8.3) and hooks that fire too aggressively (candidates for threshold tuning). Example: if `bloat-watcher.py` fires on every test file, the threshold or file-type filter needs adjustment.

**Timescale:** Days to weeks. Experience must accumulate before patterns emerge. Patterns must be confirmed before Protocol changes. Bible section 8.1 specifies the update process: observation across 3+ sessions → proposal with evidence → steelman review → 2-week advisory trial → measurement → promotion. Rushing Adaptation produces unstable protocols.

**Consistency model:** Causal consistency. Protocol changes must be traceable to specific Experience evidence. "We added `delegation-check.py` because sessions X, Y, and Z showed solo execution costing hours of conductor context." Untraceable Protocol changes are technical debt — they exist without justification and resist pruning because no one knows if they're still needed.

**Authority required:** Low-medium. Claude can propose Protocol updates (new rule, adjusted hook behavior, new skill) and execute them for minor changes. Significant Protocol changes require Lee's confirmation. Creating new hooks requires Lee approval — hooks are high-risk changes because they fire automatically on every session (Bible section on hook risk levels rates hooks as "critical").

**Concrete examples:**
1. Mistake pattern: repeated failure to verify work before claiming completion → `verification-before-completion` skill installed from obra/superpowers → `~/.claude/rules/steelman.md` includes verification as mandatory post-work step
2. Session index pattern: sessions often unnamed or poorly named → three rename hooks created (`periodic-rename.py`, `preview-rename-at-10.py`, `auto-rename-session.py`) → now documented as debt because three hooks do overlapping work (Bible section 9: "Duplicate rename logic")
3. Learnings queue review: "1-shot prompt test" observed as useful diagnostic across 5+ sessions → elevated to principle 1.4 diagnostic in Bible → derived to CLAUDE.md
4. Hook event analysis: `questioning-nudge.py` fires on every tool use including information-gathering → updated to skip info-gathering tools and resume sessions (reducing noise)

**Failure mode: Rigidity trap** — rules and hooks pile up without pruning, each one addressing a specific past mistake, until the system is so governed it can barely move. The protocols become bureaucracy. The current system has 28 hooks across 6 event types; the SessionEnd chain alone has 8 hooks with a 455-second worst-case timeout. Each hook was a reasonable Adaptation at the time — but their accumulated weight creates friction.

Bible section 8.3 provides deletion criteria to counteract rigidity: not referenced in 6 months, contradicted by evidence, superseded by a better pattern, or failed its trial period. But deletion is harder than addition — removing a rule feels risky ("what if the problem it prevented comes back?") while adding a rule feels safe ("we're preventing future mistakes").

**How to detect failure:** Sessions feel overly restricted — Claude spends more time satisfying protocol requirements than doing useful work. Hooks fire on legitimate work and block it. Rules files reference contexts that no longer exist. The exchange counter climbs but output doesn't — overhead is consuming capacity. Weekly hook event review shows hooks that fire frequently but never catch real violations (false positive rate too high).

**Recovery:** Protocol audit — review each rule, hook, and skill for current relevance. Remove or relax rules that no longer serve. Consolidate overlapping mechanisms (the three rename hooks should become one, per Bible section 9). Version-bump affected files. The `/review-learnings` process should explicitly include "what should we STOP doing" alongside "what should we START doing."

---

### Arrow 5: Codification — Capability/Protocols to Knowledge/Principles

**Direction:** Capability/Protocols → Knowledge/Principles
**Type:** The judgment arrow

**Mechanism:** Codification is the highest-stakes arrow in the system. It is the path from "this is how we've been doing it" to "this is how we should always do it." A protocol that has proven itself through repeated use, across multiple contexts, under scrutiny, earns elevation to a Principle. The process has deliberate friction because false codification — promoting a context-specific pattern to a universal principle — is expensive and hard to reverse.

`/qq-bible-add` is the primary mechanism for Codification. This slash command lives at `~/.claude/commands/qq-bible-add.md` and implements a structured ingestion pipeline:

1. **Input:** External knowledge (article URL, repo, insight) or internal observation
2. **Mapping:** Content mapped against existing Bible sections — what already exists, what's new, what conflicts
3. **Classification:** Each proposed addition classified as ADDITIVE (extends existing), UPGRADE (replaces existing with better version), NOVEL (addresses a gap), or REDUNDANT (already covered)
4. **Steelman review:** Is this actually universal, or is it context-specific? Is this proven, or premature? Does the steelman survive adversarial critique?
5. **Proposal:** Presented to Lee with evidence, classification, and recommended placement
6. **Integration:** After Lee approves — Bible updated, version bumped, derived copies updated via Constraint arrow

Bible section 8.1 specifies the full 7-step update process: Observation (3+ sessions) → Proposal (specific rule with enforcement and evidence) → Steelman → Trial (2 weeks advisory) → Measurement → Promotion → Documentation. Bible section 8.3 specifies what qualifies: used successfully in 3+ projects, confidence >0.8, survived steelman, has enforcement mechanism, human approved.

The maturation timeline matters. A pattern observed once isn't a principle. Confirmed across dozens of sessions, multiple contexts, adversarial review — that's a candidate. The Bible's current 14 principles and 19 patterns all trace to multi-project evidence:
- Principle 1.4 (Simplicity) traces to P2P's 93% code reduction (36,552 → 2,440 lines)
- Pattern 2.16 (Adversarial teams) traces to Memeta's QA swarm (40 agents, 2 HIGH blockers found pre-build)
- Anti-pattern 6.1 (The 49-day research agent) traces to P2P's failed research automation

**Timescale:** Months to quarters. Significant maturation required. Quick Codification is almost always premature Codification.

**Consistency model:** Strong. New principles constrain everything downstream via the Constraint arrow. A poorly-codified principle cascades: Bible → CLAUDE.md → rules files → hooks → every future session. The blast radius is system-wide. This is why the process has so much deliberate friction.

**Authority required:** Human judgment required. Lee must approve. No autonomous Codification. Claude can propose, assemble evidence, run the steelman, and prepare the integration — but the final gate is human. This gate exists because false codification is a serious and hard-to-reverse failure.

**Concrete examples:**
1. External article on agent orchestration → `/qq-bible-add [URL]` → classified as ADDITIVE to pattern 2.16 (adversarial teams) → steelman passes → Lee approves → Bible version bumped → CLAUDE.md updated → all future sessions inherit the improvement
2. Repeated session pattern across 20+ sessions: "orchestration vs. execution confusion" → retrospective write-up → evidence assembled → proposed as anti-pattern 6.3 (Solo execution) → steelman confirms → approved → Bible section 6 updated → `delegation-check.py` hook created as enforcement
3. Protocol from rules file: "no files > 500 lines" → confirmed effective across 50+ sessions → elevated to Critical Rule 4 in Bible → `bloat-watcher.py` hook created → `/church-size` crusade created → enforcement traceability matrix updated
4. obra/superpowers repo ingested via `/qq-bible-add` → verification-before-completion pattern mapped → classified as UPGRADE to principle 1.14 → steelman + adversarial review → integrated as v1.2.1

**Failure mode: False codification** — premature principle added before sufficient evidence, or context-specific rule elevated to universal principle. A false principle constrains future work unnecessarily and may produce worse outcomes in new contexts. It's hard to undo because removing a principle requires the same full Codification process (evidence that the principle is wrong, steelman that the removal is justified, Lee's approval). The asymmetry between adding and removing principles means false codifications accumulate.

**How to detect failure:** Bible principle produces confusion or counterproductive behavior in new contexts → may be false codification. Principle was added with limited evidence → verify evidence base. Principle references a specific project but claims universal applicability → scope check needed. Hooks enforcing the principle fire frequently but the "violations" are legitimate work → principle may be too broad.

**Recovery:** Codification of a removal or scoping. Requires the same full process: evidence, steelman, Lee approval. Bible section 8.3 specifies deletion criteria: contradicted by evidence, not referenced in 6 months, superseded by a better pattern, failed trial. Expensive — this is precisely why the gate exists on the way in.

🔖 LEE: `/qq-bible-add` is the most important slash command in the system. It's the deliberate, structured path from observation to codified principle. Use it whenever you encounter external knowledge — articles, repos, insights, conference talks — that might strengthen the Bible. The steelman and review process filters false positives. Don't hoard observations or let them sit in memory indefinitely. Run them through the process. The alternative is that good insights stay trapped in Experience and never become Principles.

---

### Arrow 6: Escalation — Activity/Execution to Knowledge/Principles (bypass)

**Direction:** Activity/Execution → Knowledge/Principles (bypassing Experience, Protocols, and the normal accumulation chain)
**Type:** Emergency bypass — "horizontal gene transfer"

**Mechanism:** When a live session produces an insight so significant it can't wait for the normal Capture → Adaptation → Codification chain, Escalation moves it directly from Activity to Principles in one step. Lee, during a live session, recognizes that a fundamental principle is wrong, missing, or needs immediate correction. He edits the Bible directly or instructs Claude to do so. This bypasses Experience accumulation entirely — there is no evidence trail, no 3-session confirmation, no 2-week trial, no learnings queue. Lee's judgment substitutes for the entire maturation process.

The "horizontal gene transfer" analogy is precise. Normal evolutionary change (Capture → Adaptation → Codification) is vertical gene transfer — slow, tested, reliable. Escalation is horizontal gene transfer — fast, untested by the normal selection process, potentially transformative or destabilizing. In biology, horizontal gene transfer is how bacteria acquire antibiotic resistance overnight. In this system, Escalation is how a session-level realization becomes a system-level principle overnight.

**Timescale:** Immediate. The fastest arrow in the system.

**Consistency model:** Human judgment is the consistency check. Lee's judgment substitutes for the normal evidence accumulation and steelman process. This works when Lee's judgment is well-calibrated (he's seen enough of the system to know what's fundamental). It fails when the insight feels more urgent than it actually is — session recency bias can make a single observation feel like a principle.

**Authority required:** Human only. Claude cannot trigger Escalation autonomously. This is the most restricted arrow in the system. The restriction exists because Escalation bypasses every safeguard that protects Principles from noise.

**Concrete examples:**
1. Session exposes that a Bible rule produces the opposite of its intent → Lee recognizes the rule is actively harmful → immediate Bible edit → Constraint arrow cascades the fix → next session no longer governed by the bad rule
2. The architectural debate that produced the 2-2-1 model (Knowledge / Capability / Activity) → fundamental gap in the system's self-understanding → immediate Atlas creation → new governing document for architectural coherence
3. Critical security insight during a live session — a hook behavior creates an unintended exposure → immediate Bible security section update → Constraint arrow propagates → hooks updated
4. A live session reveals that the "Feedback" sub-layer model was a category error (feedback is a flow, not a stock) → immediate architectural correction → Atlas section 8 written to capture the correct model

**Failure mode: Over-use** — Escalation becomes the default path for any insight, bypassing the evidence accumulation that makes Principles trustworthy. Principles become reactive (reflecting whatever the most recent session produced) rather than deliberate (reflecting well-tested patterns across many sessions). The Bible loses architectural coherence and reads like a collection of session notes rather than distilled wisdom.

**How to detect failure:** Bible update frequency from live sessions exceeds once per week → probable over-use of Escalation. Principles added via Escalation don't survive their first encounter with a novel context → evidence base was too thin. The Bible contains principles that trace to a single session rather than multiple projects → Escalation may have been used where Capture → Adaptation → Codification was appropriate.

**Prevention:** Default to the normal chain. Capture the insight to memory or learnings queue. Let it accumulate evidence. Let it survive the steelman. Reserve Escalation for genuine doctrinal emergencies — cases where waiting for the normal process would cause active harm in the intervening sessions. The test: "If this insight sits in the learnings queue for two weeks while accumulating evidence, will that delay cause real damage?" If yes, Escalate. If no, Capture.

---

### Arrow interaction analysis

#### The three feedback loops

**Fast loop** (days to weeks):
```
Execution → Capture → Adaptation → (back to Execution via Configuration)
```
- Activity produces work → Capture writes observations to Experience → Adaptation tunes Protocols → Configuration delivers updated Protocols to next session
- Authority: automatic to medium
- Consistency: eventual to causal
- Example: mistake occurs in session → `memory-ts add --type mistake` → pattern confirmed across 3 sessions → hook added to prevent recurrence → next session armed with new hook

**Slow loop** (months to quarters):
```
Adaptation → Codification → Constraint → Configuration → (back to Execution)
```
- Protocol matures through repeated use → Codified as Principle via `/qq-bible-add` → Principle constrains Capability via Constraint → Configuration delivers to next session
- Authority: human judgment required
- Consistency: strong
- Example: rules file rule ("no files > 500 lines") → confirmed effective across 50+ sessions → elevated to Bible principle 1.4 → CLAUDE.md updated → all sessions governed → `bloat-watcher.py` hook enforces

**Emergency bypass** (immediate, rare):
```
Execution → Escalation → (Constraint → Configuration)
```
- Live session insight → direct Principle update → cascades via Constraint to Capability → Configuration delivers to next session
- Authority: human only
- Consistency: human judgment replaces process
- Example: fundamental architecture insight → immediate Bible update → derived copies updated → all future sessions see new principle

#### The consistency gradient

| Arrow | Consistency | Blast radius of failure | Why this level |
|-------|------------|------------------------|----------------|
| Capture | Eventually consistent | Loses one session's learnings | Async, may be incomplete, cost of over-capture is low |
| Adaptation | Causally consistent | Bad protocol affects future sessions until pruned | Changes must trace to evidence to enable pruning |
| Configuration | Immediate | Session gets wrong configuration | What loads is what governs — no lag, no negotiation |
| Constraint | Strongly consistent | Derived copies diverge from canonical source | Drift produces system-wide inconsistency |
| Codification | Strongly consistent | False principle constrains all future work | Blast radius is everything downstream of Principles |
| Escalation | Human judgment | Same as Codification but without evidence base | Manual consistency check replaces process safeguards |

#### The authority gradient

| Arrow | Authority | Why this gate exists |
|-------|----------|---------------------|
| Configuration | Automatic | Structural consequence of session start — no decision needed |
| Capture | Automatic to low | Low friction capture prevents amnesia; over-capture filtered later |
| Adaptation | Low to medium | Evidence-based, but significant changes need sanity check |
| Codification | Human judgment | System-wide blast radius — wrong principles are expensive to fix |
| Escalation | Human only | Bypasses all safeguards — restricted to genuine emergencies |

#### Arrow failure cascade

Arrows don't fail independently. A failure in one arrow often causes or masks failures in others:

- **Capture failure → Adaptation failure.** If sessions don't write observations, there's nothing for Adaptation to work with. Protocols stagnate not because they're perfect, but because the system can't see its own problems.
- **Constraint failure → Configuration failure.** If derived copies drift from Principles (Constraint breaks), Configuration delivers stale or wrong instructions (Configuration delivers wrong content).
- **Adaptation excess → Codification excess.** If protocols accumulate without pruning (rigidity trap), the pool of "mature protocols" grows, creating pressure to codify more. False codification risk rises.
- **Escalation over-use → Constraint overload.** Every Escalation creates a Constraint cascade (update all derived copies). Frequent Escalations mean frequent cascades, increasing the chance of missed updates (drift).

---

## Section 9: Cross-cutting concerns

**Load when:** Auditing system health. Planning maintenance cadence. Diagnosing "why doesn't behavior match architecture?" problems. Building new monitoring or verification mechanisms. Understanding how the system maintains itself over time.

---

### What can't be contained in one layer

The three-layer model handles WHAT and WHERE. Cross-cutting concerns handle HOW the system maintains itself over time. These concerns span multiple layers or govern the system's overall health — they can't be attributed to Knowledge, Capability, or Activity alone because they operate across all three. Without them, the architecture is an ideal that progressively diverges from reality. The map looks correct, but the territory has moved.

These concerns are the meta-architecture: the processes that keep the map accurate, the gaps known, and the system healthy. They are maintenance functions, not features. They don't add new capabilities — they prevent existing capabilities from decaying.

---

### Consistency maintenance

**The core problem:** Capability/Machinery contains derived copies of Knowledge/Principles content. CLAUDE.md summarizes Bible rules. Rules files implement Bible rules. Agent behaviors reflect architectural decisions. Hook behaviors enforce specific principles. When canonical sources change, derived copies must update. This obligation is permanent and pervasive — it exists as long as derived copies exist, and derived copies exist because the system needs compressed, context-appropriate versions of Principles at different layers.

**Current mechanism:** Manual tracing after `/qq-bible-add`. When a Bible change is made, the integrator (Claude or Lee) must trace the enforcement chain (Bible section 3.6) and update derived copies. The enforcement traceability matrix maps each of 14 principles to its enforcement mechanisms — but tracing from principle to derived copy to update is manual and error-prone.

The update chain for a single Bible change:
```
Bible principle updated
  → Identify affected CLAUDE.md tiers (Global? Project? Operations?)
  → Update each CLAUDE.md file
  → Identify affected rules files in ~/.claude/rules/
  → Update each rules file
  → Identify affected hook behaviors in _ Operations/hooks/
  → Update hook scripts if enforcement logic changed
  → Identify affected skill behaviors
  → Update skill SKILL.md files if relevant
  → Verify no derived copy contradicts the new principle
```

**Gap:** No automated mechanism exists to detect when derived copies have drifted from their canonical source. The Bible's own section 9 documents "CLAUDE.md hierarchy broken (8 major duplications, 6 contradictions)" as CRITICAL debt — proof that drift is already present in the system today. The system relies on Lee noticing behavioral inconsistencies, which means drift is only detected when it produces visible symptoms (incorrect behavior in a session).

**Proposed: consistency sweep.** A systematic audit that reads the canonical source (Bible) and checks each derived copy for alignment. Similar to `/church` crusades but focused on doc accuracy rather than code quality. Would read each Bible principle, find every file that derives from it (via the enforcement traceability matrix), and verify the derived content matches the canonical content. Discrepancies flagged in a report.

---

### Drift detection

The highest-priority architectural gap in the system today.

**What drift means:** Practice (what actually happens in Execution) diverges from Knowledge doctrine (what the Architecture says should happen). This is different from consistency maintenance, which checks documents against documents. Drift detection checks behavior against documents. Examples:
- The steelman protocol says plans get adversarial review, but sessions consistently show plans executed without review
- The delegation protocol says 80% delegation rate, but actual rate has declined to 60% without anyone noticing
- A hook is supposed to fire on every Write operation, but a configuration change silently disabled it

**Current state:** Partial mechanisms exist. The behavior audit system (`comprehensive_analyzer.py`) analyzes 1,433+ sessions for delegation rate, response compliance, and file hygiene. Hook stats are surfaced at SessionEnd. But no mechanism compares session behavior against stated protocols in rules files and CLAUDE.md at a granular level. The system measures what it can count (delegation rate, hook fires) but not what it should observe (protocol compliance, principle adherence).

**Conceptual approach — the god agent (System 3*):** Beer's Viable System Model includes System 3* — the audit channel. While System 3 manages operational resources (Capability in this model), System 3* periodically samples operational activity to verify that what's claimed to happen actually happens. It's orthogonal to the normal management channel — it bypasses the usual reporting chain and checks reality directly.

In this system, System 3* would be an autonomous agent that:
1. Samples session transcripts from `sessions.db` (2,900+ sessions)
2. Parses behavioral patterns against stated protocols in rules files and CLAUDE.md
3. Reports discrepancies with specifics: "The steelman protocol says plans get adversarial review. In the last 30 sessions, 12 involved planning. Of those, 4 (33%) show no evidence of steelman review."
4. Provides trend lines: "Delegation rate was 72% last month, 65% this month. Declining."

**Status:** Partial — `com.lfi.behavior-audit` LaunchAgent (weekly, Monday 8am) runs `comprehensive_analyzer.py`, saving `comprehensive_report.md` + `comprehensive_data.json` to `behavior-audit/`. Measures delegation rate, correction patterns, response compliance. Full System 3* (automated protocol comparison against CLAUDE.md/rules files) remains a gap.

**What it would require:**
- Session transcript analysis capability (the `sessions` CLI + `sessions.db` FTS5 provides the raw data)
- Protocol extraction from rules files and CLAUDE.md (parse stated behaviors into checkable assertions)
- Pattern matching between transcripts and assertions (did the session exhibit the stated behavior?)
- Discrepancy reporting with specifics and evidence

🔖 LEE: Drift detection is the most important unsolved problem in this system. Without it, the architecture becomes a fiction — an idealized map of a territory that has moved. The Bible, the Atlas, CLAUDE.md, the rules files — all of them describe what SHOULD happen. None of them verify what DOES happen, beyond coarse metrics like delegation rate. The god agent / System 3* concept is the solution. When you're ready to build it, this is where it lives architecturally: a cross-cutting concern that samples Activity and compares against Knowledge.

---

### System boot sequence

What loads at session start, in order, and why it matters architecturally. This is the Configuration arrow (section 8, Arrow 2) made concrete.

| Order | What loads | Source | Architectural role |
|------:|-----------|--------|-------------------|
| 1 | Global CLAUDE.md | `~/.claude/CLAUDE.md` | Universal behaviors: Conductor mindset, questioning protocol, verification protocol, mistake documentation, response format, security audit |
| 2 | Global rules files | `~/.claude/rules/*.md` | Auto-loaded behavioral governance: `code-quality.md`, `Build Bible.md`, `steelman.md`, `voice.md`, `integrations.md` |
| 3 | Project CLAUDE.md | `~/CC/Work/LFI/CLAUDE.md` (or project-specific) | Project-level: agent routing, sequencing rules, integration quick-reference, skills, EA rituals |
| 4 | Project rules files | Project `.claude/rules/` directory | Project-level rules: `Build Bible.md` (project copy) |
| 5 | Operations CLAUDE.md | If present in operations directory | Operation-level overrides for specific subsystems |
| 6 | Auto-memory | `~/.claude/projects/[id]/memory/MEMORY.md` | Persistent project memory: preferences, patterns, context from previous sessions |
| 7 | SessionStart hooks | `session-context.py`, `inbox-monitor.py`, `memory-ts session-start.ts` | Smart context loading, file hygiene check, memory system initialization |

After boot:
- Hooks arm across 6 event types (PreToolUse, PostToolUse, SessionStart, UserPromptSubmit, PreCompact, SessionEnd)
- Skills become invocable but do NOT load until invoked — on-demand loading keeps boot weight low
- The Configuration arrow completes — this session's execution context is fixed

**Session state vs. session memory:** Each session starts fresh from the Configuration above. There is no persisted "previous session state" at the session level — that would be Execution layer memory, which is ephemeral by design. Continuity comes from Knowledge/Experience: MEMORY.md (persistent project memory), `sessions.db` (searchable session index), `_state.md` (project state captured at session end), HANDOFF.md (explicit handoff document if created). A new session must reconstruct context from these Knowledge/Experience artifacts — it does not inherit the previous session's Execution state.

**The ~1,185-line problem:** The 4-tier CLAUDE.md hierarchy totals approximately 1,185 lines across all files, plus auto-loaded rules files. This is deliberate — the scale serves a 47-agent, 28-hook system that operates as a full team. But it creates a real tension: instruction volume may exceed the model's effective attention window. The Bible documents this as CRITICAL debt: "8 major duplications, 6 contradictions." Deduplication and hierarchy clarification are the mitigation path — not reducing the system's scope, but eliminating redundancy so every line earns its place.

---

### Health monitoring

The system monitors its own health through several mechanisms, each operating at a different cadence and granularity.

**Dashboards (4 visual + 1 API):**

| Dashboard | Port | What it shows | Pattern |
|-----------|-----:|--------------|---------|
| Memory dashboard | 8766 | Memory system status, learnings, review schedule | Static HTML rebuild |
| LFI operations dashboard | 8701 | System health, script status, hook stats | Static HTML rebuild |
| Total recall dashboard | 7860 | Memory system v1 / Memeta (Gradio-based) | Gradio server |
| Outreach dashboard | — | Outreach pipeline, channel status | Static HTML rebuild |
| Memory API server | 8765 | memory-ts backend (not visual) | Live Bun process |

All visual dashboards use static HTML rebuilt on data change — no server-side state, no APIs, no framework (Bible pattern reference: static HTML dashboard approach). The memory API server is the exception: a live Bun process serving the memory-ts system.

**Backup verification:**
- Daily at 2am: `cc-backup.sh` backs up full `~/` directory + `~/.claude/` config + LaunchAgent plists → Google Drive
- Daily at 3am: `db-backup.sh` backs up all SQLite databases via `sqlite3 .backup` (corruption-safe) → Google Drive `_databases/` subfolder with 7-day rotation
- Failure → Pushover alert to Lee's devices
- Verification: check `~/Library/Logs/lfi/db-backup.log` for last run status
- Gap: no automated verification of backup integrity beyond `sqlite3 .backup` success/failure code (Bible principle 2.17: untested backups are decorative)

**LaunchAgent monitoring:**
- `unified-health-monitor` (continuous service, RunAtLoad + KeepAlive) monitors the 47 LaunchAgents
- Auto-restart on crash for critical services
- Gap: no alerting when a LaunchAgent stops firing silently (it's registered but not executing). The agent is monitored for crash, but not for silent failure.

**Pushover alerts:** Critical failure notifications delivered to Lee's devices. Configured for: backup failure, monitoring script failure, threshold breaches, forgotten conversations (5-minute timer when Claude is waiting for input via `start_delayed_poke.sh`).

**Hook statistics:** `stats.py` fires at SessionEnd and surfaces hook system stats for the last 7 days. Provides: hook fire counts, failure counts, timing data. Enables weekly hook event review for Adaptation decisions.

**Behavior audit:** `comprehensive_analyzer.py` analyzes 1,433+ sessions for delegation rate (currently 72%, target 80%), response summary compliance, session documentation rate, and file organization hygiene. Run on-demand, results feed weekly metrics review.

**arch-sweep (active):** A nightly LaunchAgent (`com.lfi.arch-sweep`, 2:00am daily) that reads the Atlas and Directory, verifies every file path exists, checks component counts against reality (agents, hooks, databases), and flags drift between the Atlas description and filesystem reality. Results in `/tmp/arch-sweep-report.md` (full) and `/tmp/arch-sweep-alerts.md` (drift only). This is the primary anti-rot mechanism for the Atlas itself — without it, the Atlas degrades into fiction as the system evolves. Script: `hooks/arch-sweep.py`.

🔖 LEE: The arch-sweep is BUILT and running. It runs at 2am. On its first verified clean run: 0 alerts, 4/4 key docs found, counts matched, 86/86 referenced paths valid. The arch-sweep is now the enforcement mechanism for Atlas accuracy — when it flags drift, fix the Atlas.

---

### Pace-layer table

Functional classification (Knowledge / Capability / Activity) is primary — it determines where something lives and how it's governed. Temporal classification (how frequently something changes) is secondary — it informs maintenance cadence, not layer membership. A component's pace layer tells you how often to audit it and how much scrutiny a change deserves.

| Component | Layer | Sub-layer | Change frequency | Why it changes |
|-----------|-------|-----------|------------------|----------------|
| Bible critical rules (14) | Knowledge | Principles | Weeks-months | High-evidence codification required; faster than traditional orgs because AI reduces validation cost |
| Bible patterns (19) | Knowledge | Principles | Weeks-months | Pattern confirmation across multiple sessions; still needs cross-context evidence |
| Architectural decisions | Knowledge | Principles | Months | Deliberate, well-considered, hard to reverse |
| Atlas (this document) | Knowledge | Principles | Months | Structural model changes are rare; content accuracy verified by arch-sweep |
| CLAUDE.md (global) | Capability | Machinery | Weeks-months | Derived from Bible; updates when Bible updates via Constraint arrow |
| CLAUDE.md (project) | Capability | Machinery | Weeks | Project-specific instructions evolve with project needs |
| Rules files (5+) | Capability | Protocols | Days-weeks | Adaptation arrow fires more frequently than Codification |
| Agent definitions (47) | Capability | Machinery | Weeks-months | Stable once battle-tested; changes driven by new requirements |
| Hooks (28) | Capability | Protocols | Days-weeks | Adaptation-driven; hookify makes creation easy but accumulation is a risk |
| Skills (100+) | Capability | Machinery | Days-weeks | Easy to add and refine; on-demand loading means low cost of expansion |
| Slash commands (65+) | Capability | Machinery | Weeks | User-invoked workflows; stable once proven |
| LaunchAgents (47) | Capability | Machinery | Weeks-months | Infrastructure; changes are deliberate and tested |
| Python scripts (317) | Capability | Machinery | Days-weeks | Actively developed operational code |
| MEMORY.md | Knowledge | Experience | Daily-weekly | Frequent session captures; groomed periodically |
| Memeta / memory-ts | Knowledge | Experience | Daily | FSRS review cycle; continuous capture |
| Session index (`sessions.db`) | Knowledge | Experience | Daily | Every session adds entries; 2,900+ and growing |
| Learnings queue | Knowledge | Experience | Weekly | Accumulated observations awaiting review |
| Decision journals / task notes | Knowledge | Experience | Per-task | Written during complex decisions per principle 1.10 |
| Databases (6 SQLite) | Knowledge | Experience | Daily | Continuous writes from session activity, meeting intelligence, contacts |

---

### Synchronization cadence

The maintenance schedule for the entire system. Each cadence corresponds to specific arrows and mechanisms.

| Cadence | What happens | Arrow served | Mechanism |
|---------|-------------|-------------|-----------|
| **Per-session** | Configuration loads (CLAUDE.md, rules, hooks arm) | Configuration | Automatic boot sequence |
| **Per-session** | Capture fires (session index, topic capture, memory curation) | Capture | SessionEnd hooks, PreCompact hooks |
| **Daily** | Mid-session captures (mistake docs, MEMORY.md updates, task notes) | Capture | Claude-initiated + Lee-initiated |
| **Daily** | arch-sweep runs (Atlas accuracy check, file path verification, component counts) | Cross-cutting | LaunchAgent (forthcoming) |
| **Daily** | Backup verification (CC codebase at 2am, databases at 3am) | Cross-cutting | LaunchAgent + Pushover on failure |
| **Continuous** | Adaptation fires on every meaningful signal (correction, insight, hook alert) | Adaptation | In-session, immediate |
| **Weekly** | Scheduled adaptation review (learnings queue, MEMORY.md grooming) — backstop for signals that didn't trigger immediate response | Adaptation | Manual + `/review-learnings` + Friday 5pm LaunchAgent |
| **Weekly** | Behavior audit (delegation rate, corrections, compliance) | Cross-cutting | `com.lfi.behavior-audit` (Monday 8am) — check `behavior-audit/comprehensive_report.md` |
| **Monthly** | Bible review (evidence citations, cross-references, structural integrity, version bump) | Codification | Manual + `/qq-bible-add` batch review |
| **Monthly** | Atlas structural review (does the model still match reality?) | Cross-cutting | Manual + arch-sweep results |
| **On-demand** | Post-project retrospective (extract learnings, update debt table) | Capture + Adaptation | Manual after project completion |
| **On-demand** | `/qq-bible-add` (external knowledge ingestion) | Codification | Lee-triggered |
| **On-demand** | Escalation (doctrinal emergency, immediate Bible edit) | Escalation | Lee-only, reserved for genuine emergencies |

🔖 LEE: This cadence is the maintenance heartbeat. Daily captures are the minimum viable cadence — miss them and the system stops learning. Weekly is the primary active cadence: adaptation reviews, behavior audit, hook review. Weekly is the fastest the slow layer should change; it's also the slowest the fast layer should tolerate before feeding back. Monthly is where structural deliberation happens — Bible reviews, Atlas accuracy audits. The most common failure mode is skipping the weekly cadences when things feel smooth. Smooth-running systems that skip maintenance accumulate quiet debt.

---

### Known architectural gaps

A summary of gaps identified across section 8 (arrows) and this section (cross-cutting concerns), ordered by impact. These are distinct from the Bible's debt inventory (Bible section 9), which focuses on implementation debt. These gaps are structural — they represent missing pieces of the architecture itself.

| Gap | Impact | Where it appears | Mitigation path |
|-----|--------|-----------------|-----------------|
| Partial drift detection (System 3*) | Behavioral drift detected at coarse level (delegation rate, corrections); protocol-level compliance not yet automated | Section 9: Drift detection | `com.lfi.behavior-audit` active; full protocol comparison remains a gap |
| No consistency sweep | Derived copies drift from canonical sources undetected | Section 9: Consistency maintenance | Build automated comparison of Bible → CLAUDE.md → rules files |
| No arch-sweep | Atlas accuracy degrades as system evolves | Section 9: Health monitoring | Build nightly LaunchAgent verifying file paths and component counts |
| Capture fragility | SessionEnd hook bottleneck (455s worst-case) | Section 8: Arrow 3 | Move heavy hooks to async LaunchAgents (Bible section 9 proposed fix) |
| CLAUDE.md hierarchy drift | 8 duplications, 6 contradictions across 4 tiers | Section 8: Arrow 1 | Deduplicate, resolve contradictions, enforce single-source per instruction |
| Adaptation without pruning | 28 hooks, 3 overlapping rename hooks | Section 8: Arrow 4 | Periodic protocol audit; consolidate overlapping mechanisms |
| No Escalation frequency monitoring | Over-use of Escalation undetected | Section 8: Arrow 6 | Track Bible edits from live sessions; alert if frequency exceeds weekly |

---

## Section 10: Document index

Every document in the system, organized by architectural model layer. Load this section when you need to find a document.

The system has 35+ governing documents, 317 scripts, 100+ skills, 60 LaunchAgents, and 6 databases. Without a master index, the system is unknowable -- components that solve real problems exist but cannot be found. A conductor who cannot find the right document at the right time will re-derive decisions that were already settled, miss enforcement mechanisms that already exist, or build machinery that duplicates what is already running. This section closes that gap.

This index is organized by the architectural model layers defined in section 1. Each document appears once, in its home layer. For documents that serve multiple purposes, they appear in the layer that represents their primary role -- the canonical source determines home per the authority-record test (section 1). The index captures: name, file path, purpose (one line), maintenance status, and size estimate. For maintenance status: Active (regularly updated), Stable (updated when system changes), Archived (historical record, not updated).

---

### Knowledge / Principles

Documents that define HOW and WHY -- the governing beliefs, principles, patterns, and architectural decisions.

| Name | File path | Purpose | Status | Size |
|------|-----------|---------|--------|------|
| Build Bible | `~/CC/Work/_ Infrastructure/Build Bible.md` | HOW WE BUILD -- 14 principles, 19 patterns, playbook, agent system, anti-patterns, operations reference, evolution protocol, debt inventory, credits | Active | ~1,922 lines |
| Atlas (this document) | `~/CC/Work/_ Infrastructure/atlas.md` | HOW IT ALL FITS -- architectural model, arrows, precedence, flows, document index, evolution protocol | Active | ~2,500 lines |
| Universal learnings | `~/CC/Work/LFI/_ System/universal-learnings.md` | Cross-project insights promoted from learnings queues after 3+ confirmations | Active | ~476 lines |
| Cross-project insights | `~/CC/Work/LFI/_ System/cross-project-insights.md` | Synthesized patterns observed across multiple client engagements | Active | ~403 lines |
| Quality system overview | `~/CC/Work/LFI/_ System/quality-system-overview.md` | Unified view of questioning + verification + steelman protocols | Stable | ~396 lines |
| System architecture | `~/CC/Work/LFI/_ System/system-architecture.md` | Structural description of the 5-layer system model and component relationships | Stable | ~501 lines |

### Knowledge / Experience

Documents that record WHAT HAPPENED -- session history, decision records, learnings, mistakes, changelogs.

| Name | File path | Purpose | Status | Size |
|------|-----------|---------|--------|------|
| Directory (system inventory) | `~/CC/Work/LFI/_ System/directory.md` | WHAT EXISTS -- component catalog with rationale column for every skill, hook, agent, database, LaunchAgent, integration | Active | ~589 lines |
| Bible changelog | `~/CC/Work/LFI/_ System/Build bible changelog.md` | Every Bible change -- what, why, source, evaluation status | Active | ~27 lines (growing) |
| Debt inventory | (Bible section 9) | 15 known technical debt items with severity, impact, and mitigation status | Active | ~Bible section |
| Session index (database) | `~/CC/Work/LFI/_ Operations/session-index/sessions.db` | 2,900+ Claude Code sessions indexed with FTS5 full-text search -- searchable by content, client, date, tags | Active | ~65MB |
| Session index scripts | `~/CC/Work/LFI/_ Operations/session-index/` | `session_indexer.py`, `session_search.py`, `session_analyzer.py`, `session_topic_capture.py` | Active | 4 scripts |
| Learnings queue | `~/CC/Work/LFI/_ System/learnings-queue.md` | Pending learnings awaiting review and promotion to universal learnings | Active | ~22 lines (variable) |
| Work log | `~/CC/Work/LFI/_ System/work-log.md` | Session-level work summaries for significant sessions | Active | ~421 lines |
| Auto-memory (global) | `~/.claude/projects/-Users-lee-CC/memory/MEMORY.md` | Project-specific session memory -- preferences, patterns, project context | Active | Growing |
| Memeta memory system | `~/CC/Work/_ Infrastructure/memory-system-v1/` | FSRS-6 spaced repetition system -- 102 features, 2,037 tests, Python 3.11+ | Active | Codebase |
| Transcripts database | `~/CC/Work/LFI/_ Operations/meeting-intelligence/transcripts.db` | 1,900+ meeting transcripts with FTS5 search -- core intelligence layer | Active | Database |
| Contacts database | `~/CC/Work/LFI/_ Operations/contacts.db` | 2,000+ unified contacts + outreach log -- single source of truth for lead/contact data | Active | Database |
| EA Brain database | `~/CC/Work/LFI/_ Operations/ea_brain/ea_brain.db` | Commitments, personal intel, relationship scores -- Penny's intelligence layer | Active | Database |
| Roadmap | `~/CC/Work/LFI/_ System/roadmap.md` | System evolution roadmap and planned improvements | Active | ~116 lines |
| Mistake documentation examples | `~/CC/Work/LFI/_ System/mistake-documentation-examples.md` | Reference examples for the mistake documentation protocol | Stable | Reference |
| Mistake system index | `~/CC/Work/LFI/_ System/MISTAKE-SYSTEM-INDEX.md` | Master index for all mistake system components | Stable | Reference |

### Capability / Machinery

Documents that define WHAT RUNS -- configuration, agent definitions, database schemas, scripts, templates.

**CLAUDE.md files (4-tier hierarchy):**

| Name | File path | Scope | Status | Size |
|------|-----------|-------|--------|------|
| Global CLAUDE.md | `~/.claude/CLAUDE.md` | All projects -- conductor mindset, planning mandate, questioning/verification/steelman protocols, response format, security audit | Active | ~181 lines |
| Project CLAUDE.md (CC) | `~/CC/.claude/rules/build-bible.md` | CC project scope -- Build Bible summary (auto-loaded) | Stable | ~50 lines |
| LFI CLAUDE.md | `~/CC/Work/LFI/CLAUDE.md` | LFI project -- agent sequencing, routing table, integration quick-reference, skills, EA rituals, session maintenance, file organization | Active | ~289 lines |
| Operations CLAUDE.md | `~/CC/Work/LFI/_ Operations/CLAUDE.md` | Operations scope -- integration API docs, script inventory, folder structure, background services, EA Brain, session index, outreach system | Active | ~642 lines |
| Autonomous CLAUDE.md | `~/CC/Work/LFI/_ Operations/autonomous/CLAUDE.md` | Autonomous operations scope | Stable | ~31 lines |

**Rules files (`~/.claude/rules/` -- auto-loaded every session):**

| File | Purpose |
|------|---------|
| `code-quality.md` | Code quality protocol -- 10 commandments, bestiary hunting, TDD mandate, good code checklist |
| `Build Bible.md` | Build Bible summary -- 14 rules, 8 anti-patterns, architecture quick facts |
| `steelman.md` | Steelman protocol -- adversarial plan review, upgrade signals, integration with planning flow |
| `voice.md` | Voice enforcement -- mandatory personal-voice skill gate for any writing as Lee |
| `integrations.md` | Tool-specific behaviors -- Apple Reminders, Python venvs, Todoist session links, background task workaround |
| `system-visibility.md` | Memory and Bible visibility prefixes -- active operations must be prefixed with visibility markers |

**Agent definitions (`~/CC/Work/LFI/_ System/agents/`):**

| Component | File(s) | Count |
|-----------|---------|-------|
| Agent reference (master roster) | `AGENT-REFERENCE.md` | 1 file, ~188 lines |
| Hierarchical teams (6 domains) | `{pm,copywriter,visual-designer,ux-architect,content-strategist,market-researcher}-{director,senior,junior}.md` | 18 agents |
| Development team | `dev-{director,senior-frontend,senior-backend,senior-devops,junior-python,junior-typescript,junior-shell,junior-css,junior-docker,junior-api,junior-react,junior-database}.md` | 12 agents |
| Specialist agents (multi-file) | `conductor/`, `emma-stratton.md`, `brand-strategist/`, `voice-analyst/`, `pm-louder-than-ten/`, `messaging-architect/`, `seo-geo-strategist/`, `learning-curator/`, `webflow-developer/` | 9 agents |
| Specialist agents (single-file) | `google-docs-editor.md`, `cfo.md`, `project-manager.md`, `sales-crm.md`, `client-success.md`, `sysadmin.md`, `creative-provocateur.md`, `stats-viewer.md`, `qa-specialist.md` | 9 agents |
| Simulation agents | `target-client-{industrial-mfr-late50s,service-biz-mid40s,construction-mid30s}.md` | 3 agents |
| Agent templates | `_templates/` -- copywriting-formulas, messaging-framework, pm-specs, design-specs, ux-specs, content-specs, research-templates, dev-patterns-*, dev-code-review | ~15 files |

**Skills (100+ across 3 locations):**

| Location | Count | Examples |
|----------|-------|---------|
| Personal skills (`~/.claude/skills/`) | ~33 | apple-reminders, penny-prep, penny-followup, cold-email-sequence-generator, sequential-thinking, python-best-practices |
| External/installed skills (`~/CC/.agents/skills/`) | ~25 | copywriting, seo-audit, marketing-psychology, gemini, personal-voice, verification-before-completion |
| Local system skills (`_ System/skills/`) | 2 | qq-docs-fix, qq-project-file-cleanup |
| Marketing skills (coreyhaines31) | ~20 | page-cro, signup-flow-cro, ab-test-setup, pricing-strategy, launch-strategy |

**Slash commands (`~/.claude/commands/`):**

| Category | Count | Key commands |
|----------|-------|-------------|
| Session management (`qq-*`) | 11 | `/qq-done`, `/qq-handoff`, `/qq-search`, `/qq-pause`, `/qq-codereview`, `/qq-agents`, `/qq-bible-add` |
| Code quality crusade (`church-*`) | 15 | `/church` (orchestrator), `/church-git`, `/church-test`, `/church-size`, `/church-arch`, `/church-type` |
| Meeting commands | 5 | `/meeting-search`, `/meeting-history`, `/meeting-people`, `/meeting-stats`, `/meeting-enrich-calendar` |
| Agent tier commands | 21 | `/emma`, `/conductor`, `/sales`, `/pm-{junior,senior,director}`, `/copywriter-{junior,senior,director}`, etc. |
| Other commands | 5 | `/verify`, `/qa-swarm`, `/handoff`, `/review-learnings`, `/gemini` |

**Hooks (`~/.claude/settings.json` -> `_ Operations/hooks/`):**

| Hook type | Count | Key hooks |
|-----------|-------|-----------|
| PreToolUse | 6 | `questioning-nudge.py`, `delegation-check.py`, `bloat-watcher.py`, `component-audit.py`, `gdocs-agent-reminder.sh`, `skill-usage-tracker.py` |
| PostToolUse | 1 | `poke/start_delayed_poke.sh` |
| SessionStart | 4 | `session-context.py`, `inbox-monitor.py`, `memory-ts session-start.ts`, echo confirmation |
| UserPromptSubmit | 7 | `poke/cancel_poke.sh`, `memory-ts user-prompt.ts`, `session-exchange-counter.py`, `preview-rename-at-10.py`, `periodic-rename.py`, `session_topic_capture.py`, `project-detector.py` |
| PreCompact | 3 | `periodic-rename.py`, `session_topic_capture.py`, `memory-ts curation.ts` |
| SessionEnd | 8 | `memory-ts curation.ts`, `auto-rename-session.py`, `session_topic_capture.py`, `session-state-capture.py`, `response-summary-check.py`, `stats.py`, `session-memory-consolidation.py`, `session_content_spotter.py` |

**Plugins (`~/.claude/settings.json`):**

| Plugin | Source | Purpose |
|--------|--------|---------|
| hookify | claude-plugins-official | Hook creation and management tooling |
| frontend-design | claude-plugins-official | Frontend design patterns and UI component assistance |
| orchestration | orchestration-marketplace | Workflow orchestration with custom `.flow` syntax |
| code-review | claude-plugins-official | Structured code review workflows |
| vercel | claude-plugins-official | Vercel deployment integration |
| dx | ykdojo | Developer experience enhancements |

**LaunchAgents (`~/Library/LaunchAgents/` -- ~47 total):**

| Category | Count | Key agents |
|----------|-------|-----------|
| Meeting and intelligence | 6 | `com.lfi.meeting-nudge` (5min), `com.lfi.daily-intelligence` (6:30am), `com.lfi.meeting-indexer`, `com.granola.checker`, `com.lfi.granola-api-sync`, `com.macwhisper.processor` |
| Notifications and rituals | 4 | `com.lfi.triage-nag` (9am), `com.lfi.eod-nag` (5pm), `com.lfi.daily-digest` (7am), `com.lfi.eod-capture` (5pm) |
| Pipeline and outreach | 5 | `com.lfi.pipeline-health` (M/W/F), `com.lfi.outreach-sync` (30min), `com.lfi.linkedin-staging` (weekly), `com.lfi.commitment-extractor` (daily), `com.lfi.crm-enricher` |
| Approval and execution | 1 | `com.lfi.poke-executor` (5min) |
| Session and learning | 5 | `com.lfi.session-indexer` (30min), `com.learning.pattern-analyzer` (Mon 7am), `com.learning.system-learner` (daily 8am), `com.lfi.learnings-review` (Fri 5pm), `com.lfi.weekly-improvement` (weekly) |
| Memory system | 2 | `com.lfi.memory-maintenance`, `com.lfi.memory-weekly-synthesis` |
| Dashboards and monitoring | 2 | `com.lfi.dashboard-fetcher` (5min), `com.lfi.unified-health-monitor` (continuous) |
| Backup and maintenance | 2 | `com.lfi.db-backup` (3am daily), `com.lfi.log-rotation` |

**Databases (all SQLite):**

| Database | Location | Records | Query interface |
|----------|----------|--------:|----------------|
| `transcripts.db` | `_ Operations/meeting-intelligence/` | 1,900+ | `transcript_intel.py`, SQL, FTS5 |
| `sessions.db` | `_ Operations/session-index/` | 2,900+ | `sessions` CLI, `session_search.py`, FTS5 |
| `contacts.db` | `_ Operations/` | 2,000+ | `contacts_db.py`, SQL |
| `ea_brain.db` | `_ Operations/ea_brain/` | -- | `ea_brain.surfacer`, commitment_tracker |
| `fsrs.db` | Memory system | -- | FSRS-6 module |
| `passive_income.db` | `_ Operations/passive-income/` | -- | SQL |

**Dashboards:**

| Dashboard | Port | Technology |
|-----------|-----:|-----------|
| Memory dashboard | 8766 | Static HTML |
| LFI operations dashboard | 8701 | Static HTML |
| Total recall (Memeta) | 7860 | Gradio |
| Memory API server | 8765 | Bun (backend, not visual) |

**Integrations:**

| Service | Client | Location |
|---------|--------|----------|
| Todoist | `lfi_integrations.todoist` | `_ Operations/lfi_integrations.py` |
| Gmail | `lfi_integrations.gmail` | `_ Operations/lfi_integrations.py` |
| Calendar | `lfi_integrations.calendar` | `_ Operations/lfi_integrations.py` |
| Notion | `lfi_integrations.notion` | `_ Operations/lfi_integrations.py` |
| Google Docs | MCP server | google-docs MCP |
| Pushover | `poke/send_poke_pushover.py` | `_ Operations/poke/` |
| 1Password | `op` CLI | System |
| Webflow | MCP server | webflow MCP |
| GitHub | `gh` CLI | System |
| Gemini | `/opt/homebrew/bin/gemini` | Homebrew |
| Memory-ts | Bun scripts | `/opt/homebrew/lib/node_modules/@rlabs-inc/memory/` |

**Templates (`~/CC/Work/LFI/_ System/templates/`):**

| Template | Purpose |
|----------|---------|
| Project template (directory) | Full project scaffold -- `project-brain.md`, `learnings-queue.md`, `.claude/` config, process-notes, messaging-framework, project-overview |
| `project-brain-template.md` | Standalone project brain template |
| `learnings-queue-template.md` | Learnings queue template for new projects |
| Task notes templates | `context.md`, `decisions.md`, `open-questions.md` -- for `/_notes/[task-name]/` directories |

### Capability / Protocols

Documents that define HOW TO ACT -- protocols, rules, workflows, reference material.

**Core protocols:**

| Name | File path | Purpose | Status | Size |
|------|-----------|---------|--------|------|
| Verification protocols | `~/CC/Work/LFI/_ System/verification-protocols.md` | Mandatory verification before claiming completion -- matrix, checklists, evidence requirements | Active | ~413 lines |
| Questioning protocol | `~/CC/Work/LFI/_ System/questioning-protocol.md` | Pre-work clarification protocol -- when to ask, how to ask, multiple-choice preference | Active | ~406 lines |
| Mistake documentation protocol | `~/CC/Work/LFI/_ System/mistake-documentation-protocol.md` | Mandatory capture protocol for mistakes -- format, routing, severity classification | Active | ~302 lines |
| File organization rules | `~/CC/Work/LFI/_ System/file-organization-rules.md` | Comprehensive decision tree and routing matrix for file placement | Active | ~533 lines |
| EA rituals | `~/CC/Work/LFI/_ System/ea-rituals.md` | Morning triage and end-of-day capture ritual definitions | Active | ~227 lines |
| Response format | `~/CC/Work/LFI/_ System/response-format.md` | Required response summary format specification | Stable | ~267 lines |

**Reference documents (`~/CC/Work/LFI/_ System/reference/`):**

| Name | File path | Purpose | Status | Size |
|------|-----------|---------|--------|------|
| Lee's voice reference | `~/CC/Work/LFI/_ System/reference/lees-voice.md` | Voice patterns, banned words, communication style for writing as Lee | Stable | ~207 lines |
| Learnings system | `~/CC/Work/LFI/_ System/reference/learnings-system.md` | How the two-level learning + promotion pipeline works | Stable | ~41 lines |
| Project lifecycle | `~/CC/Work/LFI/_ System/reference/project-lifecycle.md` | Phase definitions for client project work | Stable | ~100 lines |
| Maintenance retainers | `~/CC/Work/LFI/_ System/reference/maintenance-retainers.md` | All retainer tiers, base + AEO scope definitions | Stable | ~279 lines |
| Todoist workflow | `~/CC/Work/LFI/_ System/reference/todoist-workflow.md` | Todoist integration workflow patterns | Stable | ~51 lines |
| Maintenance messaging framework | `~/CC/Work/LFI/_ System/reference/maintenance-messaging-framework.md` | Messaging framework specific to maintenance retainer positioning | Stable | ~526 lines |

**Operational reference:**

| Name | File path | Purpose | Status | Size |
|------|-----------|---------|--------|------|
| Scheduling guidelines | `~/CC/Work/LFI/_ System/scheduling-guidelines.md` | Meeting scheduling preferences and constraints | Stable | ~29 lines |
| GitHub standards | `~/CC/Work/LFI/_ System/github-standards.md` | GitHub repo setup, READMEs, changelogs, open source packaging standards | Stable | ~154 lines |
| Penny dossier style | `~/CC/Work/LFI/_ Operations/ea_brain/PENNY-DOSSIER-STYLE.md` | Pre-meeting dossier format and content requirements | Stable | Reference |

**Messaging and voice system (`~/CC/Work/LFI/_ System/messaging-and-voice/`):**

| Name | File path | Purpose | Status | Size |
|------|-----------|---------|--------|------|
| Messaging framework | `messaging-and-voice/MESSAGING-FRAMEWORK.md` | LFI's core messaging framework | Stable | ~581 lines |
| Voice bank | `messaging-and-voice/VOICE-BANK.md` | Extracted voice patterns and examples | Stable | ~842 lines |
| Quick reference | `messaging-and-voice/QUICK-REFERENCE.md` | Condensed messaging/voice reference card | Stable | ~128 lines |
| Examples (subdirectory) | `messaging-and-voice/examples/` | `headlines.md`, `linkedin-posts.md`, `cold-emails.md` | Stable | 3 files |

**Mistake system support documents:**

| Name | File path | Purpose | Status |
|------|-----------|---------|--------|
| Mistake cheatsheet | `~/CC/Work/LFI/_ System/mistake-cheatsheet.md` | Quick-reference for mistake documentation format | Stable |
| Mistake-to-learning flow | `~/CC/Work/LFI/_ System/mistake-to-learning-flow.md` | How mistakes promote through the learning pipeline | Stable |
| Mistake system diagram | `~/CC/Work/LFI/_ System/mistake-system-diagram.md` | Visual representation of the mistake system | Stable |

**Content engine (`~/CC/Work/LFI/_ Operations/content-engine/`):**

| Script | Purpose |
|--------|---------|
| `voice-patterns.json` | Extracted voice patterns (4.6 MB source data) |
| `linkedin-generator.py` | LinkedIn post generation from voice patterns |
| `proof_points_extractor.py` | Mine testimonials from transcripts |
| `quote-bank-from-transcripts.md` | Marketing quotes extracted from meetings |

**Poke system (`~/CC/Work/LFI/_ Operations/poke/`):**

| Script | Purpose |
|--------|---------|
| `send_poke_pushover.py` | Push notifications to Lee's phone via Pushover |
| `start_delayed_poke.sh` | 5-minute delayed notification when Claude waits for input |
| `cancel_poke.sh` | Cancel pending notification when Lee responds |
| `poke_bridge.py` / `poke_executor.py` | Approval queue for notification-gated actions |

### Cross-cutting / System

Documents that span multiple layers or govern the system as a whole.

| Name | File path | Purpose | Status | Size |
|------|-----------|---------|--------|------|
| Atlas (this document) | `~/CC/Work/_ Infrastructure/atlas.md` | Master architectural document -- model, arrows, flows, index, evolution | Active | ~2,500 lines |
| System overview | `~/CC/Work/LFI/_ System/SYSTEM-OVERVIEW.md` | High-level system overview and orientation | Stable | Reference |
| System index | `~/CC/Work/LFI/_ System/SYSTEM-INDEX.md` | Index of all `_ System/` contents | Active | Reference |
| Settings file | `~/.claude/settings.json` | Master Claude Code configuration -- hooks, plugins, environment variables, permissions | Active | Config |
| Status line config | `~/.claude/statusline-dark.sh` | Custom status line showing context usage, model, session ID, topic | Stable | Config |
| Environment configuration | `~/.claude/settings.json` | `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS`, `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE`, `autoUpdatesChannel`, `defaultMode` | Active | In settings.json |

---

### Cross-references

Key relationships between documents that are not obvious from the layer tables alone.

**Bible -> CLAUDE.md (derived copy obligation):** The Bible is canonical for principles and patterns. CLAUDE.md at all tiers contains derived summaries of Bible content. When the Bible changes, affected CLAUDE.md files must update. See Bible section 3.6 (enforcement traceability matrix) to trace which CLAUDE.md sections derive from which Bible rules. The `~/.claude/rules/build-bible.md` file is explicitly a Bible summary and must stay synchronized.

**Atlas -> Bible and Directory:** The Atlas references both but duplicates neither. For HOW WE BUILD (principles, patterns, playbook), see the Bible. For WHAT EXISTS (detailed component specs with rationale), see the Directory (`directory.md`). The Atlas provides the architectural context -- WHERE things live, WHY they are there, HOW they connect.

**Directory -> all component docs:** The Directory is the master catalog. When a new component is created, it should appear in the Directory before it appears elsewhere. The Atlas section 10 tables are derived summaries of the Directory. If a discrepancy exists between this index and the Directory, the Directory is canonical for component details; the Atlas is canonical for layer placement.

**Session index -> all session transcripts:** The session index (`sessions.db`) contains metadata (name, date, tags, summary, git state). Full transcripts are stored separately and accessible via session ID. The `sessions` CLI tool queries the index; `/qq-search` is the slash command interface.

**Bible changelog -> Bible:** The changelog (`Build bible changelog.md`) is the evolution history of the Bible. Reading a Bible principle in context sometimes requires reading its changelog entry to understand why it was added or modified.

**Learnings queue -> universal learnings -> Bible:** The promotion pipeline from observation to codified principle. Learnings queue holds pending observations. After 3+ confirmations, they promote to universal learnings. During weekly Bible review (section 8.2), recurring universal learnings may become new principles or patterns. Each step is a separate document in a different layer.

**Mistake documentation -> learnings queue -> universal learnings:** Mistakes captured via `memory-ts --intent high` feed anti-pattern identification. Recurring mistakes can be named as anti-patterns and codified in the Bible (section 6).

**CLAUDE.md hierarchy -> rules files -> hooks:** The CLAUDE.md hierarchy defines behavioral expectations. Rules files (`~/.claude/rules/`) auto-load enforcement summaries every session. Hooks (`~/.claude/settings.json`) provide runtime enforcement. These three layers form a defense-in-depth stack: CLAUDE.md tells Claude what to do, rules remind Claude at session start, hooks catch violations during execution.

**Agent definitions -> agent templates -> routing table:** Agent `.md` files define behavior. Templates in `_templates/` provide reusable scaffolding for agent outputs. The routing table (Bible section 5.2, LFI CLAUDE.md) maps work types to agents. All three must stay synchronized when agents are added, modified, or archived.

---

## Section 11: Evolution protocol

The Atlas must evolve as the system evolves. New components are added, old ones deprecated, counts drift, paths change, and occasionally the architectural model itself needs refinement. A governing document that does not govern its own evolution rots -- it becomes an outdated description of a system that has moved on. The moment the Atlas stops reflecting reality, every Claude Code session that loads it inherits a wrong map.

The protocol has two modes, calibrated to the magnitude of the change. Structural changes to the model, arrows, or precedence require the same rigor as Bible structural changes (Bible section 8) -- adversarial review, steelman, human approval. Maintenance-level updates (new component, corrected path, updated count) use a lighter grooming pass. This prevents two failure modes: over-governance (requiring full review for a corrected file path) and under-governance (treating fundamental architectural changes as routine maintenance).

---

### Structural changes -- full evolution protocol

**What counts as structural:** Model changes (new layer, layer removed, layer renamed, fundamental reclassification of what belongs where), new arrow type, precedence hierarchy changes, new sub-layer, new or modified placement test, fundamental revision to existing sections (rewriting the model narrative, changing the arrow semantics).

**Process:**

1. **Trigger:** Architectural debate, fundamental component reclassification that existing tests cannot resolve, new relationship type discovered in practice that no existing arrow captures, or monthly structural review identifying model drift.
2. **Adversarial debate:** Minimum 7 personas from relevant domains. Real debate -- personas argue against each other, not parallel independent reviews. The debate must produce a concrete proposal with rationale.
3. **Steelman review** of the resulting proposal (per `~/.claude/rules/steelman.md`). Critique the proposal, defend against the critiques, fold surviving critiques into a revised proposal.
4. **Lee approval:** Required for all structural changes. Present the revised proposal with the steelman record so Lee can see what was considered and rejected.
5. **Update** the affected Atlas section(s). Integrate the change as if the Atlas was always written this way -- do not append.
6. **Update `~/.claude/rules/atlas.md`** (the auto-loaded session reference) if the section index changes.
7. **Version bump:** MINOR for new section or new arrow type. MAJOR for fundamental model change (new layer, layer removed, model redefinition).
8. **Update header:** `<!-- Atlas v[version] | Last updated: [date] -->`

**Authority:** Human approval required. Claude cannot make structural changes autonomously.

**Why this rigor:** The Atlas is a normative document -- it constrains how Claude understands the entire system. A wrong structural change produces wrong understanding, which produces wrong decisions across every session that loads it. The blast radius is the entire system.

### Grooming pass -- lightweight maintenance

**What counts as grooming:** New component added (register in appropriate section), file path changed, component count corrected, section reference updated, new agent or skill registered in section 10 tables, line count refreshed, arch-sweep discrepancy resolved, cross-reference added for a newly discovered relationship.

**Process:**

1. **Trigger:** `/qq-arch-add [description]`, arch-sweep discrepancy report, direct observation that Atlas is stale, or new component creation in any layer.
2. **Apply placement tests** (consult-vs-fire, authority-record, stability-as-property) from section 4 to determine correct layer and sub-layer.
3. **Write the entry** as if the component always existed in the document. Integrate, do not append to the bottom of a section. Find the right place in the existing structure.
4. **Check cross-references:** Does the new entry create relationships that need documenting in the cross-references subsection of section 10?
5. **Verify Directory:** Does `directory.md` also need updating? If the component is new, it should appear in the Directory first.
6. **Version bump:** PATCH.
7. **No steelman needed** for grooming passes.

**Authority:** Claude can execute grooming passes autonomously for count corrections, path corrections, and line count refreshes. New component registration: Claude executes without approval for Machinery components (agents, skills, commands, hooks, LaunchAgents, database entries). Lee confirmation required for Protocols components (new rules files, new protocol documents, new skills that change behavioral expectations) because these carry higher risk of unintended behavioral changes.

**The integration principle:** Grooming passes integrate new entries as if they were always part of the document. Do not add a "Recently added" section. Do not append to the bottom of a table. Find the correct position in the existing structure, insert the entry, and update any counts or cross-references affected by the addition.

### Version control

| Version component | What triggers it | Frequency |
|-------------------|-----------------|-----------|
| MAJOR (X.0.0) | Model itself changes -- new layer, layer removed, fundamental reclassification, arrow semantics redefined | Rare -- architectural paradigm shifts |
| MINOR (x.Y.0) | New section added, significant content expansion, new arrow type, new placement test | Occasional -- meaningful structural additions |
| PATCH (x.y.Z) | Grooming pass -- path corrections, count updates, new component registration, cross-reference additions | Frequent -- routine maintenance |

Version lives in the document header: `<!-- Atlas v1.0.0 | Last updated: 2026-03-04 -->`

### Review cadence

- **Monthly:** Structural review -- does the model still accurately describe the system? Are all arrows still accurate? Is the precedence hierarchy complete? Are the placement tests sufficient? Has any component drifted to a different layer?
- **On-demand:** Grooming pass -- triggered by `/qq-arch-add`, arch-sweep discrepancy report, or direct observation of staleness.
- **Target:** The Atlas should never be more than 30 days out of date on component counts and file paths. The arch-sweep enforces this by flagging discrepancies.

### Anti-rot mechanisms

The Atlas stays accurate through five reinforcing mechanisms:

1. **`~/.claude/rules/atlas.md`** -- auto-loaded section index every session. Claude always knows the Atlas exists and how to navigate it.
2. **`/qq-arch-add`** -- the primary contribution path. Makes updating the Atlas the natural next step after creating a component. Accepts a description, applies placement tests, writes the entry, checks cross-references.
3. **PostToolUse hook on Atlas edits** -- validates file paths referenced in the edit actually exist, checks entry format matches the section's table structure, confirms integration (not appending), asks whether the Directory also needs updating.
4. **Nightly arch-sweep LaunchAgent** -- reads the Atlas, verifies every file path exists, checks component counts against reality (counts agents on disk, counts hooks in settings.json, counts LaunchAgents in `~/Library/LaunchAgents/`), reports discrepancies in the daily digest email.
5. **Evolution protocol** (this section) -- defines when and how structural changes happen. Prevents both over-governance and under-governance by matching rigor to magnitude.

These mechanisms form a closed loop: the arch-sweep catches silent drift, `/qq-arch-add` closes the gap when new components appear, the PostToolUse hook validates edits as they happen, the rules file ensures Claude remembers the Atlas exists, and the evolution protocol governs how structural changes flow through review.

🔖 LEE: The Atlas is only useful if it is accurate. The anti-rot mechanisms above are the primary defense. The arch-sweep catches what drifts silently. The `/qq-arch-add` command closes the loop when you add a new component. The evolution protocol ensures the model stays correct when the system evolves. Treat Atlas updates as a first-class task, not an afterthought. If the Atlas and the Directory ever disagree, the Directory is canonical for component details, the Atlas is canonical for layer placement -- resolve the discrepancy, do not pick one and ignore the other.

---

*End of the Atlas.*
