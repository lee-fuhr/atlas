<!-- Atlas Framework v1.0.0 | The universal architectural model for AI-augmented systems -->
<!-- This is the framework half of the Atlas. For a worked example showing how one system implements it, see atlas-example.md -->

# Atlas framework — architectural model for AI-augmented systems

*Version 1.0.0*

---

## Table of contents

- [Section 0: How to use this document](#section-0-how-to-use-this-document)
  - [Section index](#section-index)
  - [Relationship to companion documents](#relationship-to-companion-documents)
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
  - [Drift detection](#drift-detection)
  - [System boot sequence](#system-boot-sequence)
  - [Health monitoring](#health-monitoring)
  - [Pace-layer table](#pace-layer-table)
  - [Synchronization cadence](#synchronization-cadence)
  - [Known architectural gaps](#known-architectural-gaps)
- [Section 11: Evolution protocol](#section-11-evolution-protocol)
  - [Structural changes — full evolution protocol](#structural-changes----full-evolution-protocol)
  - [Grooming pass — lightweight maintenance](#grooming-pass----lightweight-maintenance)
  - [Anti-rot mechanisms](#anti-rot-mechanisms)

---

## Section 0: How to use this document

### What this is

The Atlas is an architectural framework for AI-augmented systems — any practice where a human operator works with AI agents (Claude Code, similar tools) to run a business, manage projects, or build software. It answers one question: **how does it all fit together?** The model, every relationship, every flow, every precedence rule.

This document contains the **universal framework** — the KCA model (Knowledge / Capability / Activity), the 6 inter-layer arrows, the precedence hierarchy, the placement tests, and the cross-cutting concerns that keep any such system healthy. These concepts apply regardless of your specific tools, agent count, or domain.

For a worked example showing how one system implements this framework with specific components, file paths, and operational details, see the companion document `atlas-example.md` (sections 3-7 and 10).

### What this isn't

- **Not a build bible** — that answers HOW YOU BUILD (principles, patterns, playbook). See `build-bible.md` for a companion methodology framework.
- **Not a component catalog** — that answers WHAT EXISTS in your specific system (component inventory with rationale).
- **Not a design doc** — it doesn't argue for the architecture. It documents the architecture as a framework you can adopt and adapt.

### How to use

The Atlas is designed for section-by-section loading into AI sessions. The section index below tells you which section answers which question. You rarely need the full document — load 50-300 lines at a time based on what you're working on.

### Section index

| Section | Title | Load when... |
|---------|-------|-------------|
| 0 | How to use this document | You need to understand this document itself |
| 1 | The model | Placing a new component; answering "where does X live?"; understanding the architecture |
| 2 | Precedence & conflict resolution | Resolving a conflict between rules, protocols, or instructions |
| 8 | The arrows — inter-layer flows | Tracing how something propagates through the system; understanding feedback loops |
| 9 | Cross-cutting concerns | System health, consistency maintenance, session boot sequence, health monitoring |
| 11 | Evolution protocol | Making structural changes to the Atlas itself |

**Sections 3-7 and 10** cover implementation specifics (Knowledge/Principles, Knowledge/Experience, Capability/Machinery, Capability/Protocols, Activity/Execution, and the document index). These are in the companion `atlas-example.md` because they are system-specific — your implementation will have different components, tools, and file paths. Use the example as a template for documenting your own system.

### Relationship to companion documents

```
Atlas (this)              → HOW IT FITS TOGETHER (architecture, flows, precedence)
Build bible               → HOW YOU BUILD (principles, patterns, playbook)
System inventory          → WHAT EXISTS (component catalog, file paths, rationale)
```

The three documents form a triangle. The Atlas references the build bible by section number and the inventory by component name — it never restates their content. When you need principle details, load the build bible. When you need component details, load the inventory. When you need to understand relationships, flows, or placement, load the Atlas.

### Theoretical backing

The model is grounded in five compatible frameworks: Pace Layering (Brand) — slow layers govern fast ones; Viable System Model (Beer) — every viable system requires operations, management, and an audit channel; Panarchy (Holling) — systems cycle through growth, conservation, and creative destruction; Ashby's ultrastability — a system maintains viability by responding to perturbations through multiple feedback loops; near-decomposability (Simon) — complex systems are hierarchical, with high interaction within sub-systems and loose coupling between them. The 2-2-1 model (Knowledge / Capability / Activity) implements these principles as a concrete architecture. The 6 arrows implement the feedback loops that prevent the system from fossilizing.

### System at a glance

The full framework in three tables.

**The 3 companion documents (adapt to your system):**

| Document | Answers |
|----------|---------|
| Build bible | HOW YOU BUILD — methodology, principles, patterns, playbook |
| Atlas (this) | HOW IT ALL FITS — model, flows, precedence, evolution protocol |
| System inventory | WHAT EXISTS — component catalog with rationale |

**The 6 arrows (inter-layer flows):**

| Arrow | From | To | Timescale |
|-------|------|----|-----------|
| Constraint | Knowledge / Principles | Capability | Per-change (same session as principle update) |
| Configuration | Capability | Activity | Per-session |
| Capture | Activity | Knowledge / Experience | Minutes-hours |
| Adaptation | Knowledge / Experience | Capability / Protocols | Weekly |
| Codification | Capability / Protocols | Knowledge / Principles | Days-weeks (human judgment required) |
| Escalation | Activity | Knowledge / Principles | On demand (bypass, human-only) |

**Placement cheat sheet:**

| If it... | Layer | Sub-layer |
|----------|-------|-----------|
| Is consulted as reference, never fires automatically | Knowledge | Principles or Experience |
| Is normative / prescriptive (principles, decisions) | Knowledge | Principles |
| Is empirical / descriptive (records, history, catalog) | Knowledge | Experience |
| Fires or loads on a trigger / schedule | Capability | Machinery or Protocols |
| Is standing infrastructure (agents, scripts, DBs, config files) | Capability | Machinery |
| Is behavioral governance (hooks, skills, rules, sweeps) | Capability | Protocols |
| Is running right now, will be gone when done | Activity | Execution |

---

## Section 1: The model

### Why a model?

Without a shared architectural model, every question about the system — "where should this live?", "what takes precedence?", "how does X affect Y?" — requires first-principles reasoning from scratch. First-principles reasoning from scratch produces inconsistent answers. Session A places a new component in one location; session B places a similar component somewhere else; session C doesn't know which was right. Inconsistency compounds. Within months the system becomes a maze of ad-hoc decisions that no single session can reason about.

A model gives the AI agent a shared map. When the map is accurate, navigation is fast: "this is a behavioral governance mechanism, it belongs in Capability/Protocols" takes one lookup instead of ten minutes of deliberation. When the map is wrong, the Atlas updates and the map improves — but between updates, every session navigates the same way. Consistency across sessions is the primary value.

The core insight is that the three layers aren't arbitrary divisions — they correspond to fundamentally different *kinds* of things. Knowledge is what the system knows (consultable reference). Capability is what the system can do (standing apparatus that loads and fires). Activity is what the system is doing right now (ephemeral processes). These three kinds have different stability characteristics, different maintenance rhythms, and different authority levels. Conflating them produces confusion about where to put things and why. A rules file that "feels like knowledge" but actually loads and fires every session belongs in Capability, not Knowledge — and misplacing it means misunderstanding its maintenance needs, its authority level, and its failure modes.

The model also prevents a common failure mode: treating the system as flat. Without layers, everything is "just files" and the only organization principle is folder structure. With layers, you can ask: "does this new component change what the system *knows*, what the system *can do*, or what the system *is doing right now*?" That question routes placement, determines maintenance cadence, and predicts failure modes — all from a single classification.

### Three layers defined

#### Knowledge — what the system knows

**Definition:** Knowledge is the body of reference material the system consults but does not automatically execute. It is the system's memory and judgment, consulted when needed, never fired on a trigger.

**Kind:** Consultable reference. A session loads Knowledge when it needs guidance, context, or empirical data. Knowledge does not load itself.

**What distinguishes it:** Knowledge is *pulled*, never *pushed*. No Knowledge component activates automatically on a schedule, on a hook event, or at session start. If something activates automatically, it belongs in Capability even if its content reads like knowledge. Knowledge has the highest authority (Principles sub-layer) and the longest time horizon.

**Examples:** Build bible principles, architectural decisions, session memory files, spaced-repetition memory records, session history (when queried for reference), system inventory, past meeting transcripts (when consulted).

#### Capability — what the system is set up to do

**Definition:** Capability is the standing apparatus and behavioral governance that exists between sessions — ready to activate, configured to fire, waiting for a trigger. It is the system's infrastructure and its rules of engagement.

**Kind:** Standing apparatus + governance. Capability persists across sessions and activates when conditions are met (session start, hook event, user invocation, schedule).

**What distinguishes it:** Capability *fires*. Every component in Capability either loads automatically (config files, hooks, scheduled jobs) or fires when invoked (skills, scripts, slash commands). The test is: does it DO something when activated, or does it just SIT there waiting to be read? If it does something, it's Capability.

**Examples:** Config files that load every session, agent definitions (spawn when invoked), hooks (fire on events), skills (fire when loaded), scheduled jobs (fire on schedule), scripts (fire when called), databases (active I/O machinery), rules files (load and govern behavior).

#### Activity — what the system is doing right now

**Definition:** Activity is the set of live, ephemeral processes currently executing. It is the system in motion — born from Capability's configuration, dead when the process ends.

**Kind:** Ephemeral live processes. Activity has no persistence of its own — when a session ends, that Activity ceases to exist. Its artifacts (files written, memories captured, databases updated) persist in Knowledge or Capability, but the Activity itself is gone.

**What distinguishes it:** Activity is *ephemeral*. It has a birth (session start, agent spawn, script invocation) and a death (session end, task completion, script exit). Nothing in Activity survives without being captured into another layer. If something persists between sessions, it is not Activity.

**Examples:** An active AI coding session, a running task-spawned agent, an active scheduled process, a hook script currently executing, a script in flight.

### Five sub-layers defined

#### Knowledge / Principles (normative — what should be)

**Definition:** The system's normative beliefs — deliberate, codified judgments about how things should work. Principles are prescriptive: they tell the system what to do and what not to do.

**Key characteristics:**
- Highest authority below human real-time command
- Slowest rate of change (weeks between updates; never without cross-session evidence)
- Requires human judgment to modify (the Codification arrow — see section 8)
- Small in volume, large in influence

**What belongs here:** Build bible principles, build bible patterns, build bible anti-patterns, architectural decisions, the Atlas model itself.

**What doesn't belong here:** Config file directives (those are Machinery — they fire), rules files (those are Protocols — they fire), anything that loads automatically.

#### Knowledge / Experience (empirical — what has been)

**Definition:** The system's accumulated empirical record — what happened, what was observed, what was learned. Experience is descriptive: it records reality rather than prescribing behavior.

**Key characteristics:**
- Medium authority (informs but doesn't command)
- Grows continuously through the Capture arrow (Activity -> Experience)
- Eventually consistent — there is always some lag between what happened and what's recorded
- Large in volume, valuable in aggregate

**What belongs here:** Session memory (auto-memory), spaced-repetition records, session index data (queried as reference), meeting transcript archives (queried as reference), system inventory (canonical empirical record of what exists), contacts databases (queried as reference), intelligence records, past session artifacts consulted for context.

**What doesn't belong here:** Running queries against these databases (that's Activity). The databases themselves as I/O machinery (that's Capability/Machinery). The distinction: the *data* is Experience; the *database engine and schema* is Machinery.

#### Capability / Machinery (standing apparatus — files, agents, scripts, configs that load and fire)

**Definition:** The standing technical infrastructure that loads, fires, and executes. Machinery is the system's skeleton and muscles — it doesn't decide what to do (that's Protocols), but it provides the apparatus to do it.

**Key characteristics:**
- Fires automatically or on invocation — never just sits there
- Persists between sessions
- Medium rate of change (days-weeks)
- Configuration-heavy — many Machinery components are configured by Protocols

**What belongs here:** Config files (load every session and configure behavior), agent definitions (spawn when invoked), scheduled job definitions (fire on schedule), scripts (fire when invoked), databases (active I/O infrastructure), slash commands (fire when invoked), MCP server configurations, plugin installations.

**What doesn't belong here:** The build bible (consulted, not fired — Knowledge/Principles). Session memory (empirical record, not apparatus — Knowledge/Experience). Hooks (behavioral governance — Capability/Protocols).

#### Capability / Protocols (behavioral governance — rules, hooks, skills, sweeps that govern behavior)

**Definition:** The behavioral rules and automated governance mechanisms that constrain and direct how Machinery and Activity operate. Protocols are the system's nervous system — they sense conditions and trigger responses.

**Key characteristics:**
- Fires automatically on triggers (hooks) or when loaded (rules files, skills)
- Governs behavior rather than providing infrastructure
- Medium-fast rate of change (days-weeks, faster than Machinery)
- The bridge between Knowledge (which says what SHOULD happen) and Activity (which is happening NOW)

**What belongs here:** Rules files (load and govern behavior), hooks (fire on events), skills (invoked and fire), sweeps (invoked and fire), verification protocols, questioning protocol, steelman protocol, security audit protocol.

**What doesn't belong here:** Config files (that's Machinery — it's infrastructure that loads, not governance that constrains). The build bible (that's Knowledge — it's consulted, Protocols enforce it). Agent definitions (that's Machinery — agents are apparatus, not governance).

**The distinguishing question between Machinery and Protocols:** Does this component *provide infrastructure* (Machinery) or *govern behavior* (Protocols)? A database is infrastructure. A hook that checks file size before writes is governance. A config file is infrastructure (it loads context). A rules file is governance (it constrains behavior). When ambiguous, ask: "If I removed this, would the system lose a *capability* (Machinery) or lose a *constraint* (Protocols)?"

#### Activity / Execution (live processes — sessions, running agents, tool calls, scripts in flight)

**Definition:** Everything currently alive and running. Execution is the system's heartbeat — it exists only in the present tense.

**Key characteristics:**
- Fully ephemeral — born from Configuration, dies when process ends
- Fastest rate of change (seconds-hours)
- Lowest authority (Activity follows orders from Capability and Knowledge)
- The only layer where actual work happens — Knowledge and Capability are inert without Activity

**What belongs here:** Active AI coding sessions, running task-spawned agents, active scheduled processes, hook scripts currently executing, scripts in flight, tool calls in progress, MCP server connections currently open.

**What doesn't belong here:** Anything that persists after the process ends. A file written by an agent is not Activity — it's an artifact that lives in whichever layer it belongs to. The agent *while running* is Activity. The agent definition *on disk* is Capability/Machinery.

### The 3 placement tests

When you need to determine where a component belongs, apply these tests in order. Most components are resolved by test 1 alone.

#### Test 1 — Consult-vs-fire

**Question:** "Would the AI consult this like a reference, or does it load and fire automatically?"

**How to apply:** Imagine a new session starts. Does this component get pulled in and read when the AI needs it (consult -> Knowledge)? Or does it load/activate/fire without the AI choosing to consult it (fire -> Capability)? Or is it a currently-running process (Activity)?

**Correct placement example:** Rules files fire automatically — they load into every session and constrain behavior without the AI choosing to read them. -> **Capability/Protocols.** The build bible is consulted — the AI loads it when it needs principle guidance, not automatically. -> **Knowledge/Principles.**

**Counter-example where intuition misleads:** Agent definitions feel like knowledge — you'd read them to understand the system's agent roster. But they *fire*: when a session spawns an agent, the agent definition loads and configures that agent's behavior. The definition is standing apparatus that activates on invocation. -> **Capability/Machinery**, not Knowledge.

#### Test 2 — Authority-record

**Question:** "Is this the canonical record, or a derived copy?"

**How to apply:** Trace the information to its source. If this component IS the authoritative source — the place where this information is created and maintained — it lives where its nature dictates. If it's a summary, copy, or derivative of another source, the derivative lives downstream of the canonical source.

**Correct placement example:** Build bible rule 1.1 (Orchestrate, don't execute) -> canonical, lives in Knowledge/Principles. The config file summary of that same rule ("Orchestrate, don't execute — delegate to specialist agents") -> derived copy that lives in Capability/Machinery. The config file entry isn't a principle; it's a piece of machinery that loads the principle's summary into each session.

**Counter-example where intuition misleads:** A system inventory document feels like it should live in Capability — it documents Capability components, after all. But the inventory IS the canonical empirical record of what exists in the system. It's not machinery that fires; it's a reference document that gets consulted. -> **Knowledge/Experience**, because it's the authoritative empirical record, not a piece of standing apparatus.

#### Test 3 — Stability-as-property

**Question:** "Is this stable because it's foundational, or stable because it's battle-tested?"

**How to apply:** Some components are very stable. The question is *why* they're stable. If stable because they express foundational beliefs (principles, architectural decisions), they belong in Knowledge/Principles. If stable because they've been refined through use and their current form works well (mature scripts, proven agent definitions), they belong in Capability. Stability is a *property*, not a *location indicator*.

**Correct placement example:** Agent definitions may be very stable — they rarely change. But they're stable because they've been tested and refined across hundreds of sessions, not because they express a foundational belief about how things should work. -> **Capability/Machinery.** A new build bible rule is foundational from day 1, even before it's proven. -> **Knowledge/Principles.**

**Counter-example where intuition misleads:** Database schemas are extremely stable and feel "foundational." But they're infrastructure — active I/O machinery that fires on every read/write. Their stability comes from maturity, not from expressing normative beliefs. -> **Capability/Machinery**, not Knowledge/Principles. If you promoted a schema to Principles, you'd be saying "this schema is a belief about how things should work" rather than "this schema is a piece of infrastructure that works well." The build bible principle that governs databases might be "single source of truth" — *that's* the principle. The schema is the machinery that implements it.

### Quick-placement table

| Component | Layer | Sub-layer | Deciding test |
|-----------|-------|-----------|---------------|
| Build bible | Knowledge | Principles | Consult-vs-fire: consulted, not fired |
| Principles | Knowledge | Principles | Authority-record: canonical normative source |
| Patterns | Knowledge | Principles | Authority-record: canonical normative source |
| Anti-patterns | Knowledge | Principles | Authority-record: canonical normative source |
| Atlas (this document) | Knowledge | Principles | Authority-record: canonical architectural model |
| Architectural decisions | Knowledge | Principles | Stability-as-property: foundational from day 1 |
| Session memory | Knowledge | Experience | Authority-record: canonical empirical record of past sessions |
| Spaced-repetition records | Knowledge | Experience | Consult-vs-fire: consulted (reviewed for learning), not fired |
| Session index data | Knowledge | Experience | Authority-record: canonical empirical record of session history |
| Transcript archives | Knowledge | Experience | Authority-record: canonical empirical record of meetings |
| System inventory | Knowledge | Experience | Authority-record: canonical empirical record of what exists |
| Contacts data | Knowledge | Experience | Authority-record: canonical empirical record of contacts |
| Intelligence records | Knowledge | Experience | Authority-record: canonical empirical record of relationships |
| Config files (multi-tier) | Capability | Machinery | Consult-vs-fire: loads and fires every session |
| Agent definitions | Capability | Machinery | Consult-vs-fire: fires when agents spawn |
| Scheduled job definitions | Capability | Machinery | Consult-vs-fire: fires on schedule |
| Scripts | Capability | Machinery | Consult-vs-fire: fires when invoked |
| Databases (as infrastructure) | Capability | Machinery | Consult-vs-fire: active I/O machinery |
| Slash commands | Capability | Machinery | Consult-vs-fire: fires when invoked |
| MCP server configurations | Capability | Machinery | Consult-vs-fire: fires on connection |
| Plugin installations | Capability | Machinery | Consult-vs-fire: fires when activated |
| Rules files | Capability | Protocols | Consult-vs-fire: loads and governs behavior |
| Hooks | Capability | Protocols | Consult-vs-fire: fires automatically on events |
| Skills | Capability | Protocols | Consult-vs-fire: invoked and fires |
| Sweeps (systematic audits) | Capability | Protocols | Consult-vs-fire: invoked and fires |
| Verification protocols | Capability | Protocols | Governs behavior: constrains completion claims |
| Questioning protocol | Capability | Protocols | Governs behavior: constrains action without clarification |
| Review protocols | Capability | Protocols | Governs behavior: constrains plan approval |
| Security audit protocol | Capability | Protocols | Governs behavior: constrains component installation |
| AI coding sessions | Activity | Execution | Ephemeral: born from Configuration arrow |
| Running agents (task-spawned) | Activity | Execution | Ephemeral: live processes |
| Active scheduled processes | Activity | Execution | Ephemeral: running now |
| Hook scripts currently executing | Activity | Execution | Ephemeral: in-flight |
| Tool calls in progress | Activity | Execution | Ephemeral: in-flight |

### Theoretical backing

**Pace Layering (Brand).** Stewart Brand's "How Buildings Learn" identified that complex systems have layers that change at different rates — the faster layers innovate and the slower layers constrain and stabilize. In this model: Activity changes with every session (seconds-hours), Capability changes as tools and protocols mature (days-weeks), Knowledge/Experience grows continuously but slowly (days-months), and Knowledge/Principles changes only when well-tested insights warrant codification (weeks-months — faster than traditional organizations because AI reduces validation cost, but still slower than Protocols). The slow layers govern the fast ones through the Constraint and Configuration arrows. This is not a metaphor — it is the literal mechanism. The build bible (slow, Principles) constrains what hooks and skills (faster, Protocols) are allowed to do. Config files (medium, Machinery) configure each session (fast, Execution). Violating pace layering — letting a fast layer modify a slow one without human judgment — is the definition of the "false codification" failure mode.

**Viable System Model (Beer).** Stafford Beer's VSM describes what every self-maintaining system needs. System 1 (operations) maps to Activity/Execution — where work actually happens. System 2 (coordination) maps to the Configuration arrow — how Capability configures each execution instance. System 3 (management) maps to Capability — machinery and protocols that govern and configure operations. System 4 (intelligence) maps to Knowledge/Experience — the empirical learning layer that scans the environment and feeds adaptation. System 5 (policy) maps to Knowledge/Principles — the normative layer that defines identity, values, and constraints. The Atlas gives Beer's abstract model concrete locations: System 5 lives in your build bible. System 1 lives in whatever AI process is currently running.

**Panarchy (Holling).** Holling's adaptive cycle theory describes how complex systems cycle through growth (r), conservation (K), collapse (Omega), and reorganization (alpha). This system's feedback arrows map onto the panarchy cycle. Capture (Activity -> Experience) is rapid accumulation (r phase) — every session deposits empirical data. Adaptation (Experience -> Protocols) is consolidation (K phase) — accumulated experience tightens and tunes behavioral governance. Codification (Protocols -> Principles) is creative destruction and reorganization (Omega/alpha) — a mature protocol earns promotion to a principle, which may force restructuring of other principles and protocols. Understanding panarchy explains why Codification is rare and requires human judgment: reorganizing Principles is a high-stakes Omega event, not a routine update.

### The dual-lens principle

Functional classification (Knowledge / Capability / Activity) is the primary lens — it answers WHERE things live and WHY. Temporal classification (how fast things change) is the secondary lens — it answers WHEN things need maintenance and how urgently changes propagate.

Do not confuse them. Something can change slowly and still live in Capability (stable agent definitions that haven't been modified in months are still Machinery — they fire). Something can change quickly and still live in Knowledge (session memory updates frequently but is still Experience — it's consulted, not fired). Rate of change is a maintenance signal, not a location indicator.

When the two lenses disagree with your intuition, trust the functional lens. A component's *kind* (consultable vs. firing vs. ephemeral) determines its home. Its *rate of change* determines its maintenance schedule.

### What this model does not tell you

- **Change cadence and maintenance schedules** — use the pace-layer table in section 9
- **Component lifecycle** (born, mature, deprecated, archived) — that's the system inventory's job
- **What should exist** — only where things that DO exist should live. Gaps are tracked in the build bible's debt inventory
- **Cost or performance** — those are orthogonal concerns tracked in the build bible
- **Intra-layer conflicts** — those are handled by section 2 (precedence & conflict resolution)
- **Inter-layer flows** — those are handled by section 8 (the arrows)

---

## Section 2: Precedence & conflict resolution

### Why precedence matters

When two directives conflict, the system needs a deterministic resolution mechanism. Without one, the AI falls back to best-guess reasoning — which is inconsistent across sessions and produces unpredictable behavior. Session A resolves a conflict one way; session B resolves the same conflict differently; neither documents the reasoning; the inconsistency compounds. The precedence hierarchy provides a lookup table: given two conflicting directives, the resolution is mechanical, not interpretive.

The governing rule is: **broader scope wins over narrower scope; explicit wins over implicit.** A human real-time command is explicit and has maximum scope (this session, this moment, overrides everything). A system default is implicit and has minimum scope (only when nothing else applies). The seven levels below implement this rule as a concrete hierarchy.

### The 7 levels

**1. Human real-time command** — The operator speaking now takes absolute precedence. The system serves the operator, not the other way around. Any instruction from the operator in a live session overrides every level below. This is not a theoretical principle — it is the operational reality that the entire system exists to serve human judgment. When the operator says "ignore that rule and do X," the AI does X.

**2. Principles (build bible)** — The build bible contains the system's normative knowledge: critical rules, reusable patterns, anti-patterns. These represent well-tested, deliberately codified insights that have survived adversarial review and real-world application across multiple projects. High stability, high authority. Principles override everything except the operator's direct command.

**3. Architectural decisions** — Scoped applications of principles to specific contexts. "We use SQLite for all databases in this system" is an architectural decision. "All analysis tasks use a minimum quality tier for accuracy" is an architectural decision. These are documented in the build bible, in project `decisions.md` files, or in the Atlas itself. They override config files and protocols because architectural decisions represent deliberate, documented judgment calls — they are principles applied to a specific domain.

**4. Config file directives** — The multi-tier config file system (Global -> Rules -> Project -> Operations) contains standing behavioral instructions that load every session. More specific than principles (they tell the AI what to do in this project, not universally), less universal than architectural decisions. Within the config file hierarchy: operations-level overrides project-level; project-level overrides global-level. More specific scope wins.

**5. Protocols / Rules** — Rules files, verification protocols, questioning protocol, review protocols, security audit protocol. These are behavioral governance — they tell the AI WHAT to do in specific situations. More specific than config files (they govern particular behaviors, not overall session configuration), less authoritative because they are enforcement mechanisms for higher-level directives rather than directives themselves.

**6. Hooks / Skills** — Automated behaviors (hooks across event types) and on-demand capabilities (skills). Most specific, least authoritative among configured behaviors. A hook that fires on a post-tool-use event is a narrow, specific behavior. A skill loaded for a single task is narrower still. These implement protocols and config file directives — they don't override them.

**7. System defaults** — What happens when nothing else specifies behavior. The AI model's training defaults, fallback behaviors, format conventions. The implicit baseline. Overridden by everything above.

### The precedence rule

> When two directives conflict: the one with broader scope wins. When scope is equal: the explicit directive wins over the implicit one. When both are explicit and equal in scope: the higher level in the hierarchy (lower number) wins.

### Conflict examples

**Example 1 — Hook contradicts config file:**
- Hook (PostToolUse): "Always add a timestamp comment to every file you edit"
- Config file directive: "Only add comments where logic isn't self-evident"
- **Resolution:** Config file (level 4) > Hook (level 6). Follow the config file — add comments only where non-obvious. If the hook is producing wrong behavior, update it via the Adaptation arrow (see section 8).

**Example 2 — Skill behavior contradicts rules file:**
- Skill loaded: a design skill outputs files to `~/Desktop`
- Rules file: all output files go to an inbox folder unless destination is clear
- **Resolution:** Rules/Protocols (level 5) > Skill behavior (level 6). Route output to the inbox. The skill's default output path is a suggestion; the rules file is governance.

**Example 3 — Build bible principle vs operator's real-time command:**
- Build bible rule: "Orchestrate, don't execute — delegate to specialist agents"
- Operator says: "Just write this function directly, don't spawn an agent"
- **Resolution:** Human command (level 1) > Principles (level 2). Write the function directly. This is correct behavior — the build bible serves the operator, not the reverse. The operator's override here does not change the principle; it exercises the human's absolute right to override any level below.

**Example 4 — Two rules files contradict each other:**
- Global rules file: "No files > 500 lines"
- A project-level rules file: "This generated file is expected to be 1,200 lines — do not split it"
- **Resolution:** Both are level 5 (Protocols). Apply scope rule: more specific scope wins. The project-level rule overrides the global rule for that specific file. The global 500-line rule still applies to all other files in the project.

**Example 5 — Config file directive vs architectural decision:**
- Config file (global): "Use the cheapest model tier for simple tasks to minimize cost"
- Architectural decision (documented): "All session-index analysis uses a higher quality tier for accuracy"
- **Resolution:** Architectural decision (level 3) > Config file directive (level 4). Use the higher quality tier for session-index analysis. The cost optimization directive yields to the quality-specific architectural decision.

### Decision tree for genuine conflicts

When two directives conflict, walk this tree:

1. **Is one a live instruction from the operator?** -> Follow the operator. Always. No exceptions.
2. **Is one a build bible principle and the other a derived directive** (config file, rules file, hook)? -> Follow the build bible principle. Flag the derived directive as needing update — it should be brought into alignment with the principle it violates.
3. **Are both the same level** (e.g., two rules files, two config file tiers)? -> More specific scope wins. Project-level overrides global. Domain-specific overrides general-purpose.
4. **Are both the same level and same scope?** -> More explicit wins over implicit. A directive that says "do X in situation Y" beats a default behavior pattern.
5. **Still ambiguous after steps 1-4?** -> **Stop. Ask the operator.** Document the conflict in `decisions.md` (if task notes exist) or in the session summary. Flag it for the next Atlas/build bible review.

When a conflict reaches step 5 — when the decision tree genuinely cannot resolve it — that signals a gap in the system. The resolution the operator provides should be documented as either a new architectural decision (level 3) or a config file update (level 4) so the same conflict never reaches step 5 again. Track these in the Atlas evolution protocol (section 11).

### Edge cases

**Config files contain principle summaries.** Config files often include condensed versions of build bible principles. When the summary conflicts with the full build bible text, the build bible text wins — it is the canonical source (Authority-record test). The config file summary is a derived copy at level 4; the build bible principle is the authority at level 2.

**Hooks enforce principles.** Some hooks directly enforce build bible principles (e.g., a delegation-checking hook enforces "Orchestrate, don't execute"). The hook is level 6 but it enforces a level 2 directive. If the hook's implementation diverges from the principle it enforces, the principle wins and the hook should be updated. The hook does not gain the principle's authority — it is still a level 6 mechanism.

**Protocols reference each other.** A review protocol references the questioning protocol. A verification protocol references both. When protocols reference each other, they operate as a chain, not a hierarchy — the full chain applies. A conflict between two protocols at the same level falls to step 3 of the decision tree: more specific scope wins.

---

## Section 8: The arrows — inter-layer flows

### Why flows matter as much as containers

A static box diagram of Knowledge / Capability / Activity is a snapshot of the system's structure. It tells you what exists and where it lives. But it tells you nothing about how the system behaves over time — how a mistake in a Tuesday session becomes a hook that fires on Thursday, or how an article the operator reads in January becomes a build bible principle that governs all work in March. The arrows are the system's behavior over time. Structure without flow is a museum exhibit — accurate but inert. Flow without structure is chaos — active but uncontrolled. The six arrows define how the system learns, adapts, and maintains coherence across the three layers.

An earlier model had "Feedback" as a sub-layer of an "Operation" layer — a box that collected experience and sprayed it upward. That was a category error. Feedback is a flow, not a stock. The box metaphor suggests a location where experience accumulates before being broadcast, like a mailroom sorting observations into envelopes. The arrow metaphor reveals what's actually happening: three distinct feedback loops, each with different timescale, consistency requirement, and authority level. Capture writes observations to Experience. Adaptation tunes protocols when patterns emerge. Codification — rarely — elevates a mature protocol to a principle. These are different operations with different stakes, not stages in a pipeline.

Activity doesn't spray to all layers simultaneously. The feedback chain is sequential and gated. Experience accumulates via Capture, then tunes protocols when patterns emerge via Adaptation, then — rarely — becomes a principle when evidence is overwhelming via Codification. Three distinct loops, three distinct authority levels. Fast loop: Configuration fires at session start, Execution produces work, Capture writes observations, Adaptation tunes protocols (minutes to weeks). Slow loop: Adaptation matures protocols, Codification elevates proven ones to principles, Constraint propagates principles to Capability, Configuration delivers them to new sessions (months to quarters). Emergency bypass: Escalation moves a live insight directly to Principles, skipping the entire accumulation chain (immediate, but rare and human-triggered).

Each arrow has a consistency requirement that matches its blast radius. Capture is eventually consistent — asynchronous, may be incomplete, loses one session's learnings at worst. Adaptation is causally consistent — protocol changes must trace to specific evidence, because an untraceable protocol change is technical debt. Codification is strongly consistent — new principles constrain everything downstream via Constraint, so a false principle corrupts the entire system. This gradient exists because damage scales with proximity to Principles: a broken Capture loses observations; a broken Codification introduces a false principle that constrains all future work across all sessions.

Authority follows the same gradient. Capture is automatic — low authority, happens without human decision, because the cost of over-capturing is noise while the cost of under-capturing is amnesia. Adaptation is low-medium authority — the AI can propose protocol changes and execute minor ones, but significant changes need human confirmation. Codification is human-judgment-required — wrong principles are expensive to fix and propagate through Constraint to every downstream component. Escalation is human-only — the most restricted arrow, because it bypasses the evidence accumulation that makes Principles trustworthy. The gradient protects Principles from noise: the closer an arrow gets to Principles, the more authority it requires to fire.

---

### Arrow 1: Constraint — Knowledge/Principles to Capability

**Direction:** Knowledge/Principles -> Capability (both Machinery and Protocols)
**Type:** Downward governing arrow

**Mechanism:** Principles define the authority frame that makes Capability coherent. This is not enforcement — Principles don't fire checks or block operations. It's definition: Principles establish the constraints that config files, rules files, agent definitions, hook behaviors, and skill behaviors must respect. The derivation relationship is the key: every component in Capability is downstream of Principles. The build bible's rules establish what Machinery can be built and what Protocols can require. When a config file is written, it derives from build bible principles. When a rules file is created, it implements specific build bible rules. When a hook is authored, it enforces a specific principle. Changes in Principles cascade — a build bible update requires tracing every derived copy and updating it.

An enforcement traceability matrix makes Constraint explicit. Each principle maps to specific enforcement mechanisms. For example:
- A principle about delegation -> a delegation-checking hook, a delegation tracker
- A principle about simplicity -> a size-watching hook, a size audit sweep, a code quality rule
- A principle about observability -> a deploy phase gate, an observability audit sweep

The matrix also reveals enforcement gaps: principles with no automated enforcement. These gaps should be documented as known debt.

**Timescale:** Always active — Constraint is not a periodic event but a standing relationship. However, the downstream effect (derived copies updating after a build bible change) happens at the cadence of principle updates. The propagation delay — time between a principle update and all derived copies reflecting the change — is the window of vulnerability.

**Consistency model:** Strong consistency required. Derived copies MUST accurately reflect canonical sources. A config file that summarizes a build bible rule incorrectly is not just a documentation problem — it produces inconsistent behavior. Sessions where the config file is consulted will behave differently from sessions where the build bible is consulted directly. A multi-tier config file hierarchy amplifies this risk: a principle may be accurately stated at the global tier but incorrectly summarized at the project tier.

**Authority required:** Implicit. Principles don't actively enforce; they define the frame. Enforcement happens at the Protocols level (hooks, rules files) which are derived from Principles. The authority is structural, not operational.

**Concrete examples:**
1. Build bible rule ("Orchestrate, don't execute") -> config file "Primary directive" section -> rules file summary -> delegation-checking hook -> every session starts with conductor mindset and is actively monitored for solo execution
2. Build bible verification requirement -> verification protocol reference doc -> auto-loaded review rule -> every plan receives adversarial review
3. Build bible rule set -> rules file summary -> auto-loaded every session as a compressed reference -> sessions can consult the full build bible for depth but always have the summary active
4. Architectural decision (e.g., "SQLite for all databases") -> constrains all agent database designs -> constrains backup strategy -> constrains data access patterns

**Failure mode: Drift.** The most dangerous silent failure in the system. The build bible changes but derived copies don't update. Sessions running config files see one behavior; sessions consulting the build bible directly see another. The system behaves inconsistently across sessions. Because a multi-tier config file hierarchy can have thousands of lines across multiple files, drift can accumulate undetected.

**How to detect failure:** Currently, in most systems, no automated mechanism detects derived-copy drift. Detection is manual: load a build bible principle, then check its config file implementation for accuracy. Discrepancy = drift. A consistency sweep (section 9) can automate this by comparing canonical sources against derived copies.

**Recovery:** Update all derived copies. Use an enforcement traceability matrix to identify which files derive from which principles. Update each. Version-bump the build bible if the update represents a structural change. The enforcement chain to trace:
```
Build bible rule change -> config file update -> rules file update -> hook behavior update -> skill behavior update
```
Missing any step in this chain leaves a derived copy drifted.

---

### Arrow 2: Configuration — Capability to Activity/Execution

**Direction:** Capability (Machinery + Protocols) -> Activity/Execution
**Type:** Session instantiation arrow

**Mechanism:** Every execution context — AI coding session, running agent, scheduled process — is born by loading the Configuration that Capability provides. At session start, a boot sequence fires:

1. Global config file — universal behaviors: conductor mindset, questioning protocol, verification protocol, response format
2. Global rules files — auto-loaded behavioral governance: code quality, build bible summary, review protocols, voice rules, integration rules
3. Project config file — project-specific instructions
4. Project rules files — any rules in the project's rules directory
5. Operations config file (if present) — operation-level overrides
6. Session memory — persistent project memory from previous sessions
7. SessionStart hooks fire — smart context loading, file hygiene checks, memory system initialization

After boot: hooks arm (register for their trigger events across event types). Skills become invocable but are NOT loaded — they load on demand when invoked. The accumulated state of Capability is what each execution inherits.

Configuration is not gradual or negotiable. It happens at session instantiation. What loads is what governs. There is no mid-session Configuration update — if a rules file changes after session start, that session continues with its loaded version. The next session gets the updated version.

**Timescale:** Per-session, immediate. Configuration fires once at session start and produces a fixed execution context.

**Consistency model:** Immediate within a session. What's loaded at session start is what governs that session. There is no lag between "rules file updated" and "new session uses it." However, existing sessions see their loaded configuration, not updates made after session start. This creates a window where two concurrent sessions can have different configurations — one loaded before a rules update, one after.

**Authority required:** Automatic. No human decision required for Configuration to happen. It is the structural consequence of Capability existing and a session starting.

**Failure mode: Stale configuration** (config file references outdated principles, rules file not updated after build bible change) or **broken hook** (hook registers but fails to fire, or fires incorrectly). Either produces a session that thinks it's correctly configured but isn't.

A subtler failure: **configuration overload**. If instruction volume exceeds the model's effective attention window, later instructions may be deprioritized. A config file hierarchy that has grown to thousands of lines with duplications and contradictions is both too large and internally inconsistent.

**How to detect failure:** Session behavior doesn't match expected. A planning task completes without review -> review rule not loaded or instructions lost in volume. A quality violation occurs without hook intervention -> quality hook broken or not armed. A delegation violation goes undetected -> delegation hook not firing.

**Recovery:** Restart session (reloads all configuration). For broken hooks: reload all configuration. For persistent issues: inspect hook code directly. For stale configuration: trace back to Constraint arrow and verify derived copies match canonical source.

---

### Arrow 3: Capture — Activity/Execution to Knowledge/Experience

**Direction:** Activity/Execution -> Knowledge/Experience
**Type:** Learning arrow

**Mechanism:** Capture is the only path from "what happened in a session" to "what the system knows." Without Capture, Activity is a black hole — work happens and disappears. Multiple capture mechanisms operate at different automation levels:

**Automatic capture** (fires without human intervention):
- SessionEnd hooks: generate intelligent session name, write topic to session index, generate state file for next-session continuity, extract memories, log hook statistics
- Pre-compaction hooks: capture session name and topic before context loss, run memory curation
- User-prompt hooks: track exchange count, capture topic periodically, detect project mentions

**AI-initiated capture** (the AI writes during session):
- Session memory updates — persistent project memory written during session
- Mistake documentation — structured mistake entries with tags
- File outputs that become permanent records (task notes, decisions, open questions)

**Operator-initiated capture** (human decides significance):
- End-of-session ritual — summary, file cleanup, rename, index
- Direct memory edits with elevated priority
- Learnings queue entries for later review
- Decision journal entries for complex choices

A session index database is the backbone of Capture. Every session produces an entry: session ID, name, topics, timestamps, exchange count. A search tool provides keyword lookup, context retrieval, and analytics. This is how past work becomes findable.

**Timescale:** Minutes for automatic captures (hook-driven, fires at session milestones). Session-duration for AI-initiated captures (can happen any time). Indefinite for operator-initiated captures (the operator may capture days after a session, though this creates risk — see the principle of documenting when fresh).

**Consistency model:** Eventually consistent. Captures happen asynchronously and may be incomplete. A session can end with no meaningful Capture at all if automatic mechanisms fail and no one initiates manual capture.

**Authority required:** Automatic (for system captures — very low threshold). Low (for AI capturing to memory — low friction). Human judgment (for significance filtering — what's worth capturing vs. noise).

**Failure mode: Capture gap** — session completes, work happens, nothing is written back to Experience. Future sessions have no record of what happened. Patterns repeat. Mistakes recur. The same debugging approach gets retried. The same wrong approach gets attempted. Capture gap is the most common failure in the system, because most capture mechanisms require either deliberate action or automatic hooks to fire correctly.

**How to detect failure:** Sessions repeat the same mistakes -> check mistake documentation completeness. "We discussed this in a previous session" but search finds no record -> Capture gap. Session memory doesn't reflect work done -> AI-initiated capture missed. Session index has a gap in entries -> automatic SessionEnd hooks failed.

**Recovery:** There is no recovery for a missed capture — the moment and its context are gone. The principle of documenting when fresh exists because of this: rationale is clearest at the moment of decision, and degrades irreversibly with time.

**Prevention:** An end-of-session ritual helps but isn't sufficient. The most important captures should happen immediately when insights occur, not retroactively. Multiple redundant capture mechanisms (automatic + AI-initiated + operator-initiated) create defense in depth — if one layer fails, another may catch it.

The Capture arrow is the most fragile in any AI-augmented system. Knowledge doesn't accumulate if Capture doesn't fire. Every significant insight, mistake, or decision should be captured before the session ends — but ideally at the moment it occurs, not during a rushed end-of-session ritual.

---

### Arrow 4: Adaptation — Knowledge/Experience to Capability/Protocols

**Direction:** Knowledge/Experience -> Capability/Protocols
**Type:** Improvement arrow

**Mechanism:** Accumulated experience in Knowledge/Experience tunes the behavioral governance in Capability/Protocols. Adaptation is how the system gets better without requiring human architectural decisions for every small improvement. It is the bridge between "we noticed this pattern" and "we now prevent or encourage it."

The Adaptation flow works through several concrete pathways:

1. **Mistake patterns -> hooks.** A mistake documented three times suggests a hook should prevent it. The process turns a recurring observation into an active prevention mechanism. Example: repeated solo execution -> delegation-checking hook created with enforcement tracking.

2. **Learnings queue -> rules files.** A learnings queue accumulates observations from sessions. A periodic review walks through pending items. Confirmed learnings are promoted. Patterns across learnings suggest rules file updates. Example: repeated observation "voice skill must load before writing as the operator" -> explicit rule added.

3. **Session memory patterns -> config file updates.** When session memory repeatedly records the same preference or workflow pattern, it becomes a candidate for config file codification. Example: "adversarial debates are the default for brainstorming" observed across multiple sessions -> added to memory -> potential config file rule.

4. **Session index patterns -> process improvements.** Patterns in session history reveal workflow issues. Example: sessions frequently end without rename -> auto-rename hook created. Sessions often have stale names -> periodic rename hook added.

5. **Hook event logs -> hook tuning.** Hook event logs record every hook firing. Periodic review identifies hooks that never fire (candidates for removal) and hooks that fire too aggressively (candidates for threshold tuning).

**Timescale:** Days to weeks. Experience must accumulate before patterns emerge. Patterns must be confirmed before Protocol changes. A healthy update process specifies: observation across 3+ sessions -> proposal with evidence -> review -> trial period -> measurement -> promotion. Rushing Adaptation produces unstable protocols.

**Consistency model:** Causal consistency. Protocol changes must be traceable to specific Experience evidence. Untraceable Protocol changes are technical debt — they exist without justification and resist pruning because no one knows if they're still needed.

**Authority required:** Low-medium. The AI can propose Protocol updates (new rule, adjusted hook behavior, new skill) and execute them for minor changes. Significant Protocol changes require human confirmation. Creating new hooks requires human approval — hooks are high-risk changes because they fire automatically on every session.

**Failure mode: Rigidity trap** — rules and hooks pile up without pruning, each one addressing a specific past mistake, until the system is so governed it can barely move. The protocols become bureaucracy. Each hook was a reasonable Adaptation at the time — but their accumulated weight creates friction.

Deletion criteria counteract rigidity: not referenced in a set period, contradicted by evidence, superseded by a better pattern, or failed its trial period. But deletion is harder than addition — removing a rule feels risky ("what if the problem it prevented comes back?") while adding a rule feels safe ("we're preventing future mistakes").

**How to detect failure:** Sessions feel overly restricted — the AI spends more time satisfying protocol requirements than doing useful work. Hooks fire on legitimate work and block it. Rules files reference contexts that no longer exist. The exchange counter climbs but output doesn't — overhead is consuming capacity.

**Recovery:** Protocol audit — review each rule, hook, and skill for current relevance. Remove or relax rules that no longer serve. Consolidate overlapping mechanisms. The review process should explicitly include "what should we STOP doing" alongside "what should we START doing."

---

### Arrow 5: Codification — Capability/Protocols to Knowledge/Principles

**Direction:** Capability/Protocols -> Knowledge/Principles
**Type:** The judgment arrow

**Mechanism:** Codification is the highest-stakes arrow in the system. It is the path from "this is how we've been doing it" to "this is how we should always do it." A protocol that has proven itself through repeated use, across multiple contexts, under scrutiny, earns elevation to a Principle. The process has deliberate friction because false codification — promoting a context-specific pattern to a universal principle — is expensive and hard to reverse.

A structured ingestion pipeline is the primary mechanism for Codification:

1. **Input:** External knowledge (article, repo, insight) or internal observation
2. **Mapping:** Content mapped against existing build bible sections — what already exists, what's new, what conflicts
3. **Classification:** Each proposed addition classified as ADDITIVE (extends existing), UPGRADE (replaces existing with better version), NOVEL (addresses a gap), or REDUNDANT (already covered)
4. **Review:** Is this actually universal, or is it context-specific? Is this proven, or premature? Does it survive adversarial critique?
5. **Proposal:** Presented to the operator with evidence, classification, and recommended placement
6. **Integration:** After human approval — build bible updated, version bumped, derived copies updated via Constraint arrow

A full update process specifies: Observation (3+ sessions) -> Proposal (specific rule with enforcement and evidence) -> Review -> Trial (advisory period) -> Measurement -> Promotion -> Documentation.

The maturation timeline matters. A pattern observed once isn't a principle. Confirmed across many sessions, multiple contexts, adversarial review — that's a candidate.

**Timescale:** Months to quarters. Significant maturation required. Quick Codification is almost always premature Codification.

**Consistency model:** Strong. New principles constrain everything downstream via the Constraint arrow. A poorly-codified principle cascades: build bible -> config files -> rules files -> hooks -> every future session. The blast radius is system-wide. This is why the process has so much deliberate friction.

**Authority required:** Human judgment required. The operator must approve. No autonomous Codification. The AI can propose, assemble evidence, run the review, and prepare the integration — but the final gate is human. This gate exists because false codification is a serious and hard-to-reverse failure.

**Concrete examples:**
1. External article on agent orchestration -> ingestion pipeline -> classified as ADDITIVE to an existing pattern -> review passes -> operator approves -> build bible version bumped -> config files updated -> all future sessions inherit the improvement
2. Repeated session pattern: "orchestration vs. execution confusion" -> evidence assembled -> proposed as anti-pattern -> review confirms -> approved -> build bible updated -> enforcement hook created
3. Protocol from rules file: "no files > 500 lines" -> confirmed effective across 50+ sessions -> elevated to critical rule in build bible -> enforcement hook created -> audit sweep created -> enforcement traceability matrix updated

**Failure mode: False codification** — premature principle added before sufficient evidence, or context-specific rule elevated to universal principle. A false principle constrains future work unnecessarily and may produce worse outcomes in new contexts. It's hard to undo because removing a principle requires the same full Codification process (evidence that the principle is wrong, review that the removal is justified, human approval). The asymmetry between adding and removing principles means false codifications accumulate.

**How to detect failure:** Build bible principle produces confusion or counterproductive behavior in new contexts -> may be false codification. Principle was added with limited evidence -> verify evidence base. Principle references a specific project but claims universal applicability -> scope check needed. Hooks enforcing the principle fire frequently but the "violations" are legitimate work -> principle may be too broad.

**Recovery:** Codification of a removal or scoping. Requires the same full process: evidence, review, human approval. Deletion criteria: contradicted by evidence, not referenced in a set period, superseded by a better pattern, failed trial. Expensive — this is precisely why the gate exists on the way in.

The build bible ingestion pipeline is one of the most important tools in the system. It's the deliberate, structured path from observation to codified principle. The review process filters false positives. Don't hoard observations or let them sit in memory indefinitely. Run them through the process. The alternative is that good insights stay trapped in Experience and never become Principles.

---

### Arrow 6: Escalation — Activity/Execution to Knowledge/Principles (bypass)

**Direction:** Activity/Execution -> Knowledge/Principles (bypassing Experience, Protocols, and the normal accumulation chain)
**Type:** Emergency bypass — "horizontal gene transfer"

**Mechanism:** When a live session produces an insight so significant it can't wait for the normal Capture -> Adaptation -> Codification chain, Escalation moves it directly from Activity to Principles in one step. The operator, during a live session, recognizes that a fundamental principle is wrong, missing, or needs immediate correction. They edit the build bible directly or instruct the AI to do so. This bypasses Experience accumulation entirely — there is no evidence trail, no multi-session confirmation, no trial period, no learnings queue. The operator's judgment substitutes for the entire maturation process.

The "horizontal gene transfer" analogy is precise. Normal evolutionary change (Capture -> Adaptation -> Codification) is vertical gene transfer — slow, tested, reliable. Escalation is horizontal gene transfer — fast, untested by the normal selection process, potentially transformative or destabilizing. In biology, horizontal gene transfer is how bacteria acquire antibiotic resistance overnight. In this system, Escalation is how a session-level realization becomes a system-level principle overnight.

**Timescale:** Immediate. The fastest arrow in the system.

**Consistency model:** Human judgment is the consistency check. The operator's judgment substitutes for the normal evidence accumulation and review process. This works when the operator's judgment is well-calibrated (they've seen enough of the system to know what's fundamental). It fails when the insight feels more urgent than it actually is — session recency bias can make a single observation feel like a principle.

**Authority required:** Human only. The AI cannot trigger Escalation autonomously. This is the most restricted arrow in the system. The restriction exists because Escalation bypasses every safeguard that protects Principles from noise.

**Concrete examples:**
1. Session exposes that a build bible rule produces the opposite of its intent -> operator recognizes the rule is actively harmful -> immediate build bible edit -> Constraint arrow cascades the fix -> next session no longer governed by the bad rule
2. An architectural debate produces a fundamental new model -> gap in the system's self-understanding -> immediate Atlas creation -> new governing document for architectural coherence
3. Critical security insight during a live session — a hook behavior creates an unintended exposure -> immediate build bible security section update -> Constraint arrow propagates -> hooks updated
4. A live session reveals that a previous architectural model was a category error -> immediate architectural correction -> Atlas updated to capture the correct model

**Failure mode: Over-use** — Escalation becomes the default path for any insight, bypassing the evidence accumulation that makes Principles trustworthy. Principles become reactive (reflecting whatever the most recent session produced) rather than deliberate (reflecting well-tested patterns across many sessions). The build bible loses architectural coherence and reads like a collection of session notes rather than distilled wisdom.

**How to detect failure:** Build bible update frequency from live sessions exceeds once per week -> probable over-use of Escalation. Principles added via Escalation don't survive their first encounter with a novel context -> evidence base was too thin. The build bible contains principles that trace to a single session rather than multiple projects -> Escalation may have been used where Capture -> Adaptation -> Codification was appropriate.

**Prevention:** Default to the normal chain. Capture the insight to memory or learnings queue. Let it accumulate evidence. Let it survive review. Reserve Escalation for genuine doctrinal emergencies — cases where waiting for the normal process would cause active harm in the intervening sessions. The test: "If this insight sits in the learnings queue for two weeks while accumulating evidence, will that delay cause real damage?" If yes, Escalate. If no, Capture.

---

### Arrow interaction analysis

#### The three feedback loops

**Fast loop** (days to weeks):
```
Execution -> Capture -> Adaptation -> (back to Execution via Configuration)
```
- Activity produces work -> Capture writes observations to Experience -> Adaptation tunes Protocols -> Configuration delivers updated Protocols to next session
- Authority: automatic to medium
- Consistency: eventual to causal
- Example: mistake occurs in session -> mistake documented -> pattern confirmed across 3 sessions -> hook added to prevent recurrence -> next session armed with new hook

**Slow loop** (months to quarters):
```
Adaptation -> Codification -> Constraint -> Configuration -> (back to Execution)
```
- Protocol matures through repeated use -> Codified as Principle -> Principle constrains Capability via Constraint -> Configuration delivers to next session
- Authority: human judgment required
- Consistency: strong
- Example: rules file rule ("no files > 500 lines") -> confirmed effective across 50+ sessions -> elevated to build bible principle -> config files updated -> all sessions governed -> enforcement hook enforces

**Emergency bypass** (immediate, rare):
```
Execution -> Escalation -> (Constraint -> Configuration)
```
- Live session insight -> direct Principle update -> cascades via Constraint to Capability -> Configuration delivers to next session
- Authority: human only
- Consistency: human judgment replaces process
- Example: fundamental architecture insight -> immediate build bible update -> derived copies updated -> all future sessions see new principle

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

- **Capture failure -> Adaptation failure.** If sessions don't write observations, there's nothing for Adaptation to work with. Protocols stagnate not because they're perfect, but because the system can't see its own problems.
- **Constraint failure -> Configuration failure.** If derived copies drift from Principles (Constraint breaks), Configuration delivers stale or wrong instructions (Configuration delivers wrong content).
- **Adaptation excess -> Codification excess.** If protocols accumulate without pruning (rigidity trap), the pool of "mature protocols" grows, creating pressure to codify more. False codification risk rises.
- **Escalation over-use -> Constraint overload.** Every Escalation creates a Constraint cascade (update all derived copies). Frequent Escalations mean frequent cascades, increasing the chance of missed updates (drift).

---

## Section 9: Cross-cutting concerns

### What can't be contained in one layer

The three-layer model handles WHAT and WHERE. Cross-cutting concerns handle HOW the system maintains itself over time. These concerns span multiple layers or govern the system's overall health — they can't be attributed to Knowledge, Capability, or Activity alone because they operate across all three. Without them, the architecture is an ideal that progressively diverges from reality. The map looks correct, but the territory has moved.

These concerns are the meta-architecture: the processes that keep the map accurate, the gaps known, and the system healthy. They are maintenance functions, not features. They don't add new capabilities — they prevent existing capabilities from decaying.

---

### Consistency maintenance

**The core problem:** Capability/Machinery contains derived copies of Knowledge/Principles content. Config files summarize build bible rules. Rules files implement build bible rules. Agent behaviors reflect architectural decisions. Hook behaviors enforce specific principles. When canonical sources change, derived copies must update. This obligation is permanent and pervasive — it exists as long as derived copies exist, and derived copies exist because the system needs compressed, context-appropriate versions of Principles at different layers.

**Current mechanism:** Manual tracing after a build bible update. When a change is made, the integrator (AI or human) must trace the enforcement chain and update derived copies. An enforcement traceability matrix maps each principle to its enforcement mechanisms — but tracing from principle to derived copy to update is manual and error-prone.

The update chain for a single build bible change:
```
Principle updated
  -> Identify affected config file tiers (Global? Project? Operations?)
  -> Update each config file
  -> Identify affected rules files
  -> Update each rules file
  -> Identify affected hook behaviors
  -> Update hook scripts if enforcement logic changed
  -> Identify affected skill behaviors
  -> Update skill docs if relevant
  -> Verify no derived copy contradicts the new principle
```

**Gap:** No automated mechanism exists in most systems to detect when derived copies have drifted from their canonical source. Drift is only detected when it produces visible symptoms (incorrect behavior in a session).

**Proposed: consistency sweep.** A systematic audit that reads the canonical source (build bible) and checks each derived copy for alignment. Reads each principle, finds every file that derives from it (via the enforcement traceability matrix), and verifies the derived content matches the canonical content. Discrepancies flagged in a report.

---

### Drift detection

The highest-priority architectural gap in most AI-augmented systems.

**What drift means:** Practice (what actually happens in Execution) diverges from Knowledge doctrine (what the architecture says should happen). This is different from consistency maintenance, which checks documents against documents. Drift detection checks behavior against documents. Examples:
- A review protocol says plans get adversarial review, but sessions consistently show plans executed without review
- A delegation protocol says 80% delegation rate, but actual rate has declined without anyone noticing
- A hook is supposed to fire on every write operation, but a configuration change silently disabled it

**Current state in most systems:** Partial mechanisms may exist — behavior analysis that checks delegation rate, hook stats surfaced at session end. But no mechanism compares session behavior against stated protocols at a granular level. The system measures what it can count (delegation rate, hook fires) but not what it should observe (protocol compliance, principle adherence).

**Conceptual approach — the audit agent (System 3*):** Beer's Viable System Model includes System 3* — the audit channel. While System 3 manages operational resources (Capability in this model), System 3* periodically samples operational activity to verify that what's claimed to happen actually happens. It's orthogonal to the normal management channel — it bypasses the usual reporting chain and checks reality directly.

In this framework, System 3* would be an autonomous agent that:
1. Samples session transcripts from the session index
2. Parses behavioral patterns against stated protocols in rules files and config files
3. Reports discrepancies with specifics: "The review protocol says plans get adversarial review. In the last 30 sessions, 12 involved planning. Of those, 4 (33%) show no evidence of review."
4. Provides trend lines: "Delegation rate was 72% last month, 65% this month. Declining."

**What it would require:**
- Session transcript analysis capability (a session index with full-text search provides the raw data)
- Protocol extraction from rules files and config files (parse stated behaviors into checkable assertions)
- Pattern matching between transcripts and assertions (did the session exhibit the stated behavior?)
- Discrepancy reporting with specifics and evidence

Drift detection is the most important unsolved problem in most AI-augmented systems. Without it, the architecture becomes a fiction — an idealized map of a territory that has moved. The build bible, the Atlas, config files, rules files — all of them describe what SHOULD happen. None of them verify what DOES happen, beyond coarse metrics. The audit agent / System 3* concept is the solution. It lives architecturally as a cross-cutting concern that samples Activity and compares against Knowledge.

---

### System boot sequence

What loads at session start, in order, and why it matters architecturally. This is the Configuration arrow (section 8, Arrow 2) made concrete.

| Order | What loads | Architectural role |
|------:|-----------|-------------------|
| 1 | Global config file | Universal behaviors: conductor mindset, questioning protocol, verification protocol, response format, security audit |
| 2 | Global rules files | Auto-loaded behavioral governance: code quality, build bible summary, review protocols, voice rules, integration rules |
| 3 | Project config file | Project-level: project-specific instructions, agent routing, integration references, skills |
| 4 | Project rules files | Any rules in the project's rules directory |
| 5 | Operations config file (if present) | Operation-level overrides for specific subsystems |
| 6 | Session memory | Persistent project memory: preferences, patterns, context from previous sessions |
| 7 | SessionStart hooks | Smart context loading, file hygiene check, memory system initialization |

After boot:
- Hooks arm across event types (PreToolUse, PostToolUse, SessionStart, UserPromptSubmit, PreCompact, SessionEnd)
- Skills become invocable but do NOT load until invoked — on-demand loading keeps boot weight low
- The Configuration arrow completes — this session's execution context is fixed

**Session state vs. session memory:** Each session starts fresh from the Configuration above. There is no persisted "previous session state" at the session level — that would be Execution layer memory, which is ephemeral by design. Continuity comes from Knowledge/Experience: session memory (persistent project memory), session index (searchable session history), state files (project state captured at session end), handoff documents (explicit handoff if created). A new session must reconstruct context from these Knowledge/Experience artifacts — it does not inherit the previous session's Execution state.

**The instruction volume problem:** A multi-tier config file hierarchy can total a thousand or more lines across all files, plus auto-loaded rules files. This is deliberate in a mature system — the scale serves a large agent roster and many hooks. But it creates a real tension: instruction volume may exceed the model's effective attention window. The mitigation path is deduplication and hierarchy clarification — not reducing the system's scope, but eliminating redundancy so every line earns its place.

---

### Health monitoring

The system monitors its own health through several mechanisms, each operating at a different cadence and granularity.

**Dashboards:** Visual dashboards showing system health, memory status, pipeline status. Use static HTML rebuilt on data change wherever possible — no server-side state, no heavy frameworks. Reserve live servers for APIs that need them.

**Backup verification:** Regular automated backups of your codebase and databases. Corruption-safe database backups (e.g., `sqlite3 .backup`). Alerting on failure. Key gap in most systems: no automated verification of backup integrity beyond success/failure codes. Untested backups are decorative.

**Process monitoring:** A health monitor for scheduled jobs (auto-restart on crash for critical services). Key gap: monitoring for crash is different from monitoring for silent failure (job is registered but not executing).

**Alerting:** Critical failure notifications delivered to the operator. Configured for: backup failure, monitoring script failure, threshold breaches.

**Hook statistics:** Track hook firing at session end — fire counts, failure counts, timing data. Enables periodic hook event review for Adaptation decisions.

**Behavior audit:** Analyze sessions for delegation rate (vs. target), response compliance, session documentation rate, and file organization hygiene. Run on-demand or scheduled, results feed periodic metrics review.

**Architecture sweep (arch-sweep):** A scheduled job that reads the Atlas and system inventory, verifies every file path exists, checks component counts against reality, and flags drift between the Atlas description and filesystem reality. This is the primary anti-rot mechanism for the Atlas itself — without it, the Atlas degrades into fiction as the system evolves.

---

### Pace-layer table

Functional classification (Knowledge / Capability / Activity) is primary — it determines where something lives and how it's governed. Temporal classification (how frequently something changes) is secondary — it informs maintenance cadence, not layer membership. A component's pace layer tells you how often to audit it and how much scrutiny a change deserves.

| Component type | Layer | Sub-layer | Change frequency | Why it changes |
|----------------|-------|-----------|------------------|----------------|
| Critical rules | Knowledge | Principles | Weeks-months | High-evidence codification required |
| Patterns | Knowledge | Principles | Weeks-months | Pattern confirmation across multiple sessions |
| Architectural decisions | Knowledge | Principles | Months | Deliberate, well-considered, hard to reverse |
| Atlas | Knowledge | Principles | Months | Structural model changes are rare; content accuracy verified by arch-sweep |
| Config files (global) | Capability | Machinery | Weeks-months | Derived from build bible; updates when build bible updates via Constraint arrow |
| Config files (project) | Capability | Machinery | Weeks | Project-specific instructions evolve with project needs |
| Rules files | Capability | Protocols | Days-weeks | Adaptation arrow fires more frequently than Codification |
| Agent definitions | Capability | Machinery | Weeks-months | Stable once battle-tested; changes driven by new requirements |
| Hooks | Capability | Protocols | Days-weeks | Adaptation-driven; easy to create but accumulation is a risk |
| Skills | Capability | Machinery | Days-weeks | Easy to add and refine; on-demand loading means low cost of expansion |
| Slash commands | Capability | Machinery | Weeks | User-invoked workflows; stable once proven |
| Scheduled jobs | Capability | Machinery | Weeks-months | Infrastructure; changes are deliberate and tested |
| Scripts | Capability | Machinery | Days-weeks | Actively developed operational code |
| Session memory | Knowledge | Experience | Daily-weekly | Frequent session captures; groomed periodically |
| Spaced-repetition memory | Knowledge | Experience | Daily | Review cycle; continuous capture |
| Session index | Knowledge | Experience | Daily | Every session adds entries |
| Learnings queue | Knowledge | Experience | Weekly | Accumulated observations awaiting review |
| Decision journals / task notes | Knowledge | Experience | Per-task | Written during complex decisions |
| Databases (as data stores) | Knowledge | Experience | Daily | Continuous writes from session activity |

---

### Synchronization cadence

The maintenance schedule for the entire system. Each cadence corresponds to specific arrows and mechanisms.

| Cadence | What happens | Arrow served | Mechanism |
|---------|-------------|-------------|-----------|
| **Per-session** | Configuration loads (config files, rules, hooks arm) | Configuration | Automatic boot sequence |
| **Per-session** | Capture fires (session index, topic capture, memory curation) | Capture | SessionEnd hooks, PreCompact hooks |
| **Daily** | Mid-session captures (mistake docs, memory updates, task notes) | Capture | AI-initiated + operator-initiated |
| **Daily** | Arch-sweep runs (Atlas accuracy check, file path verification, component counts) | Cross-cutting | Scheduled job |
| **Daily** | Backup verification (codebase + databases) | Cross-cutting | Scheduled job + alerting on failure |
| **Continuous** | Adaptation fires on every meaningful signal (correction, insight, hook alert) | Adaptation | In-session, immediate |
| **Weekly** | Scheduled adaptation review (learnings queue, memory grooming) — backstop for signals that didn't trigger immediate response | Adaptation | Manual + review tool + scheduled trigger |
| **Weekly** | Behavior audit (delegation rate, corrections, compliance) | Cross-cutting | Scheduled job |
| **Monthly** | Build bible review (evidence citations, cross-references, structural integrity, version bump) | Codification | Manual + batch review |
| **Monthly** | Atlas structural review (does the model still match reality?) | Cross-cutting | Manual + arch-sweep results |
| **On-demand** | Post-project retrospective (extract learnings, update debt table) | Capture + Adaptation | Manual after project completion |
| **On-demand** | External knowledge ingestion (articles, repos, insights) | Codification | Operator-triggered |
| **On-demand** | Escalation (doctrinal emergency, immediate build bible edit) | Escalation | Operator-only, reserved for genuine emergencies |

This cadence is the maintenance heartbeat. Daily captures are the minimum viable cadence — miss them and the system stops learning. Weekly is the primary active cadence: adaptation reviews, behavior audit, hook review. Monthly is where structural deliberation happens — build bible reviews, Atlas accuracy audits. The most common failure mode is skipping the weekly cadences when things feel smooth. Smooth-running systems that skip maintenance accumulate quiet debt.

---

### Known architectural gaps

A summary of gap types commonly found in AI-augmented systems, ordered by impact. These are distinct from a build bible's debt inventory, which focuses on implementation debt. These gaps are structural — they represent missing pieces of the architecture itself.

| Gap | Impact | Where it appears | Mitigation path |
|-----|--------|-----------------|-----------------|
| Partial drift detection (System 3*) | Behavioral drift detected at coarse level (delegation rate); protocol-level compliance not automated | Section 9: Drift detection | Build automated protocol comparison against config files/rules files |
| No consistency sweep | Derived copies drift from canonical sources undetected | Section 9: Consistency maintenance | Build automated comparison of build bible -> config files -> rules files |
| No arch-sweep | Atlas accuracy degrades as system evolves | Section 9: Health monitoring | Build scheduled job verifying file paths and component counts |
| Capture fragility | SessionEnd hook bottleneck | Section 8: Arrow 3 | Move heavy hooks to async scheduled jobs |
| Config file hierarchy drift | Duplications and contradictions across tiers | Section 8: Arrow 1 | Deduplicate, resolve contradictions, enforce single-source per instruction |
| Adaptation without pruning | Hooks and rules accumulate without review | Section 8: Arrow 4 | Periodic protocol audit; consolidate overlapping mechanisms |
| No Escalation frequency monitoring | Over-use of Escalation undetected | Section 8: Arrow 6 | Track build bible edits from live sessions; alert if frequency exceeds weekly |

---

## Section 11: Evolution protocol

The Atlas must evolve as the system evolves. New components are added, old ones deprecated, counts drift, paths change, and occasionally the architectural model itself needs refinement. A governing document that does not govern its own evolution rots — it becomes an outdated description of a system that has moved on. The moment the Atlas stops reflecting reality, every AI session that loads it inherits a wrong map.

The protocol has two modes, calibrated to the magnitude of the change. Structural changes to the model, arrows, or precedence require the same rigor as build bible structural changes — adversarial review, stress-testing, human approval. Maintenance-level updates (new component, corrected path, updated count) use a lighter grooming pass. This prevents two failure modes: over-governance (requiring full review for a corrected file path) and under-governance (treating fundamental architectural changes as routine maintenance).

---

### Structural changes — full evolution protocol

**What counts as structural:** Model changes (new layer, layer removed, layer renamed, fundamental reclassification of what belongs where), new arrow type, precedence hierarchy changes, new sub-layer, new or modified placement test, fundamental revision to existing sections (rewriting the model narrative, changing the arrow semantics).

**Process:**

1. **Trigger:** Architectural debate, fundamental component reclassification that existing tests cannot resolve, new relationship type discovered in practice that no existing arrow captures, or monthly structural review identifying model drift.
2. **Adversarial debate:** Minimum 7 personas from relevant domains. Real debate — personas argue against each other, not parallel independent reviews. The debate must produce a concrete proposal with rationale.
3. **Stress-test review** of the resulting proposal. Critique the proposal, defend against the critiques, fold surviving critiques into a revised proposal.
4. **Human approval:** Required for all structural changes. Present the revised proposal with the review record so the operator can see what was considered and rejected.
5. **Update** the affected Atlas section(s). Integrate the change as if the Atlas was always written this way — do not append.
6. **Update the auto-loaded session reference** if the section index changes.
7. **Version bump:** MINOR for new section or new arrow type. MAJOR for fundamental model change (new layer, layer removed, model redefinition).
8. **Update header:** `<!-- Atlas v[version] | Last updated: [date] -->`

**Authority:** Human approval required. The AI cannot make structural changes autonomously.

**Why this rigor:** The Atlas is a normative document — it constrains how the AI understands the entire system. A wrong structural change produces wrong understanding, which produces wrong decisions across every session that loads it. The blast radius is the entire system.

### Grooming pass — lightweight maintenance

**What counts as grooming:** New component added (register in appropriate section), file path changed, component count corrected, section reference updated, new agent or skill registered, line count refreshed, arch-sweep discrepancy resolved, cross-reference added for a newly discovered relationship.

**Process:**

1. **Trigger:** Component registration command, arch-sweep discrepancy report, direct observation that Atlas is stale, or new component creation in any layer.
2. **Apply placement tests** (consult-vs-fire, authority-record, stability-as-property) from section 1 to determine correct layer and sub-layer.
3. **Write the entry** as if the component always existed in the document. Integrate, do not append to the bottom of a section. Find the right place in the existing structure.
4. **Check cross-references:** Does the new entry create relationships that need documenting?
5. **Verify inventory:** Does the system inventory also need updating? If the component is new, it should appear in the inventory first.
6. **Version bump:** PATCH.
7. **No stress-test needed** for grooming passes.

**Authority:** The AI can execute grooming passes autonomously for count corrections, path corrections, and line count refreshes. New component registration: the AI executes without approval for Machinery components (agents, skills, commands, hooks, scheduled jobs, database entries). Human confirmation required for Protocols components (new rules files, new protocol documents, new skills that change behavioral expectations) because these carry higher risk of unintended behavioral changes.

**The integration principle:** Grooming passes integrate new entries as if they were always part of the document. Do not add a "Recently added" section. Do not append to the bottom of a table. Find the correct position in the existing structure, insert the entry, and update any counts or cross-references affected by the addition.

### Version control

| Version component | What triggers it | Frequency |
|-------------------|-----------------|-----------|
| MAJOR (X.0.0) | Model itself changes — new layer, layer removed, fundamental reclassification, arrow semantics redefined | Rare — architectural paradigm shifts |
| MINOR (x.Y.0) | New section added, significant content expansion, new arrow type, new placement test | Occasional — meaningful structural additions |
| PATCH (x.y.Z) | Grooming pass — path corrections, count updates, new component registration, cross-reference additions | Frequent — routine maintenance |

Version lives in the document header: `<!-- Atlas v[version] | Last updated: [date] -->`

### Review cadence

- **Monthly:** Structural review — does the model still accurately describe the system? Are all arrows still accurate? Is the precedence hierarchy complete? Are the placement tests sufficient? Has any component drifted to a different layer?
- **On-demand:** Grooming pass — triggered by component registration, arch-sweep discrepancy report, or direct observation of staleness.
- **Target:** The Atlas should never be more than 30 days out of date on component counts and file paths. An arch-sweep enforces this by flagging discrepancies.

### Anti-rot mechanisms

The Atlas stays accurate through five reinforcing mechanisms:

1. **Auto-loaded session reference** — a summary loaded every session. The AI always knows the Atlas exists and how to navigate it.
2. **Component registration command** — the primary contribution path. Makes updating the Atlas the natural next step after creating a component. Accepts a description, applies placement tests, writes the entry, checks cross-references.
3. **PostToolUse hook on Atlas edits** — validates file paths referenced in the edit actually exist, checks entry format matches the section's table structure, confirms integration (not appending), asks whether the inventory also needs updating.
4. **Nightly arch-sweep** — reads the Atlas and inventory, verifies every file path exists, checks component counts against reality, reports discrepancies. This is the primary anti-rot mechanism for the Atlas itself — without it, the Atlas degrades into fiction as the system evolves.
5. **Evolution protocol** (this section) — defines when and how structural changes happen. Prevents both over-governance and under-governance by matching rigor to magnitude.

These mechanisms form a closed loop: the arch-sweep catches silent drift, the registration command closes the gap when new components appear, the PostToolUse hook validates edits as they happen, the session reference ensures the AI remembers the Atlas exists, and the evolution protocol governs how structural changes flow through review.

The Atlas is only useful if it is accurate. The anti-rot mechanisms above are the primary defense. Treat Atlas updates as a first-class task, not an afterthought. If the Atlas and the system inventory ever disagree, the inventory is canonical for component details, the Atlas is canonical for layer placement — resolve the discrepancy, do not pick one and ignore the other.

---

*End of Atlas framework.*
