# Atlas applied example

> **Companion to:** [`atlas-framework.md`](atlas-framework.md)
> **Read the framework first.** This document assumes familiarity with the KCA model, the six arrows, the placement tests, and the precedence hierarchy defined in the framework.

---

## What this file is

This is a real-world example of how one AI-augmented consulting practice organized their entire system using the KCA (Knowledge-Capability-Activity) framework. The names, paths, and client references have been anonymized, but the **structure, reasoning, and categorization logic are exactly as they were built** -- because that is the educational value.

The system described here is a solo operator running a consulting practice with extensive AI automation: 47 agent definitions, 60+ scheduled background processes, 6 databases, 300+ scripts, 100+ skills, and 35+ hooks across 7 event types. It evolved over thousands of sessions across dozens of client projects. The Atlas was built to make the system knowable -- to answer "where does X live and why is it there?" for any component.

**How to use this example:**
1. Read `atlas-framework.md` first for the theory
2. Read this file to see how theory maps to practice
3. Build your own applied atlas for your system -- your components, your layers, your arrows

The value is not in copying this specific system. The value is in seeing **how the placement tests, arrows, and layer boundaries work in practice** -- so you can apply them to your own.

---

## Table of contents

- [Section 3: Knowledge / Principles](#section-3-knowledge--principles) -- The normative layer: rules, patterns, anti-patterns, enforcement
- [Section 4: Knowledge / Experience](#section-4-knowledge--experience) -- The empirical layer: memory, sessions, mistakes, learnings
- [Section 5: Capability / Machinery](#section-5-capability--machinery) -- The standing apparatus: config, agents, databases, scripts
- [Section 6: Capability / Protocols](#section-6-capability--protocols) -- The behavioral governance: SOPs, sweeps, hooks, skills
- [Section 7: Activity / Execution](#section-7-activity--execution) -- The ephemeral layer: sessions, running processes, live work
- [Section 10: Document index](#section-10-document-index) -- Master index of every document by layer

---

## Section 3: Knowledge / Principles

Normative knowledge prescribes what SHOULD happen. It is the system's ought -- the distilled, adversarially reviewed, evidence-backed rules that govern how every piece of downstream machinery behaves. Normative knowledge does not describe the world as it is; it declares how this system operates. When a CLAUDE.md directive tells an agent "never skip the red phase of TDD," that instruction traces back to a Principle. When a hook blocks solo execution after three strikes, the blocking rule traces back to a Principle. Principles are the authority layer -- everything downstream inherits from them.

Why does normative knowledge need its own sub-layer, separate from empirical knowledge? Because authority demands stability. Empirical knowledge -- memory reviews, MEMORY.md entries, session indices, mistake documentation -- changes constantly. A new session generates new experience every few hours. But a build bible's critical rules change on the order of months, through a deliberate multi-step codification process with steelman review and human sign-off. If Principles and Experience shared a layer, the most stable things in the system (rules that have survived adversarial review across dozens of projects) would coexist with the most volatile (a mistake documented ten minutes ago). The result is authority confusion: an agent consulting the layer cannot distinguish "this is a proven rule" from "this is a recent observation." Separate sub-layers make the distinction structural, not interpretive.

Everything downstream is shaped by what lives here. The Constraint arrow flows from Knowledge/Principles to all of Capability -- machinery and protocols alike. Every CLAUDE.md directive, every rules file, every hook behavior, every agent definition, every skill's enforcement logic is downstream of Principles. When a Principle changes, the blast radius is system-wide. This is not a defect; it is the design. Principles are the slow layer that stabilizes the fast ones. A system where the fast layers can override the slow ones drifts. A system where the slow layers constrain the fast ones converges. The infrequency of Principle changes is the feature, not the limitation.

### The build bible

**Location:** `build-bible.md`
**Scale:** ~1,900 lines, sections 0-10 plus changelog
**Authority:** The PRIMARY document of the Principles sub-layer. Everything else in Principles either feeds into it (via Codification) or is indexed by it.

The bible is structured as principles first (section 1), patterns second (section 2), architecture third (section 3), playbook fourth (section 4), agent system fifth (section 5), anti-patterns sixth (section 6), operations seventh (section 7), evolution eighth (section 8), debt ninth (section 9), and credits tenth (section 10). The ordering is deliberate: principles constrain patterns, patterns implement principles, and everything else is downstream.

#### Critical rules (14)

These are the non-negotiable constraints on all work. Lower-numbered principles override higher when they conflict.

| # | Rule | Core constraint |
|---|------|-----------------|
| 1.1 | Orchestrate, don't execute | The conductor coordinates and quality-controls -- never writes code, drafts copy, or does research directly |
| 1.2 | QA the design before writing code | Run adversarial review on the design before any implementation begins |
| 1.3 | Test first, then build (TDD) | RED then GREEN then REFACTOR -- the red phase proves the test has teeth |
| 1.4 | Simplicity wins | When a feature isn't earning its complexity, delete it -- don't optimize or refactor |
| 1.5 | Single source of truth | Every data domain has ONE canonical store -- no sync jobs, no drift |
| 1.6 | Config drives behavior, code stays generic | Scale by adding data files, not code -- new campaign = JSON entry, not source modification |
| 1.7 | Checkpoint gates with explicit failure plans | Every multi-step process has measurable checkpoints with predetermined failure response |
| 1.8 | Prevent, don't recover | Validate before attempting -- layered pre-validation so bad data never reaches the expensive operation |
| 1.9 | Atomic operations for crash safety | Write to temp file, then atomic rename -- operations complete fully or don't happen at all |
| 1.10 | Document decisions when they're fresh | Capture the WHY during the work, not after -- rationale is clearest at the moment of decision |
| 1.11 | Measure for self-correction, not vanity | Every metric must trigger a specific action at a specific threshold -- if it doesn't change behavior, delete it |
| 1.12 | Observe everything, alert on what matters | Every service gets structured logging, health checks, and tiered alerting (CRITICAL/IMPORTANT/ADVISORY) |
| 1.13 | Test the unhappy path first | Error paths, edge cases, and failure modes get tested before happy paths |
| 1.14 | Speed hides debt | Fast shipping without verification creates invisible technical debt -- velocity is not progress |

The rules interact in named chains: Planning chain (1.2 -> 1.7 -> 1.3), Simplicity chain (1.4 -> 1.6 -> 1.5), Reliability chain (1.8 -> 1.9 -> 1.11), Knowledge chain (1.10 -> 1.5 -> 1.11). The override rule is simple: lower numbers win conflicts.

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

Named failure modes to hunt and destroy. Each traces to the principle it violates.

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

Where rules say "do X in general," architectural decisions say "in this system, X means Y specifically." Architectural decisions are scoped applications of principles -- they bind abstract rules to concrete system choices.

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

The governing principle is direct: unenforced principles are aspirations, not architecture. The gap between stated principle and actual behavior is drift, and drift compounds silently.

The system enforces principles through five complementary mechanisms:

| Mechanism | Components | Enforcement level |
|-----------|-----------|-------------------|
| Auto-loaded rules | `~/.claude/rules/*.md` (5 files) | Passive -- relies on agent compliance |
| PreToolUse hooks | delegation-check, bloat-watcher, questioning-nudge, skill-usage-tracker, component-audit | Active -- executes on every matching tool call |
| On-demand crusades | `/church` orchestrator with 14 crusade types | Manual -- user-triggered deep scans |
| On-demand skills | 36+ skills across two locations | Manual -- loaded by conductor or user |
| Slash commands | ~65 commands | Manual -- user-triggered workflows |

The enforcement traceability matrix maps each of the 14 rules to its enforcement mechanism(s) and rates the strength. Three enforcement gaps are documented in the debt inventory: principles 1.6 (config-driven), 1.11 (actionable metrics), and the cluster of 1.5/1.9/1.10 that rely entirely on manual review with no automated enforcement.

### Universal learnings

Cross-project insights promoted from individual project learnings-queue files. These represent battle-tested observations that have been confirmed across multiple clients and contexts.

- **Location:** `~/system/universal-learnings.md`
- **Placement:** Sits in the Experience sub-layer as a source file; promoted entries become candidates for Principles via Codification
- **Promotion path:** Session insight -> learnings queue -> universal learnings -> Bible (via a codification command)
- **Threshold:** An observation needs confirmation across 3+ projects before it qualifies for Bible promotion

Universal learnings occupy a liminal position between Experience and Principles. The file itself is empirical (it records observations), but its promoted contents flow into normative rules. It is the staging area for Codification -- the last waypoint before an observation becomes a rule.

### Bible evolution protocol

The process by which the bible itself updates. The bible is a living document that improves through a disciplined 7-step process, not casual edits.

**The 7-step update process:**
1. Observation -- pattern noticed across 3+ sessions or projects
2. Proposal -- written as specific rule with enforcement mechanism and evidence
3. Steelman -- critiqued adversarially: is this actually needed?
4. Trial -- run for 2 weeks as advisory (no blocking enforcement)
5. Measurement -- check if the rule improved outcomes
6. Promotion -- if proven, move to appropriate tier with appropriate enforcement level
7. Documentation -- update the bible with rule, evidence, and rationale

**Entry gate:** A codification command ingests external sources, classifies against existing bible content, and proposes additions with steelman review. This is the Codification arrow made executable.

**Deletion criteria:** Not referenced in 6 months, contradicted by evidence, superseded by better pattern, or failed trial period.

**Version control:** Semantic versioning (MAJOR.MINOR.PATCH). Structure changes bump MAJOR; new principles/patterns bump MINOR; evidence updates bump PATCH.

**Review cadence:** Weekly learning review, monthly full bible review, on-demand after project completions. The review cadence is separate from the update cadence -- updates happen at AI/learning speed (potentially daily), but structural review happens monthly.

### Internal structure -- how Principles components relate

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

- **Who updates:** The operator (final authority) + Claude via a codification command (proposes, executes after approval)
- **What gates changes:** Steelman protocol for all changes; adversarial review for structural changes; human sign-off for all promotions
- **Update cadence:** Not on a fixed schedule. Updates happen at AI/learning speed -- potentially daily. The review cadence (weekly learnings, monthly structural) is separate from the update cadence.
- **Cascade constraint:** Bible updates trigger downstream cascade. After any Bible change, trace the enforcement chain and update derived copies (CLAUDE.md, rules files, verification protocols). The enforcement traceability matrix maps the blast radius.

> **Note:** Changes here cascade everywhere. Every Bible rule change potentially requires updates to CLAUDE.md, rules files, and verification protocols. Use the enforcement traceability matrix to understand the blast radius before modifying anything in Principles.

---

## Section 4: Knowledge / Experience

Empirical knowledge describes what HAS happened. Sessions completed, mistakes made, insights captured, patterns observed, decisions recorded, debt inventoried. It does not prescribe behavior; it records reality. When a memory system surfaces a flashcard about a client preference, that is Experience. When a session index entry records "we decided to use SQLite on 2026-01-15," that is Experience. When a mistake document captures "agent X hallucinated a file path because we didn't validate it," that is Experience. None of these observations are rules -- they are the raw material from which rules are eventually forged.

Why Experience and Principles need separate homes. The capacity they serve is fundamentally different. Principles is normative: stable, high-authority, changes slowly through deliberate codification. Experience is empirical: volatile, low-authority-by-itself, changes constantly as new sessions generate new observations. Mixing them produces two dangerous problems. First, noise: the constant churn of empirical updates drowns out the signal of deliberate principle changes. A Principle added after months of adversarial review sits next to a MEMORY.md note from yesterday's session -- the authority gap is invisible. Second, confusion: "is this a rule or an observation?" becomes unanswerable without checking the provenance of every item. Separate sub-layers make the distinction structural: if it lives in Principles, it prescribes. If it lives in Experience, it describes.

The capacitor property. Experience is the accumulation buffer between fast ephemeral Activity and slow normative Principles. Like a capacitor, it stores charge and releases it. The Capture arrow (Activity -> Experience) accumulates observations over time -- low authority, automatic, eventually consistent, on the order of minutes-to-hours. The Adaptation arrow (Experience -> Protocols) and the Codification arrow (Experience -> Principles) discharge that accumulated charge upward when patterns emerge. But the charge does not flow continuously. It accumulates, then discharges through deliberate, authority-gated channels. Codification requires 3+ project confirmation, steelman review, and human sign-off. This asymmetry -- easy to write into Experience, hard to promote out of it -- is the fundamental quality gate of the knowledge system. Without it, every one-off observation becomes a system-wide rule.

### Spaced repetition memory system

The most systematic component of Experience. A spaced repetition system manages codified knowledge as flashcard decks, surfacing cards at intervals calibrated to retention.

- **Location:** `~/projects/memory-system/`
- **Scale:** 102 features, 2,000+ tests, Python 3.11+
- **Algorithm:** FSRS (Free Spaced Repetition Scheduler) -- calculates optimal review intervals based on the operator's actual retention curve
- **What it stores:** Flashcard decks covering principles, patterns, client facts, system knowledge, domain expertise
- **Heartbeat role:** Reviews are the most systematic way experience is retained and surfaced. The FSRS cycle is the pulse of the Experience layer -- cards not reviewed drift to unreviewed, which is functionally equivalent to knowledge lost. Each individual review is small; the compounding effect across hundreds of cards is the difference between a system that remembers and one that forgets.

### MEMORY.md -- session memory

Auto-loaded, per-project memory that persists across conversations within the same project scope.

- **Location pattern:** `~/.claude/projects/[project-id]/memory/MEMORY.md`
- **What it stores:** Patterns confirmed across sessions, user preferences, project context, active state (known bugs, in-flight decisions)
- **Update mechanism:** Claude edits this file during sessions when directed or when capturing important context that would otherwise be lost at session end
- **Scope:** Per-project. Global learnings are promoted to universal learnings, not duplicated into every project's MEMORY.md
- **Authority level:** Low -- MEMORY.md records observations, not rules. An entry like "the operator prefers adversarial debates for brainstorming" is an observed preference, not a system principle. It becomes a principle only if it survives the Codification path.

### Session index -- searchable session history

A structured log of past Claude Code sessions with metadata, making past work searchable.

- **Scale:** 2,900+ sessions indexed
- **Metadata per session:** Name, date, tags, summary, git state, duration, tools used
- **Query interface:** CLI via Bash -- search for keywords, read conversation context, view analytics
- **What it enables:** Context continuity across sessions ("when did we discuss X?"), decision archaeology ("what was the decision on Y?"), pattern detection ("how often do we use agent Z?")
- **Why it matters:** Without a session index, every new session starts from zero context. The session index transforms thousands of past sessions from inaccessible history into queryable institutional memory.

### Decision journal

Records of key decisions -- what was decided, what alternatives were considered, and why.

- **Format:** `context.md` + `decisions.md` + `open-questions.md` per complex task, in `_notes/[task-name]/`
- **When created:** Initialized at task start for multi-session tasks, complex decisions, high-stakes work, or anything requiring an audit trail
- **Why it matters:** Decisions are expensive to re-derive. "Why did we decide X?" should be answerable by reading a file, not by re-running the analysis that produced the decision. The decision journal is the canonical record of rationale -- consulted whenever a past decision is questioned or revisited.

### Learnings queues -- per-project observation capture

The frontline capture mechanism for observations worth recording but not yet processed.

- **Location pattern:** `[project]/learnings-queue.md`
- **What they store:** Observations, patterns noticed, potential improvements, session insights -- anything worth capturing but not yet reviewed or promoted
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
                     Build bible          (Codification arrow, high-threshold, human judgment)
```

- **Review cadence:** Weekly adaptation review. A scheduled process surfaces pending items.

### Mistake documentation

The system for capturing failures so they do not repeat.

- **Mechanism:** `memory-ts add --type mistake --content "[What] | [Why] | [How]" --intent high --tags [project],[domain],[type],[severity]`
- **What is captured:** What happened, why it happened, how to prevent it -- structured as three fields to enforce completeness
- **Downstream effect:** Mistakes captured here feed the Adaptation arrow (Protocols adjust to prevent recurrence) and, for recurring patterns, the Codification arrow (repeated mistakes become named anti-patterns in the bible)
- **Critical property:** Mistakes that are not captured repeat. The documentation overhead per mistake is small; the cost of repeating it is large.

### Bible changelog

The version history of the Principles sub-layer itself.

- **Location:** `~/system/bible-changelog.md`
- **What it records:** Every bible change -- version, date, what changed, why, what source prompted it, how to evaluate whether the change improved outcomes
- **Why it matters:** Understanding WHY a principle exists often requires reading its changelog entry. The current state of the bible tells you what the rules are; the changelog tells you how they got that way. When someone proposes changing a rule, the changelog reveals the original context and evidence -- preventing the system from re-litigating settled decisions.

### Debt inventory

Known technical debt and architectural gaps, making the invisible visible.

- **Source:** Bible section 9
- **Scale:** 15 items -- 2 critical, 3 high, 7 medium, 3 low
- **What it records:** Severity, issue description, impact assessment, mitigation status
- **Purpose:** Technical debt that is not inventoried accumulates silently. The debt inventory makes it visible, prioritized, and reviewable. Items are resolved, re-prioritized, or added during each review cycle. Resolved items move to a "Resolved" section -- they are not deleted, because the resolution history is itself valuable experience.

### System inventory

The canonical record of what exists in the system.

- **Location:** `system-inventory.md`
- **Scale:** ~41KB, covers all system components with a "why it exists" column tracing each to a bible principle
- **Placement rationale:** The inventory is empirical -- it IS the canonical record of what exists. This placement may surprise those who expect it to live in Capability. But the authority-record test is clear: the inventory is the canonical source of truth about the system's composition. You consult the inventory to understand what exists; you fire Capability components to do work. Consulting versus firing is the Knowledge/Capability boundary.
- **Update mechanism:** Updated in parallel with any system change that adds, removes, or modifies components

### Internal flows

**The FSRS consolidation cycle.** The spaced repetition system manages review intervals. Cards surface when due. The operator reviews. Retention data updates scheduling. The next review interval is calculated from this session's performance. This is the most systematic part of Experience -- everything else in this sub-layer is more ad-hoc. The FSRS cycle runs daily; other Experience components update opportunistically.

**The memory promotion pipeline.** Observations flow upward through progressively higher authority gates:

```
Session insight ----Capture arrow----> MEMORY.md
    (immediate, low-threshold, automatic)

MEMORY.md pattern ----Review----> Universal learnings
    (weekly, medium-threshold, requires pattern across sessions)

Universal learning ----Codification arrow----> Build bible
    (months, high-threshold, 3+ project confirmation,
     steelman review, human judgment required)
```

Each stage has higher entry requirements than the last. A session insight needs only to be worth recording. A MEMORY.md pattern needs confirmation across multiple sessions. A universal learning needs cross-project validation. A bible entry needs adversarial review and human sign-off. This progressive filtering is the system's quality gate -- it prevents premature codification while ensuring genuinely proven patterns eventually reach Principles.

**Session index as experience database.** Sessions leave structured records with metadata. The CLI queries them. Patterns across sessions become visible in aggregate -- which agents are used most, which clients generate the most sessions, which topics recur. This aggregate view feeds both Adaptation (Protocol adjustments) and Codification (Principle candidates).

### Relationship to Principles

Experience feeds Principles via the Codification arrow -- but this is a deliberate, human-judgment-gated channel, not automatic sync. Experience does NOT automatically update Principles. The flow is: observe pattern in Experience -> confirm via multiple instances across projects -> propose codification -> steelman review -> the operator approves -> bible updates -> Constraint arrow propagates downstream to Capability.

This gate exists because premature codification is a serious failure mode. An observation after one session is not a principle. A pattern confirmed across dozens of sessions, in multiple contexts, under adversarial review -- that is a candidate for codification. The contribution rules require: used successfully in 3+ projects, confidence score >0.8, survived steelman review, has a specific enforcement mechanism, and human review approved.

The asymmetry is intentional. Writing to Experience is cheap, fast, and automatic (Capture arrow). Promoting from Experience to Principles is expensive, slow, and deliberate (Codification arrow). The system biases toward capturing too much experience (low threshold) and promoting too little of it (high threshold). This is correct. Lost experience can often be re-observed; a false principle actively damages the system by constraining downstream machinery based on insufficient evidence.

### Relationship to Protocols

Experience also feeds Protocols via the Adaptation arrow. This channel is faster than Codification (weeks, not months) and lower-authority (Protocol adjustments, not Principle creation). When Experience reveals that a workflow consistently produces poor results, Protocols adapt -- a new verification step is added, a hook is tightened, an agent prompt is refined. This does not require a bible change; it requires a machinery or protocol adjustment informed by empirical evidence.

The key distinction: Codification creates new rules (normative). Adaptation adjusts existing workflows (procedural). Both consume Experience. They differ in authority, latency, and scope.

> **Note:** The FSRS cycle is the heartbeat of this layer. Spaced repetition reviews are the most systematic way experience accumulates and surfaces. Skipping reviews is like skipping heartbeats -- each one is small, but the compounding effect is memory decay and knowledge loss. The promotion pipeline's progressive thresholds exist specifically to prevent premature codification -- resist the urge to shortcut it.

---

## Section 5: Capability / Machinery

Machinery is the standing apparatus -- the durable components that configure each execution session. They exist before any session begins and persist after every session ends. A CLAUDE.md file, an agent definition, a database schema, a LaunchAgent plist: these are not ephemeral artifacts of work-in-progress. They are the infrastructure that work runs on. They load and fire. They shape what Claude can do before a single prompt is typed.

Machinery does not live in Knowledge, even though it may contain knowledge. CLAUDE.md files contain summaries of bible principles, but their purpose is not to inform -- it is to configure behavior. Agent definitions encode expertise, but their purpose is not to teach -- it is to provide a ready-made specialist for delegation. The test is consult-vs-fire: if it loads automatically and shapes execution, it is Machinery.

Machinery is distinct from Protocols, its sibling sub-layer in Capability. Both configure the system, but they configure different things. Machinery is structural: the agents, the databases, the background processes, the scripts -- the WHAT of the system. Protocols are behavioral: the rules, the hooks, the skills, the sweeps -- the HOW of the system. An agent definition (Machinery) defines what the copywriter team can do. A voice enforcement rule (Protocol) specifies how they must behave when drafting as the operator.

The "mature machinery" distinction matters. Agent definitions (47 of them) are Machinery, not Principles, even though they are stable and important. Stability-as-property: they are stable because they have been battle-tested across hundreds of sessions, not because they are foundational normative knowledge. When a new agent is added, it enters through Adaptation (experience informs the definition, the definition gets refined through use) before reaching its current form. Agent definitions ARE the refined output of that process -- they are codified practice, not first principles.

Machinery carries a derived-copies obligation. CLAUDE.md contains derived summaries of bible principles. Rules files implement bible rules in session-loadable form. This creates a maintenance chain: when canonical sources in Knowledge/Principles change, derived copies in Machinery must update. A nightly architecture sweep catches drift. The enforcement traceability matrix maps each principle to its enforcement mechanisms, making the dependency chain explicit.

### CLAUDE.md -- the 4-tier instruction system

The CLAUDE.md system is the primary configuration vector for Claude Code behavior. Four tiers load in sequence, each more specific than the last. Together they total approximately 1,185 lines of standing instructions active in every session.

**Loading order and file paths:**

| Tier | File | Scope | Approx. lines | What it governs |
|------|------|-------|------:|-----------------|
| 1. Global | `~/.claude/CLAUDE.md` | All projects, all sessions | ~200 | Conductor mindset, questioning/verification/steelman protocols, response format, security audit, file naming rules |
| 2. Rules | `~/.claude/rules/*.md` | All projects, auto-loaded modular | ~160 | Specific behavioral rules (see table below) |
| 3. Project | `~/projects/practice/CLAUDE.md` | Practice directory tree | ~290 | Agent sequencing, routing table, integration quick-reference, skills catalog, rituals, session maintenance, file organization |
| 4. Operations | `~/projects/practice/operations/CLAUDE.md` | Operations scripts context | ~646 | Script inventory, integration API docs, folder structure, background services, intelligence system, session index, outreach system |

**Rules files** (auto-loaded every session from `~/.claude/rules/`):

| File | What it governs |
|------|----------------|
| `code-quality.md` | TDD mandate, 10 Commandments of code quality, bestiary hunting (named code smells), good code checklist before completion |
| `build-bible.md` | Bible quick-reference summary -- 14 critical rules, 8 anti-patterns, architecture quick facts, pointers for when to consult the full bible |
| `steelman.md` | Adversarial plan review protocol -- mandatory critique/defend/fold cycle on every plan, upgrade signals for QA swarm and adversarial team |
| `voice.md` | Voice enforcement for communication drafted as the operator -- mandatory `personal-voice` skill load, banned words, warmth checklist |
| `integrations.md` | Tool-specific behaviors -- Apple Reminders, Python venvs outside synced folders, session resume links, background task bug workaround |
| `system-visibility.md` | Visibility prefixes for memory operations and bible operations -- forces explicit markers when reading/writing to memory or bible files |

**Loading mechanism:** Tiers 1 and 2 load automatically on every session start. Tier 3 loads when the working directory is within the practice project tree. Tier 4 loads when working within operations. Auto-compact triggers at 70% context usage.

**Derived copies note:** CLAUDE.md at all tiers contains summaries of bible principles. These are derived copies -- the canonical source is the bible. When the bible changes, CLAUDE.md files must update. The enforcement traceability matrix maps this dependency chain. Known debt: 8 major duplications (~250 lines) exist across tiers 1-3, including conductor mindset (3x), response format (3x), and critical partner protocol (3x).

**Decision tree for new instructions:** Universal rules go to Global or Rules, practice-specific goes to Project, operations/scripts go to Operations. Content belongs in exactly one tier. If it appears in two, one is wrong.

### Agent definitions -- 47 agents in 7 teams + specialists

**3-tier hierarchical model:**

```
Director (Opus, ~5% of work) --> Senior (Sonnet, ~15% of work) --> Junior (Haiku, ~80% of work)
```

Directors create bulletproof task definitions so precise that juniors cannot fail. Seniors decompose director-level specs into junior-executable chunks and quality-check output. Juniors execute using formulas (~80 lines each, no guessing). This implements bible pattern 2.1 (hierarchical cost optimization 80/15/5).

**7 hierarchical teams (30 agents):**

| Domain | Director | Senior | Junior | Team purpose |
|--------|----------|--------|--------|-------------|
| Development | `dev-director.md` | 3 seniors (frontend, backend, devops) | 8 juniors (python, typescript, shell, css, docker, api, react, database) | All coding work -- architecture through execution |
| Product management | `pm-director.md` | `pm-senior.md` | `pm-junior.md` | Project timelines, phases, client care |
| Copywriting | `copywriter-director.md` | `copywriter-senior.md` | `copywriter-junior.md` | Marketing copy, CTAs, headlines |
| Visual design | `visual-designer-director.md` | `visual-designer-senior.md` | `visual-designer-junior.md` | Visual design direction and execution |
| UX architecture | `ux-architect-director.md` | `ux-architect-senior.md` | `ux-architect-junior.md` | Site structure, wireframes, user flows |
| Content strategy | `content-strategist-director.md` | `content-strategist-senior.md` | `content-strategist-junior.md` | Sitemap architecture, content planning |
| Market research | `market-researcher-director.md` | `market-researcher-senior.md` | `market-researcher-junior.md` | Competitive analysis, market positioning |

The Development team is larger than the others because it covers 8 stack specialties at the junior tier. All other teams follow the standard 3-agent Director/Senior/Junior structure.

**Specialist agents (17, no hierarchy):**

| Category | Agent | Purpose |
|----------|-------|---------|
| Strategy | Conductor | Master orchestrator for complex multi-phase projects |
| Strategy | Messaging Specialist | Messaging framework specialist |
| Strategy | Brand Strategist | Brand positioning and identity strategy |
| Strategy | Voice Analyst | Extract voice patterns from transcripts and content |
| Strategy | Messaging Architect | Deep messaging architecture |
| Creative | SEO/GEO Strategist | SEO and GEO strategy with industry context |
| Creative | Creative Provocateur | Pattern-breaking ideas, challenge conventional thinking |
| Technical | Webflow Developer | Webflow implementation from design specs |
| Technical | Google Docs Editor | All Google Doc work delegated here -- no exceptions |
| Technical | SysAdmin | System maintenance, file organization, service health |
| Operations | Project Manager (methodology-based) | PM with specific methodology training |
| Operations | CFO | Financial analysis, pricing, profitability modeling |
| Operations | Project Manager | Generic project management |
| Operations | Sales/CRM | Pipeline management, lead qualification, follow-ups |
| Operations | Client Success | Client relationship health, retention, upsell identification |
| Operations | Learning Curator | Reviews all agent outputs for patterns, promotes learnings |
| Operations | Stats Viewer | System statistics and analytics display |

Additionally, 3 target client personas exist as simulation agents for messaging testing (industrial manufacturer, service business owner, construction company).

**Agent definition formats:** 8 agents use progressive disclosure (multi-file: `core.md` + examples + templates + checklists per bible pattern 2.3). This reduces context window consumption by ~60% -- `core.md` provides orientation in ~200 tokens, deeper modules load on demand. The remaining agents use monolithic single-file definitions.

**Cost model and routing:** 80% Haiku / 15% Sonnet / 5% Opus within the Claude model family. Cross-model routing adds Gemini (1M+ context, web grounding), Codex (background autonomous tasks), and Perplexity (citation-backed research) as additive specialists -- not replacements. Selection decision tree in the bible.

### Databases -- 6 SQLite schemas

| Database | Location | Records | Query interface | Purpose |
|----------|----------|--------:|----------------|---------|
| `transcripts.db` | `operations/meeting-intelligence/` | 1,900+ | `transcript_search.py`, SQL, FTS5 | Meeting transcripts and extracted insights. Core intelligence layer. |
| `sessions.db` | `operations/session-index/` | 2,900+ | `sessions` CLI, `session_search.py`, FTS5 | Claude Code session index -- search past work by content, client, date. |
| `contacts.db` | `operations/` | 2,000+ | `contacts_db.py`, SQL | Unified contacts and outreach log. Single source of truth for all lead/contact data. Aggregates touches from outreach bot, LinkedIn, email. |
| `intel.db` | `operations/intel-system/` | -- | surfacer, commitment tracker, relationship scorer | Commitments, personal intel, relationship scores. Intelligence layer. |
| `fsrs.db` | `operations/memory-system/` | -- | FSRS-6 algorithm module | Spaced repetition scheduling for the memory system. |
| `passive_income.db` | `operations/passive-income/` | -- | SQL | Passive income tracking. |

**Canonical domains (bible principle 1.5):** Meetings = `transcripts.db`. Session history = `sessions.db`. People = `contacts.db`. Commitments/relationship intel = `intel.db`. Each domain has exactly one canonical store. The `outreach_log` table in `contacts.db` aggregates touches from all outreach channels to prevent double-tapping contacts.

**Backup:** All databases backed up nightly at 3am via a backup script using `sqlite3 .backup` (not file copy -- corruption-safe). 7-day rotation to cloud storage. Push notification on failure.

**FTS5 indexing:** The three primary databases (transcripts, sessions, contacts) use SQLite FTS5 full-text search indexes, enabling sub-second keyword search across thousands of records without external search infrastructure.

### Scheduled background processes -- ~47 processes

All managed via the system's scheduler (e.g., macOS LaunchAgents, cron, systemd). Organized by function category:

| Category | Count | Description | Key processes |
|----------|------:|-------------|--------------|
| Meeting and intelligence | 6 | Meeting prep, transcript indexing, intelligence automation | Meeting nudge (every 5min), daily intelligence (6:30am), meeting indexer, transcript sync |
| Notifications and rituals | 4 | Daily rituals, digest, EOD capture | Morning triage (9am M-F), EOD nag (5pm M-F), daily digest (7am), EOD capture (5pm weekdays) |
| Pipeline and outreach | 5 | CRM health, outreach sync, prospect staging, commitment mining, CRM enrichment | Pipeline health (M/W/F 7:15am), outreach sync (every 30min), LinkedIn staging (weekly), commitment extractor (daily), CRM enricher |
| Approval and execution | 1 | Execute approved items from task queue | Approval executor (every 5min) |
| Session and learning | 5 | Session indexing, learning analysis, pattern detection, improvement | Session indexer (every 30min), pattern analyzer (Monday 7am), system learner (daily 8am), learnings review (Friday 5pm), weekly improvement (weekly) |
| Memory system | 2 | Memory maintenance, weekly synthesis | Memory maintenance, memory weekly synthesis (weekly) |
| Dashboards and monitoring | 2 | Dashboard data refresh, service health monitoring | Dashboard fetcher (every 5min), unified health monitor (continuous) |
| Backup and maintenance | 2 | Database backup, log rotation | DB backup (daily 3am), log rotation |
| Continuous services | ~20 | Dashboard servers, memory services, monitoring daemons | Various utility services |

**Key processes to understand:**

- **Unified health monitor** -- The system's immune system at the process level. Watches all services, auto-restarts crashed processes. Implements bible principle 1.12 (observe everything). Without this, silent service death (anti-pattern 6.8) would be the norm.
- **DB backup** -- Daily 3am, `sqlite3 .backup` to cloud storage. 7-day rotation. Push alert on failure. The safety net for all SQLite databases.
- **Meeting nudge** -- Every 5 minutes, checks for upcoming meetings and triggers dossier generation at 60min/30min marks. Implements bible pattern 2.13 (ritualization: silent prep, then notification, then engagement).
- **Session indexer** -- Incremental session index update every 30 minutes. Keeps the session database current so past-session search returns recent results.
- **Daily digest** -- 7am daily morning email digest of overnight system activity. The human-facing summary of what all the other processes did while the operator was asleep.

### Python scripts -- 300+ scripts organized by function

A large and heterogeneous collection spanning operations, intelligence, integrations, and utilities. Organized by function category rather than exhaustive listing.

| Category | Key scripts | Purpose |
|----------|-------------|---------|
| Integration layer | `integrations.py` | Unified API client for Todoist, Gmail, Calendar, Notion. Single source for all external service access. |
| Meeting intelligence | `dossier_generator.py`, `transcript_search.py`, `daily_intelligence.py`, `commitment_extractor.py` | Pre-meeting dossiers, transcript querying, daily intelligence automation, commitment mining from transcripts |
| Session management | `session_indexer.py`, `session_search.py`, `session_topic_capture.py`, `rename_session.py` | Session indexing, search, live topic capture, intelligent session naming |
| CRM and outreach | `pipeline_health.py`, `outreach_sync.py`, `linkedin_staging.py`, `crm_enricher.py`, `contacts_db.py` | Pipeline monitoring, outreach synchronization, prospect staging, CRM enrichment, master contacts database |
| Notifications | `notification_launcher.py`, `send_push_notification.py` | macOS notifications, push notifications, delayed notification management |
| Hooks | `questioning-nudge.py`, `delegation-check.py`, `bloat-watcher.py`, `component-audit.py`, `session-context.py`, `inbox-monitor.py` + 20 more | PreToolUse, PostToolUse, SessionStart, UserPromptSubmit, PreCompact, SessionEnd event handlers |
| Learning system | `pattern-analyzer.py`, `system-learner.py`, `learnings_review.py` | Pattern detection, self-learning analysis, weekly learnings review |
| Memory system | `session-memory-consolidation.py`, various memory scripts | Session memory extraction, deduplication, memory entry creation |
| Content engine | `linkedin-generator.py`, `proof_points_extractor.py` | LinkedIn post generation from voice patterns, marketing quote mining from transcripts |
| Rituals | `triage_data.py`, `eod_capture.py` | Morning triage data consolidation, end-of-day session summary |
| Backup | `db-backup.sh`, `codebase-backup.sh` | SQLite backup (3am), full codebase backup (2am) |
| Approval queue | `approval_bridge.py`, `approval_executor.py` | Task-based approval queue enqueue and execution. Pattern 2.11 (async human-in-loop). |

### Routing tables, templates, configuration

**Routing tables:** Define which agents handle which task types. The routing logic is the primary mechanism by which the Conductor delegates to specialists. Key constraint: agent sequencing dependencies are non-negotiable -- Voice Analyst before Messaging Specialist, Content Strategist before UX Architect, UX Architect before Visual Designer.

**Templates:** Task notes templates (context.md, decisions.md, open-questions.md). Agent templates include copywriting formulas, messaging frameworks, PM specs, design specs, UX specs, content specs, research templates, development patterns, and code review template.

**Plugins (6 configured):**

| Plugin | Source | Purpose |
|--------|--------|---------|
| `hookify` | official | Create hooks from natural language -- "prevent this behavior" converts to a hook rule |
| `frontend-design` | official | Frontend design patterns and UI component assistance |
| `orchestration` | marketplace | Multi-agent workflow syntax with `.flow` files |
| `code-review` | official | Structured code review workflows |
| `vercel` | official | Vercel deployment integration |
| `dx` | community | Developer experience enhancements |

**Dashboards (5):**

| Dashboard | Port | Type |
|-----------|-----:|------|
| Memory dashboard | 8766 | Static HTML, rebuilt on data change |
| Operations dashboard | 8701 | Static HTML, rebuilt on data change |
| Total recall dashboard | 7860 | Gradio-based (memory system interface) |
| Memory API server | 8765 | Live Bun process (backend, not visual) |
| Outreach dashboard | -- | Static HTML, rebuilt on data change |

All visual dashboards use static HTML rebuilt on data change -- no server-side state, no APIs, no framework. The memory API server is the exception: it is a live process serving the memory system.

**Integrations (12 external services):**

| Service | Method | Single source location |
|---------|--------|----------------------|
| Calendar | `integrations.py` | `integrations.calendar` |
| Gmail | `integrations.py` | `integrations.gmail` (dual-account: work + personal) |
| Google Docs | MCP server | MCP tools |
| Transcripts | `transcripts.db` (SQLite) | `operations/meeting-intelligence/` |
| Notion | `integrations.py` | `integrations.notion` |
| Todoist | `integrations.py` | `integrations.todoist` |
| Pushover | `send_push_notification.py` | CLI script |
| 1Password | `op` CLI | `op read "op://Secrets/[Item]/[field]"` |
| Webflow | MCP server | MCP tools |
| GitHub | `gh` CLI | Bash |
| Gemini | CLI | Homebrew |
| Memory system | Bun scripts | Node module |

**Decision tree for integration method:** MCP if available and working, `integrations.py` if supported, dedicated CLI if available, build into `integrations.py` as last resort (never create new standalone clients).

**Derived copies principle:** The consistency obligation for Machinery: every component in this sub-layer that summarizes or implements Principles content must be updated when the canonical source changes. This includes:
- CLAUDE.md at all 4 tiers (summarizes bible rules)
- Rules files in `~/.claude/rules/` (implement bible rules in auto-loadable form)
- Verification protocol files (implement bible verification requirements)
- Agent definitions (implement bible patterns like progressive disclosure and cost optimization)

When the bible changes: trace the enforcement chain using the enforcement traceability matrix, then update all derived copies. A nightly architecture sweep automates drift detection.

> **Note:** Agent definitions are mature machinery -- stable because battle-tested, not because foundational. When you add a new agent, register it in the system inventory and update counts. Agent and process counts drift over time; the nightly sweep catches discrepancies.

---

## Section 6: Capability / Protocols

Protocols are the normative layer of Capability. They specify what SHOULD happen, not what always happens. The distinction from Machinery is ontological: Machinery IS the apparatus (agents, databases, scripts exist as structural facts), while Protocols OUGHT TO govern behavior (rules, hooks, skills prescribe expected conduct). A database schema is a fact about the system. A rule that says "never write to the database without a transaction" is a behavioral commitment about how the system should be used.

The is/ought distinction matters in practice. A hook is not just a piece of code -- it is a behavioral commitment. "On PreToolUse, check whether the conductor should be delegating instead of executing" is not a description of what happens; it is a prescription of what must happen on every tool invocation. A skill is not just a prompt file on disk -- it is a behavioral capability that changes what Claude can do when invoked. Skills and hooks are both code, but their architectural role is governance, not infrastructure.

Protocols organize into four categories, following a military taxonomy: SOPs (standing operating procedures -- persistent behavioral rules that apply to every session), Sweeps (targeted systematic audits against specific quality concerns), Capabilities (skills and plugins that extend what the system can do), and Event automation (hooks that fire automatically on lifecycle events). SOPs are the standing orders. Sweeps are the inspections. Capabilities are the specialized equipment. Event automation is the immune system.

The immune system analogy deserves elaboration because hooks are the most powerful and the most dangerous part of Protocols. Hooks fire without being asked, detect conditions, and trigger responses -- exactly like an immune response. A PostToolUse hook that validates output is the immune response to quality failure. A PreToolUse hook that blocks execution after three delegation strikes is the immune response to conductor laziness. But a broken hook that fires on every event and blocks all work is an autoimmune disorder -- the immune system attacking the body it is supposed to protect. The 35 hooks in this system represent significant governance power, and that power demands careful testing before deployment.

### SOPs -- standing operating procedures

Rules files and protocols that govern standing behavior across all sessions. These are persistent -- they load at session start and shape behavior throughout.

**Rules files** (`~/.claude/rules/` directory, auto-loaded every session):

| File | What it governs | Key behaviors enforced |
|------|----------------|----------------------|
| `code-quality.md` | Code quality standards | TDD red/green/refactor mandate; 10 Commandments (no `any`, no silent catches, atomic commits, 500-line limit, etc.); bestiary hunting (named code smells: Any Hydra, Silent Catch Wraith, God Class Leviathan, useEffect Kraken, etc.); session-start ritual (run tests first); good code checklist before claiming completion |
| `build-bible.md` | Bible quick-reference | 14 critical rules summary; 8 anti-patterns with detection signals; architecture quick facts; pointers for when to load the full bible |
| `steelman.md` | Plan review protocol | Mandatory adversarial review before execution on every plan; critique-defend-fold cycle; upgrade signals (steelman to QA swarm to adversarial team); integration with questioning, planning, and verification flow |
| `voice.md` | Communication style | Mandatory `personal-voice` skill load before any draft as the operator; banned words; warmth checklist; no post-hoc voice checking -- load skill first, then write |
| `integrations.md` | Tool-specific behaviors | Apple Reminders via CLI; Python venvs outside cloud-synced folders; task format for session resumption; background task bug workaround |
| `system-visibility.md` | Operation visibility markers | Memory operations, bible operations, atlas operations, and inventory operations all prefixed with visibility markers. Distinguishes active reads from system-injected context. |
| `atlas.md` | Atlas section index | KCA model quick-ref, placement tests, 6 arrows, document triangle. Section routing table -- "when to load" triggers for each section. Entry point into the Atlas without loading it in full. |

**Verification protocol:**
- What it governs: mandatory verification before claiming completion. "Never use completion language ('done', 'fixed', 'working', 'complete') without verification evidence."
- Mandatory for: code changes, build changes, behavioral changes, doc references, config changes, anything affecting functionality
- Integration chain: Questioning protocol BEFORE work -> Steelman DURING planning -> Verification AFTER work. All three mandatory, every time.

**Questioning protocol:**
- What it governs: pre-work clarification. "Default to questioning. When in doubt, ask."
- Key behavior: 2-4 questions per ambiguous request; multiple-choice preferred over open-ended; "grill me" mode for major changes and risky decisions
- Target: ask BEFORE starting work, not after discovering ambiguity mid-execution

**Mistake documentation protocol:**
- What it governs: mandatory capture when mistakes occur
- Pre-work: query mistakes for relevant domain/project and internalize prevention strategies before starting

**Security audit protocol:**
- What it governs: mandatory security review before installing any new skill, agent, plugin, or hook
- Process: run an auditor tool; review report; only proceed if SAFE TO INSTALL
- Risk levels: Skills (medium), Agents (high), Plugins (high), Hooks (critical -- run automatically on events)

**Voice enforcement:**
- What it governs: all communication drafted as the operator must load the `personal-voice` skill FIRST -- no exceptions, no post-hoc checking
- Key constraint: skill loading happens before drafting, not after. The skill shapes the writing from the first word.

**Codify-agent-failures rule:**
- What it governs: when an agent makes a mistake that wastes meaningful time, add a specific preventive NEVER rule to the relevant CLAUDE.md tier immediately
- Rationale: negative rules ("NEVER do X because Y happened") prevent the exact class of mistake that actually occurred; CLAUDE.md rules are active every session while notes are passive and get buried

### Sweeps -- targeted systematic audits

A sweep is any targeted systematic quality audit against a specific concern, run across the codebase or content. The concept is the unit; specific sweeps are instances of that concept.

**What a sweep is:**

1. A specific quality concern is identified (e.g., "no files > 500 lines", "no `any` types", "no silent catch blocks")
2. The codebase is scanned systematically for violations
3. Violations are catalogued with location, severity, and remediation guidance
4. Violations are remediated (or documented with justification for exceptions)
5. Results are reported

Sweeps differ from one-off fixes in three ways: they are systematic (entire codebase, not one file), they are targeted (one specific concern per sweep, not general review), and they are reusable (the same sweep can run repeatedly as a regression check).

**The 14 code quality sweep types:**

Each sweep has a dedicated slash command. An orchestrator command coordinates one or more specialist sweeps.

| Sweep | What it audits | Bible principle |
|-------|---------------|-----------------|
| Naming | Variable, function, file naming quality | Commandment VIII (names are documentation) |
| Type | Type safety violations, `any` usage, unchecked casts | Commandment I (no `any` types) |
| Test | Missing tests, test quality, coverage, red phase confirmation | Bible 1.3 (TDD) |
| Dead code | Unused code, zombie TODOs, commented-out blocks | Commandment IX (delete dead code) |
| Architecture | Architecture violations, SRP breaches, dependency direction | Bible 1.5 (single source of truth) |
| Dependencies | Unjustified packages, version issues, phantom dependencies | Commandment VII (justify dependencies) |
| File size | Files approaching or exceeding 500 lines | Bible 1.4 (simplicity), anti-pattern 6.7 |
| Observability | Missing logging, silent failures, unmonitored services | Bible 1.12 (observe everything) |
| React | React anti-patterns -- useEffect Kraken, God Components, Infinite Re-render Ouroboros | Engineering bestiary |
| Accessibility | Accessibility violations in UI code | Web standards |
| Copy | Writing quality, voice violations, banned words | Content quality, voice enforcement |
| Adaptive | Adaptive content quality, complexity calibration | Bible 1.4 (simplicity) |
| Git | Git commit quality, message conventions, atomic commit violations | Commandment III (atomic commits) |
| Secret | Secrets in code or commits -- API keys, tokens, credentials | Commandment X (no secrets in commits) |

**The sweep concept extends beyond code.** The same systematic-audit pattern applies to any domain:
- Design quality sweep: systematic review of UI/UX decisions against design principles
- Copy quality sweep: systematic review of all text in a product against voice guidelines
- Product quality sweep: systematic review against product principles and user needs
- Infrastructure sweep: systematic review of background processes, databases, integrations for health and alignment
- New sweep types enter through the evolution protocol

**Sweeps vs hooks -- complementary enforcement:**

| Dimension | Hooks | Sweeps |
|-----------|-------|--------|
| Timing | Real-time (fires on events) | On-demand (user invokes) |
| Scope | Single tool call / session event | Entire codebase |
| Coverage | Narrow (specific event type) | Broad (systematic scan) |
| Purpose | Prevent violations as they happen | Remediate accumulated violations |
| Analogy | Adaptive immune response | Periodic health checkup |

Both are necessary. Hooks catch violations in real time but can only see individual events. Sweeps catch accumulated violations that slipped past hooks or pre-date hook installation. A system with only hooks accumulates invisible debt. A system with only sweeps catches problems too late.

### Capabilities -- skills and plugins

**Skills (100+, multiple locations):**

Skills are specialized behaviors loaded on demand when invoked by Claude or the operator. They are not always-on like rules files -- they load when needed and provide context-specific expertise for a particular task type.

| Category | Skills | Purpose |
|----------|--------|---------|
| Engineering | `code-quality`, `component-hardener`, `debugging`, `python-best-practices`, `test-driven-development`, `datetime-context` | Code quality auditing, security hardening, debugging methodology, language-specific practices, TDD workflow, temporal context |
| Intelligence | `intel-prep`, `intel-followup`, `intel-dossier`, `intel-status` | Pre-meeting intelligence, post-meeting processing, full person dossier, background service health |
| Strategy | `competitive-analysis`, `gtm-pricing`, `launch-gtm-execution`, `marketing-strategy-pmm` | Competitive analysis, pricing strategy, launch execution, product marketing |
| Sales | `cold-email-sequence-generator`, `outbound-optimizer`, `sales-enablement`, `sales-methodology-implementer` | Email sequences, outbound optimization, sales enablement, methodology implementation |
| Integrations | `apple-reminders`, `reddit-fetch`, `resend` | Apple Reminders via CLI, Reddit content via Gemini (WebFetch workaround), Resend email API |
| Meta | `find-skills`, `sequential-thinking`, `groom-claude`, `add-memory`, `arch-add`, `arch-load` | Skill discovery, step-by-step reasoning, CLAUDE.md grooming, memory addition, component registration (with placement tests), Atlas section routing |
| Creative | `canvas-design`, `pptx`, `slack-gif-creator` | Visual design assets, PowerPoint, animated GIFs |

Additionally, external/installed skills (20+) from various sources covering marketing, development tooling, voice enforcement, and more.

**How skills differ from hooks:**
- Skills are invoked -- Claude or the operator must explicitly call them. They load specialized behavior for a specific task, execute, and are done.
- Hooks are armed -- they register at session configuration time and fire automatically when their trigger event occurs. No invocation needed.
- Skills can spawn hooks (via the hookify plugin). Hooks can reference skills. But they are architecturally distinct: skills are opt-in capabilities, hooks are always-on governance.

**Slash commands (~65):**

Slash commands are user-invokable workflows defined as `.md` files in `~/.claude/commands/`. They overlap with but are distinct from skills: a command is a prompt template invoked by typing `/command-name`; a skill is a behavior module that loads specialized context.

| Category | Count | Key commands |
|----------|------:|-------------|
| Session management | 11 | `/done` (clean exit), `/handoff` (context save), `/rename` (session naming), `/search` (past sessions), `/pause` (park with task), `/agents` (browse roster), `/codereview` (quality audit) |
| Code quality crusade | 15 | `/church` (orchestrator), `/church-test`, `/church-type`, `/church-size`, `/church-arch`, `/church-dead`, `/church-secret`, + 8 more |
| Meeting commands | 5 | `/meeting-search`, `/meeting-history`, `/meeting-people`, `/meeting-stats` |
| Agent tier commands | 22 | `/conductor`, `/sales`, `/pm-junior`, `/pm-senior`, `/pm-director`, `/copywriter-junior`, etc. |
| Other | ~12 | `/verify`, `/qa-swarm`, `/handoff`, `/review-learnings` |

**Plugins (6 configured):**

| Plugin | What it adds | Architectural role |
|--------|-------------|-------------------|
| `hookify` | Hook creation from natural language -- "prevent this behavior" becomes a hook | The Adaptation arrow in action: observed unwanted behavior -> Protocol change (new hook) without writing hook code |
| `orchestration` | Multi-agent workflow syntax with `.flow` files | Complex workflow definition and execution beyond what individual slash commands support |
| `code-review` | Structured code review workflows | Codifies review process rather than relying on ad-hoc review |
| `frontend-design` | Frontend design patterns and UI component assistance | Design system knowledge for UI work |
| `vercel` | Vercel deployment integration | Deployment pipeline integration |
| `dx` | Developer experience enhancements | General DX improvements |

The `hookify` plugin deserves special attention because it is a meta-capability: it creates new Protocols from natural language. "I want to prevent Claude from making direct git pushes without confirmation" -> hookify generates a PreToolUse hook on Bash commands matching `git push`. This is the Adaptation mechanism in action: experience (observed unwanted behavior) -> Protocol change (new hook) -> without requiring the operator to write hook code manually. The plugin makes hook creation accessible but does not make it safe -- review what hookify generates before approving it.

### Event automation -- hooks

**37 hooks across 7 event types:**

The hook system is the immune system of the Protocols layer. Hooks fire without being asked, on specific lifecycle events, and implement behavioral governance that cannot rely on Claude remembering to do it.

**7 event types:**

| Event type | Hook count | When it fires | Worst-case timeout | Purpose |
|------------|----------:|---------------|--------------------:|---------|
| SessionStart | 4 | When a Claude Code session begins or resumes | 20s | Setup -- load context, check inbox health, initialize memory |
| UserPromptSubmit | 8 | When the operator sends a message | 28s | Audit -- track exchanges, capture topics, detect projects, cancel pending notifications, surface architecture sync reminders |
| PreToolUse | 7 | Before any tool call executes | 12s | Block + audit -- delegation enforcement, file size warnings, questioning reminders, component security audits, architecture sync detection |
| PostToolUse | 1 | After a tool call completes | 5s | Notification -- start delayed push notification when Claude asks for input |
| PreCompact | 3 | Before context compaction | 128s | State preservation -- capture session name, topic, and curate memories before context loss |
| SessionEnd | 8 | When session closes | 455s | Cleanup + capture -- memory consolidation, session naming, topic capture, state capture, response format check, hook stats |

**Enforcement tiers in practice:** Only `delegation-check.py` blocks execution (3-strike escalation per bible pattern 2.8: strikes 1-2 send notifications, strike 3 blocks the tool call + creates review task, resets on successful delegation). `bloat-watcher.py` and other advisory hooks warn once, never block. All other hooks audit, capture, or enrich -- they never prevent work.

**Hook inventory by category:**

| Category | Hooks | Purpose | Key hooks |
|----------|------:|---------|-----------|
| Quality gates | 3 | Prevent quality violations in real time | `delegation-check.py` (blocks after 3 solo-execution strikes), `bloat-watcher.py` (warns on >500-line files), `component-audit.py` (security audit on new components) |
| Behavioral nudges | 2 | Remind and guide behavior | `questioning-nudge.py` (rotating reminders to question before acting), Google Docs agent reminder |
| Architecture sync | 2 | Keep Atlas and system inventory in sync after edits | Detects atlas/inventory edits, writes reminder -> surfaces pending reminder in chat, clears file |
| Capture automation | 9 | Automatic experience and context capture | Session topic capture, periodic rename, session memory consolidation, session state capture, auto-rename, memory curation |
| Session tracking | 4 | Track session state and exchanges | Exchange counter, preview rename, project detector, skill usage tracker |
| Context management | 2 | Smart context loading and inbox monitoring | Session start intelligence, inbox overflow warning |
| Notifications | 2 | Push notification management | Start delayed notification on AskUserQuestion, cancel timer when operator responds |
| Memory system | 3 | Memory integration | Session start, user prompt, curation hooks |
| Reporting | 2 | Session-end statistics and format checking | Hook system stats, response summary format check |

**SessionEnd bottleneck:** 8 hooks with 455s worst-case total timeout. The two heaviest are memory consolidation (180s) and content spotting (120s). Proposed fix: move heavy hooks to async background processing, reducing SessionEnd total.

**Known fragility:**
- 3 hooks share a file marker approach for session identification. If markers collide or are not cleaned up, hooks misidentify sessions. Proposed fix: use session ID environment variable instead.
- No circuit breaker exists for cascading hook failures. A broken hook does not prevent subsequent hooks from running, but the total timeout accumulates.

**Bypass mechanism:** Set `SKIP_HOOK_[NAME]=1` as an environment variable to disable a specific hook for a session.

**Hook creation safety protocol:**
1. hookify generates the hook definition
2. The operator reviews the generated hook before approving
3. New hooks start in advisory mode (warn, don't block) for a trial period
4. After trial, hooks can be promoted to blocking enforcement if the violations they catch are real
5. Hooks that never fire are candidates for deletion

> **Note:** Hooks are the immune system. A broken hook is like an autoimmune disorder -- it attacks the work it is supposed to protect. Every new hook should be tested before deploying. The hookify plugin makes hook creation accessible but does not make it safe -- always review what hookify generates. The SessionEnd bottleneck is the most visible symptom of hook accumulation -- monitor it and consider async migration for heavy hooks.

---

## Section 7: Activity / Execution

### What "right now" means architecturally

Activity is the only layer where actual work happens. Every file written, every agent dispatched, every client deliverable produced, every database row inserted -- all of it occurs in Activity/Execution. Knowledge holds what the system knows. Capability holds what the system can do. Activity is where those two meet reality. Code gets written here. Proposals get drafted here. Meetings get prepped here. It is the sole site of value production.

And yet Activity is architecturally the lightest layer. Everything here is transient. A Claude Code session runs for minutes or hours, then ends. A background process fires, does its work, exits. A Task-spawned agent completes and its execution context evaporates. Nothing in Activity persists after the process ends -- not unless the Capture arrow carries it back to Knowledge/Experience. This is the fundamental paradox: the layer that produces all value has no memory of its own. A session that doesn't capture its insights, mistakes, or decisions leaves no trace. It happened, it produced artifacts (files, database writes, deliverables), and those artifacts persist in their respective layers -- but the *execution itself* is gone.

Why model something so ephemeral? Because Activity is where Architecture meets Reality. The gap between what Knowledge says should happen (build bible: "orchestrate, don't execute") and what Activity actually does (Claude writing code solo for 20 minutes) -- that gap is drift. Drift is invisible without a model of what Activity is and how it should behave. The 60 background processes define what *should* run in the background. But are they actually running? Are they succeeding? Are they capturing results? Those questions live in Activity, and answering them requires understanding Activity as an architectural layer, not just "stuff that's running."

How Execution is born and dies. Born: the Configuration arrow fires at session start -- CLAUDE.md loads across all four tiers, hooks arm across event types, skills become available for invocation, MEMORY.md loads empirical context, and the session has a configured execution environment. Each execution context (session, agent, script, process) is spawned from Capability machinery. Dies: work completes, the session closes, the process exits, the script returns. The lifecycle is always: **Configure -> Execute -> Capture -> End.** The Capture step is not guaranteed -- it requires deliberate action (Claude writing to MEMORY.md, hooks firing PostToolUse, scripts logging results) or it simply doesn't happen. Uncaptured Activity is lost Activity.

The paradox of Activity's authority. Activity has the lowest architectural authority of any layer. What happens in execution does not change the Architecture -- only the Capture, Adaptation, and Codification arrows can do that. A mistake made in a session is just a mistake until Capture sends it to Experience, where it may eventually influence Protocols (via Adaptation) or Principles (via Codification). A brilliant new workflow discovered during execution is just a one-off until it's captured and codified. Activity is where all value is produced, but Activity does not shape the system. The feedback arrows do that. This is by design -- it prevents ephemeral execution from destabilizing slow-changing layers. A bad session cannot corrupt Principles. It can only corrupt Principles if someone lets the Codification arrow fire without proper human judgment.

---

### Component types

#### Claude Code sessions

The primary execution context. Each session is a fresh instantiation of the configured system: the 4-tier CLAUDE.md hierarchy loads, hooks arm across event types, MEMORY.md loads empirical context from past sessions, and the full Capability layer becomes available. The operator is present (foreground interactive session) or absent (background session -- rare but possible).

**Key properties:**

- Each session is stateless at the Execution level -- no memory persists from previous sessions except via Knowledge/Experience (MEMORY.md, session index, task notes). Session N has no direct link to session N-1. Continuity is maintained through captured artifacts, not through execution state.
- Sessions are the primary channel for operator-Claude interaction and work production.
- Session transcript feeds the session index via the Capture arrow.
- Context compaction occurs within long sessions -- Claude's context window fills and earlier content is compressed. This is an intra-session lifecycle event, not a cross-session one.

**Failure mode:** Session crash. On restart, CLAUDE.md reloads, rules re-arm, MEMORY.md re-loads. Work-in-progress that was not captured is lost. Mitigation: capture frequently -- update MEMORY.md, write task notes, commit code. The smaller the gap between "work done" and "work captured," the lower the blast radius of a crash.

#### Running agents (Task-spawned subagents)

Spawned via the Task tool within a session. These are the delegation mechanism -- the Conductor dispatches work to specialist agents rather than executing solo (bible rule 1.1: orchestrate, don't execute).

**Key properties:**

- Inherit the parent session's configured environment or receive explicit context via the Task tool prompt. Agent definitions configure their behavior, expertise, and constraints.
- Can run in parallel (independent tasks -- multiple Task calls in a single message) or sequentially (dependent tasks -- one agent's output feeds the next).
- Background agents run without the operator present; results return when complete. Foreground agents run with immediate feedback.
- Agent sequencing matters -- dependency chains exist (e.g., Voice Analyst before Brand Strategist before Messaging Architect before Copywriter). Violating sequencing produces wasted work.

**Failure mode:** Silent background failure. An agent completes with an error but the parent session does not notice because it has moved on to other work. Mitigation: always check background agent outputs before proceeding. Do not treat background agent completion as guaranteed success.

#### Tool calls in progress

Individual tool invocations within a session: Read, Write, Edit, Bash, Grep, Glob, Task, WebFetch, WebSearch, NotebookEdit, and MCP tool calls.

**Key properties:**

- Atomic at the tool level -- each call is a discrete operation that succeeds or fails.
- Not atomic at the session level -- a multi-step task (read file, edit three sections, run tests) can fail partway through, leaving the codebase in an intermediate state.
- Hooks fire around tool calls: PreToolUse hooks can block or modify a tool call before it executes; PostToolUse hooks can act on the result after completion. These are the Protocols layer governing Activity in real time.

**Failure mode:** Partial state from a failed tool sequence. If a multi-step edit fails after the second of three edits, the file is in an intermediate state. Mitigation: the atomic operations pattern (bible rule 1.9 -- temp file + rename) prevents partial state for critical writes. For multi-file changes, commit or checkpoint before the sequence begins.

#### Active scripts (Python scripts invoked via Bash)

300+ Python scripts exist in the system; some subset is running at any given time -- invoked by background processes on schedule, by Claude via the Bash tool, or directly by the operator in a terminal.

**Key properties:**

- Run in the shell context of the session or background process that invoked them. They may use the integrations module for external service access or interact with SQLite databases directly.
- May write to databases, files, logs, or external services (push notifications, email).
- Not always visible to Claude -- a script invoked by a background process runs entirely outside any Claude Code session. Its output appears only in logs or database records.

**Failure mode:** Script runs, fails silently, produces no output or wrong output. A database write that silently corrupts data is worse than a script that crashes loudly. Mitigation: structured logging (bible rule 1.12 -- observe everything), health monitoring, and push alerts for critical failures.

#### Active background processes

~47 scheduled processes define scheduled and persistent background work. At any moment, several are actively executing.

**Key properties:**

- Run independently of Claude Code sessions -- they fire even when the operator is not using Claude, even when no session is active. They are the system's autonomous nervous system.
- Scheduled (daily digest, nightly architecture sweep, morning dossier generation) or persistent (monitors, watchers).
- Write results to logs, databases, or notification channels.

**Failure mode:** A process fails and stops firing. Unlike a session crash (which the operator notices immediately), a background process failure can be silent for days. The morning digest doesn't arrive -- is it broken, or just nothing to report? Mitigation: health monitoring, architecture sweep checks, and the operational principle that absence of output from a scheduled process is itself a signal that requires investigation.

#### DB I/O operations

Active reads and writes to the SQLite databases. Multiple components interact with databases concurrently -- scripts, background processes, and Claude sessions may all access the same database.

**Key properties:**

- SQLite enforces serialization at the file level via its locking mechanism -- concurrent writes are handled, though long-running write transactions may block other writers.
- Read operations are safe to run concurrently and do not block.
- The data stored in these databases is Knowledge/Experience. The databases as active I/O infrastructure are Capability/Machinery. The act of reading from or writing to them right now is Activity/Execution. Three layers, one component -- this is the dual-lens principle in action.

**Failure mode:** Corrupted database state from an interrupted write (process killed mid-transaction, disk full, system crash). Mitigation: the atomic operations pattern applies -- use transactions, never leave partial writes. SQLite's WAL mode provides crash recovery for most scenarios.

#### Background tasks

Claude Code background tasks (spawned via the Task tool), background Bash commands, and any work delegated to run without blocking the active session.

**Key properties:**

- Run without direct observation -- the operator and Claude are doing other work while these execute.
- Results arrive asynchronously and may require integration with ongoing work. The context in which the result arrives may have shifted from the context in which the task was dispatched.
- **Known bug:** `run_in_background: true` on Bash commands should be avoided. Background Bash results can crash sessions after context compaction. Use the Task tool for parallel work.

**Failure mode:** Context compaction loss. A background task's result arrives after compaction has discarded the context that spawned it. The result cannot be integrated and may crash the session. Mitigation: save important background outputs to files early -- before compaction can discard the originating context.

---

### Lifecycle

```
Capability/Machinery ──(Configuration)──> Execution context created
                                          │
                                          ├── CLAUDE.md loads (4 tiers)
                                          ├── Hooks arm (across event types)
                                          ├── MEMORY.md loads
                                          ├── Skills become available
                                          │
                                          v
                              Work executes (sessions, agents,
                              scripts, background processes,
                              tool calls, DB I/O, background tasks)
                                          │
                                          v
Activity/Execution ──(Capture)──────────> Knowledge/Experience
                                          │
                                          ├── Session transcript -> session index
                                          ├── Insights/mistakes -> MEMORY.md
                                          ├── Artifacts -> files, databases
                                          ├── Observations -> memory system
                                          │
                                          v
                              Activity ends (session closes,
                              process exits, work completes)
```

The key insight: the middle arrow (Capture) is the only one that is not automatic. Configuration fires reliably at session start -- CLAUDE.md always loads. But Capture requires deliberate action. A session that produces brilliant insights but does not write them to MEMORY.md, does not document mistakes, and does not update task notes -- that session's value evaporates when it ends. The lifecycle is Configure -> Execute -> **Capture (if deliberate)** -> End.

---

### Foreground vs background

This distinction is operationally significant but not architecturally load-bearing. Both foreground sessions and background processes are Activity/Execution -- they occupy the same sub-layer. But the operational differences affect failure modes, recovery, and capture reliability.

| Dimension | Foreground | Background |
|-----------|-----------|------------|
| Operator present? | Yes | No |
| Feedback loop | Immediate -- operator observes, redirects | Delayed -- results appear later (or never) |
| Failure visibility | Immediate -- operator sees the error | May be silent -- requires monitoring to detect |
| Capture mechanism | Active -- Claude captures deliberately | Automatic -- hooks, scripts, logging |
| Recovery on failure | Operator can redirect, retry, adjust | Requires monitoring infrastructure to detect and alert |
| Examples | Interactive Claude sessions, foreground agents | Background processes, background Task agents, scheduled scripts |

Background Activity carries higher operational risk. Failures may be silent, capture depends on automated mechanisms rather than deliberate Claude action, and recovery requires monitoring infrastructure rather than human judgment. The system mitigates this with: process health monitoring, architecture sweep checks, push alerting for critical failures, and the daily digest that surfaces anomalies.

---

### What Execution is not

**Not persistent.** Nothing stored here survives session end without the Capture arrow. If you are looking at something that persists between sessions, it is not Activity -- it is Knowledge or Capability.

**Not authoritative.** What happens in Execution does not change Knowledge or Capability without feedback arrows (Capture -> Adaptation -> Codification). A session cannot modify the bible. A session cannot change a hook's behavior permanently. A session can write to MEMORY.md (Capture) and can edit a hook file (which is a Capability change, not an Activity change -- the edit persists because it modified Capability, not because Activity persisted).

**Not where the system's identity lives.** Identity -- the system's beliefs, principles, and architectural model -- lives in Knowledge/Principles. Activity expresses identity but does not define it.

**Not a design concern at the individual level.** This section models the *types* of execution (sessions, agents, scripts, processes, tool calls, DB I/O, background tasks), not individual instances. Which specific Bash command is running right now is operational, not architectural. The architectural question is: what kinds of execution exist, how are they born, how do they fail, and how do they capture their results?

---

### Failure modes summary

| Failure | What happens | Detection | Mitigation |
|---------|-------------|-----------|------------|
| Session crash | Work-in-progress lost if uncaptured | Operator notices immediately | Capture frequently: MEMORY.md, task notes, git commits |
| Silent background failure | Process fails without alerting anyone | Health monitoring, daily digest, architecture sweep | Push alerts, monitoring hooks, absence-of-output checks |
| Capture gap | Session completes successfully but writes nothing back to Experience | Pattern: same mistakes repeat across sessions; no session index entry | Deliberate capture habits; PostToolUse capture hooks; session-rename hook at session end |
| Partial state | Multi-step operation fails midway, leaving intermediate state | Manual inspection of files/database | Atomic operations (temp file + rename); transactions; checkpoint before multi-step sequences |
| Context compaction loss | Background task result arrives after compaction discards originating context | Unrecoverable session error | Save background outputs to files before compaction risk; avoid `run_in_background: true` on Bash |
| Agent sequencing violation | Agents dispatched out of dependency order; downstream agent produces garbage from inadequate inputs | Output quality degradation; wasted tokens | Follow agent sequencing rules; Conductor verifies prerequisites before dispatch |
| Stale configuration | Session loads outdated CLAUDE.md or rules after a mid-session edit to those files | Behavior doesn't match recently updated rules | Restart session after Capability changes; edits to Machinery/Protocols take effect on next Configuration arrow |

---

### Drift detection

Activity is where drift becomes visible. Drift is the gap between what Knowledge/Capability says should happen and what Activity actually does. Three forms:

**Behavioral drift.** Claude's execution diverges from configured protocols. Example: the questioning protocol says "default to questioning -- when in doubt, ask." If a session proceeds for 15 minutes on ambiguous requirements without asking a single clarifying question, that is behavioral drift. Detection: behavior audit, correction-type tracking.

**Operational drift.** Background processes diverge from their intended schedule or output. Example: a process that should fire daily has been silently failing for a week. The daily digest that should surface anomalies has itself drifted. Detection: architecture sweep, health checks, absence-of-output monitoring.

**Capture drift.** The Capture arrow weakens -- sessions produce valuable work but capture less of it over time. Knowledge/Experience grows stale relative to what Activity has actually learned. Detection: session index gaps (sessions that ran but were not indexed), MEMORY.md staleness (last update date vs. session count since), repeated mistakes that should have been captured after the first occurrence.

When drift is detected, the correction path is: detect in Activity -> capture the observation (Capture arrow to Experience) -> adjust governance (Adaptation arrow to Protocols) -> if systemic, codify (Codification arrow to Principles). Drift is never fixed by changing Activity directly -- Activity is ephemeral. You fix drift by changing the layers that configure Activity.

> **Note:** Everything here is ephemeral. The only lasting effect of Activity is what Capture sends back. When you notice a pattern in execution -- an insight, a mistake, a better approach -- capture it immediately. Ephemeral becomes permanent only through deliberate capture. The Capture arrow is the only path from "what happened today" to "what the system knows." The biggest risk in Activity is not failure -- failures are loud and recoverable. The biggest risk is the capture gap: sessions that succeed, produce value, and teach nothing because nobody wrote it down.

---

## Section 10: Document index

Every document in the system, organized by architectural model layer. Load this section when you need to find a document.

The system described here has 35+ governing documents, 300+ scripts, 100+ skills, 60 background processes, and 6 databases. Without a master index, the system is unknowable -- components that solve real problems exist but cannot be found. A conductor who cannot find the right document at the right time will re-derive decisions that were already settled, miss enforcement mechanisms that already exist, or build machinery that duplicates what is already running. This section closes that gap.

This index is organized by the architectural model layers defined in the framework. Each document appears once, in its home layer. For documents that serve multiple purposes, they appear in the layer that represents their primary role -- the canonical source determines home per the authority-record test. The index captures: name, purpose, maintenance status, and size estimate.

---

### Knowledge / Principles

Documents that define HOW and WHY -- the governing beliefs, principles, patterns, and architectural decisions.

| Name | Purpose | Status | Size |
|------|---------|--------|------|
| Build bible | HOW WE BUILD -- 14 principles, 19 patterns, playbook, agent system, anti-patterns, operations reference, evolution protocol, debt inventory | Active | ~1,900 lines |
| Atlas (this document) | HOW IT ALL FITS -- architectural model, arrows, precedence, flows, document index, evolution protocol | Active | ~2,500 lines |
| Universal learnings | Cross-project insights promoted from learnings queues after 3+ confirmations | Active | ~476 lines |
| Cross-project insights | Synthesized patterns observed across multiple client engagements | Active | ~403 lines |
| Quality system overview | Unified view of questioning + verification + steelman protocols | Stable | ~396 lines |
| System architecture | Structural description of the 5-layer system model and component relationships | Stable | ~501 lines |

### Knowledge / Experience

Documents that record WHAT HAPPENED -- session history, decision records, learnings, mistakes, changelogs.

| Name | Purpose | Status | Size |
|------|---------|--------|------|
| System inventory | WHAT EXISTS -- component catalog with rationale column for every skill, hook, agent, database, process, integration | Active | ~589 lines |
| Bible changelog | Every bible change -- what, why, source, evaluation status | Active | Growing |
| Debt inventory | 15 known technical debt items with severity, impact, and mitigation status | Active | Bible section |
| Session index (database) | 2,900+ Claude Code sessions indexed with FTS5 full-text search -- searchable by content, client, date, tags | Active | ~65MB |
| Session index scripts | Session indexer, search, analyzer, topic capture | Active | 4 scripts |
| Learnings queue | Pending learnings awaiting review and promotion to universal learnings | Active | Variable |
| Work log | Session-level work summaries for significant sessions | Active | ~421 lines |
| Auto-memory (global) | Project-specific session memory -- preferences, patterns, project context | Active | Growing |
| Spaced repetition memory system | FSRS-6 spaced repetition system -- 102 features, 2,000+ tests, Python 3.11+ | Active | Codebase |
| Transcripts database | 1,900+ meeting transcripts with FTS5 search -- core intelligence layer | Active | Database |
| Contacts database | 2,000+ unified contacts + outreach log -- single source of truth for lead/contact data | Active | Database |
| Intelligence database | Commitments, personal intel, relationship scores -- intelligence layer | Active | Database |
| Roadmap | System evolution roadmap and planned improvements | Active | ~116 lines |

### Capability / Machinery

Documents that define WHAT RUNS -- configuration, agent definitions, database schemas, scripts, templates.

**CLAUDE.md files (4-tier hierarchy):**

| Name | Scope | Status | Size |
|------|-------|--------|------|
| Global CLAUDE.md | All projects -- conductor mindset, planning mandate, questioning/verification/steelman protocols, response format, security audit | Active | ~181 lines |
| Project CLAUDE.md | Project scope -- bible summary (auto-loaded) | Stable | ~50 lines |
| Practice CLAUDE.md | Practice project -- agent sequencing, routing table, integration quick-reference, skills, rituals, session maintenance, file organization | Active | ~289 lines |
| Operations CLAUDE.md | Operations scope -- integration API docs, script inventory, folder structure, background services, intelligence system, session index, outreach system | Active | ~642 lines |

**Rules files (auto-loaded every session):**

| File | Purpose |
|------|---------|
| `code-quality.md` | Code quality protocol -- 10 commandments, bestiary hunting, TDD mandate, good code checklist |
| `build-bible.md` | Bible summary -- 14 rules, 8 anti-patterns, architecture quick facts |
| `steelman.md` | Steelman protocol -- adversarial plan review, upgrade signals, integration with planning flow |
| `voice.md` | Voice enforcement -- mandatory personal-voice skill gate for any writing as the operator |
| `integrations.md` | Tool-specific behaviors -- Apple Reminders, Python venvs, session links, background task workaround |
| `system-visibility.md` | Memory and bible visibility prefixes -- active operations must be prefixed with visibility markers |

**Agent definitions:**

| Component | Count |
|-----------|-------|
| Agent reference (master roster) | 1 file |
| Hierarchical teams (6 domains) | 18 agents |
| Development team | 12 agents |
| Specialist agents (multi-file) | 9 agents |
| Specialist agents (single-file) | 9 agents |
| Simulation agents | 3 agents |
| Agent templates | ~15 files |

**Skills (100+ across 3 locations):**

| Location | Count | Examples |
|----------|-------|---------|
| Personal skills | ~33 | apple-reminders, intel-prep, intel-followup, cold-email-sequence-generator, sequential-thinking, python-best-practices |
| External/installed skills | ~25 | copywriting, seo-audit, marketing-psychology, personal-voice, verification-before-completion |
| Local system skills | 2 | docs-fix, project-file-cleanup |
| Marketing skills (community) | ~20 | page-cro, signup-flow-cro, ab-test-setup, pricing-strategy, launch-strategy |

**Slash commands:**

| Category | Count | Key commands |
|----------|-------|-------------|
| Session management | 11 | `/done`, `/handoff`, `/search`, `/pause`, `/codereview`, `/agents`, `/bible-add` |
| Code quality crusade | 15 | `/church` (orchestrator), `/church-git`, `/church-test`, `/church-size`, `/church-arch`, `/church-type` |
| Meeting commands | 5 | `/meeting-search`, `/meeting-history`, `/meeting-people`, `/meeting-stats` |
| Agent tier commands | 21 | `/conductor`, `/sales`, `/pm-junior`, `/pm-senior`, `/pm-director`, etc. |
| Other commands | 5 | `/verify`, `/qa-swarm`, `/handoff`, `/review-learnings` |

**Hooks:**

| Hook type | Count | Key hooks |
|-----------|-------|-----------|
| PreToolUse | 6 | `questioning-nudge.py`, `delegation-check.py`, `bloat-watcher.py`, `component-audit.py`, `skill-usage-tracker.py` |
| PostToolUse | 1 | Delayed notification trigger |
| SessionStart | 4 | Session context, inbox monitor, memory init |
| UserPromptSubmit | 7 | Notification cancel, memory hooks, exchange counter, rename preview, topic capture, project detector |
| PreCompact | 3 | Periodic rename, topic capture, memory curation |
| SessionEnd | 8 | Memory curation, auto-rename, topic capture, state capture, format check, stats, memory consolidation, content spotter |

**Plugins:**

| Plugin | Source | Purpose |
|--------|--------|---------|
| hookify | official | Hook creation and management tooling |
| frontend-design | official | Frontend design patterns and UI component assistance |
| orchestration | marketplace | Workflow orchestration with custom `.flow` syntax |
| code-review | official | Structured code review workflows |
| vercel | official | Vercel deployment integration |
| dx | community | Developer experience enhancements |

**Background processes (~47 total):**

| Category | Count | Key processes |
|----------|-------|-------------|
| Meeting and intelligence | 6 | Meeting nudge (5min), daily intelligence (6:30am), meeting indexer, transcript sync |
| Notifications and rituals | 4 | Morning triage (9am), EOD nag (5pm), daily digest (7am), EOD capture (5pm) |
| Pipeline and outreach | 5 | Pipeline health (M/W/F), outreach sync (30min), LinkedIn staging (weekly), commitment extractor (daily), CRM enricher |
| Approval and execution | 1 | Approval executor (5min) |
| Session and learning | 5 | Session indexer (30min), pattern analyzer (Mon 7am), system learner (daily 8am), learnings review (Fri 5pm), weekly improvement (weekly) |
| Memory system | 2 | Memory maintenance, memory weekly synthesis |
| Dashboards and monitoring | 2 | Dashboard fetcher (5min), unified health monitor (continuous) |
| Backup and maintenance | 2 | DB backup (3am daily), log rotation |

**Databases (all SQLite):**

| Database | Records | Query interface |
|----------|--------:|----------------|
| `transcripts.db` | 1,900+ | `transcript_search.py`, SQL, FTS5 |
| `sessions.db` | 2,900+ | `sessions` CLI, `session_search.py`, FTS5 |
| `contacts.db` | 2,000+ | `contacts_db.py`, SQL |
| `intel.db` | -- | surfacer, commitment tracker |
| `fsrs.db` | -- | FSRS-6 module |
| `passive_income.db` | -- | SQL |

**Dashboards:**

| Dashboard | Port | Technology |
|-----------|-----:|-----------|
| Memory dashboard | 8766 | Static HTML |
| Operations dashboard | 8701 | Static HTML |
| Total recall (memory system) | 7860 | Gradio |
| Memory API server | 8765 | Bun (backend, not visual) |

**Integrations:**

| Service | Client | Location |
|---------|--------|----------|
| Todoist | `integrations.todoist` | `operations/integrations.py` |
| Gmail | `integrations.gmail` | `operations/integrations.py` |
| Calendar | `integrations.calendar` | `operations/integrations.py` |
| Notion | `integrations.notion` | `operations/integrations.py` |
| Google Docs | MCP server | google-docs MCP |
| Pushover | `send_push_notification.py` | `operations/notifications/` |
| 1Password | `op` CLI | System |
| Webflow | MCP server | webflow MCP |
| GitHub | `gh` CLI | System |
| Gemini | CLI | System |
| Memory system | Bun scripts | Node module |

**Templates:**

| Template | Purpose |
|----------|---------|
| Project template (directory) | Full project scaffold -- project brain, learnings queue, config, process notes, messaging framework, project overview |
| Project brain template | Standalone project brain template |
| Learnings queue template | Learnings queue template for new projects |
| Task notes templates | `context.md`, `decisions.md`, `open-questions.md` -- for task notes directories |

### Capability / Protocols

Documents that define HOW TO ACT -- protocols, rules, workflows, reference material.

**Core protocols:**

| Name | Purpose | Status | Size |
|------|---------|--------|------|
| Verification protocols | Mandatory verification before claiming completion -- matrix, checklists, evidence requirements | Active | ~413 lines |
| Questioning protocol | Pre-work clarification protocol -- when to ask, how to ask, multiple-choice preference | Active | ~406 lines |
| Mistake documentation protocol | Mandatory capture protocol for mistakes -- format, routing, severity classification | Active | ~302 lines |
| File organization rules | Comprehensive decision tree and routing matrix for file placement | Active | ~533 lines |
| Rituals | Morning triage and end-of-day capture ritual definitions | Active | ~227 lines |
| Response format | Required response summary format specification | Stable | ~267 lines |

**Reference documents:**

| Name | Purpose | Status | Size |
|------|---------|--------|------|
| Voice reference | Voice patterns, banned words, communication style for writing as the operator | Stable | ~207 lines |
| Learnings system | How the two-level learning + promotion pipeline works | Stable | ~41 lines |
| Project lifecycle | Phase definitions for client project work | Stable | ~100 lines |
| Maintenance retainers | All retainer tiers, scope definitions | Stable | ~279 lines |
| Task workflow | Task integration workflow patterns | Stable | ~51 lines |
| Maintenance messaging framework | Messaging framework specific to maintenance retainer positioning | Stable | ~526 lines |

**Operational reference:**

| Name | Purpose | Status | Size |
|------|---------|--------|------|
| Scheduling guidelines | Meeting scheduling preferences and constraints | Stable | ~29 lines |
| Code standards | Repo setup, READMEs, changelogs, open source packaging standards | Stable | ~154 lines |
| Dossier style | Pre-meeting dossier format and content requirements | Stable | Reference |

**Messaging and voice system:**

| Name | Purpose | Status | Size |
|------|---------|--------|------|
| Messaging framework | Core messaging framework | Stable | ~581 lines |
| Voice bank | Extracted voice patterns and examples | Stable | ~842 lines |
| Quick reference | Condensed messaging/voice reference card | Stable | ~128 lines |
| Examples (subdirectory) | Headlines, LinkedIn posts, cold emails | Stable | 3 files |

**Mistake system support documents:**

| Name | Purpose | Status |
|------|---------|--------|
| Mistake cheatsheet | Quick-reference for mistake documentation format | Stable |
| Mistake-to-learning flow | How mistakes promote through the learning pipeline | Stable |
| Mistake system diagram | Visual representation of the mistake system | Stable |

**Content engine:**

| Script | Purpose |
|--------|---------|
| `voice-patterns.json` | Extracted voice patterns |
| `linkedin-generator.py` | LinkedIn post generation from voice patterns |
| `proof_points_extractor.py` | Mine testimonials from transcripts |
| `quote-bank-from-transcripts.md` | Marketing quotes extracted from meetings |

**Notification system:**

| Script | Purpose |
|--------|---------|
| `send_push_notification.py` | Push notifications to the operator's phone |
| `start_delayed_notification.sh` | Delayed notification when Claude waits for input |
| `cancel_notification.sh` | Cancel pending notification when operator responds |
| `approval_bridge.py` / `approval_executor.py` | Approval queue for notification-gated actions |

### Cross-cutting / System

Documents that span multiple layers or govern the system as a whole.

| Name | Purpose | Status | Size |
|------|---------|--------|------|
| Atlas (this document) | Master architectural document -- model, arrows, flows, index, evolution | Active | ~2,500 lines |
| System overview | High-level system overview and orientation | Stable | Reference |
| System index | Index of all system contents | Active | Reference |
| Settings file | Master Claude Code configuration -- hooks, plugins, environment variables, permissions | Active | Config |
| Status line config | Custom status line showing context usage, model, session ID, topic | Stable | Config |

---

### Cross-references

Key relationships between documents that are not obvious from the layer tables alone.

**Bible -> CLAUDE.md (derived copy obligation):** The bible is canonical for principles and patterns. CLAUDE.md at all tiers contains derived summaries of bible content. When the bible changes, affected CLAUDE.md files must update. The enforcement traceability matrix traces which CLAUDE.md sections derive from which bible rules.

**Atlas -> Bible and system inventory:** The Atlas references both but duplicates neither. For HOW WE BUILD (principles, patterns, playbook), see the bible. For WHAT EXISTS (detailed component specs with rationale), see the system inventory. The Atlas provides the architectural context -- WHERE things live, WHY they are there, HOW they connect.

**System inventory -> all component docs:** The inventory is the master catalog. When a new component is created, it should appear in the inventory before it appears elsewhere. The Atlas section 10 tables are derived summaries of the inventory. If a discrepancy exists between this index and the inventory, the inventory is canonical for component details; the Atlas is canonical for layer placement.

**Session index -> all session transcripts:** The session index contains metadata (name, date, tags, summary, git state). Full transcripts are stored separately and accessible via session ID.

**Bible changelog -> Bible:** The changelog is the evolution history of the bible. Reading a bible principle in context sometimes requires reading its changelog entry to understand why it was added or modified.

**Learnings queue -> universal learnings -> Bible:** The promotion pipeline from observation to codified principle. Learnings queue holds pending observations. After 3+ confirmations, they promote to universal learnings. During weekly bible review, recurring universal learnings may become new principles or patterns. Each step is a separate document in a different layer.

**Mistake documentation -> learnings queue -> universal learnings:** Mistakes captured with high intent feed anti-pattern identification. Recurring mistakes can be named as anti-patterns and codified in the bible.

**CLAUDE.md hierarchy -> rules files -> hooks:** The CLAUDE.md hierarchy defines behavioral expectations. Rules files auto-load enforcement summaries every session. Hooks provide runtime enforcement. These three layers form a defense-in-depth stack: CLAUDE.md tells Claude what to do, rules remind Claude at session start, hooks catch violations during execution.

**Agent definitions -> agent templates -> routing table:** Agent files define behavior. Templates provide reusable scaffolding for agent outputs. The routing table maps work types to agents. All three must stay synchronized when agents are added, modified, or archived.

---
