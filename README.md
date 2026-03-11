# Atlas

You've got 40 components in your Claude Code system. You can't tell where new ones go.

---

## The problem

An AI-augmented setup grows fast. You start with a CLAUDE.md. Then a hook. Then three agents, a background job, a memory system, a skill library. Before long you're asking: does this go in `rules/` or in `agents/`? Is this hook behavior or is it a skill? Why does Claude place similar things differently across sessions?

The problem isn't complexity -- it's that you don't have a shared model. Without one, every placement decision is improvised, Claude makes different calls in different contexts, and you spend more time managing structure than doing work.

The drift is subtle at first. You find three versions of the same instruction in different places. A hook that should be a rules file. Knowledge that fires automatically when it should sit quietly. Six months in, the system is working against you.

And the architecture of these systems keeps evolving. New primitives (hooks, skills, MCP servers, background agents) appear constantly. Without a stable model for how they relate, every new capability is another decision made in a vacuum.

## What changes

**Every component has an obvious home.** The KCA model gives three layers (Knowledge, Capability, Activity) and six sub-layers. The 3 placement tests give unambiguous answers. You stop debating and start placing.

**Claude makes consistent decisions.** When Claude knows the model, it places new components the same way every session. The 3 tests become a decision procedure that generalizes -- even to components that didn't exist when you first built your system.

**You see how your system improves itself.** The 6 feedback arrows describe how information flows between layers: how principles constrain behavior, how experience drives adaptation, how activity captures knowledge. You stop wondering why things work (or don't).

**The framework stays current as the field evolves.** New primitives get placed using the same 3 tests. When the community discovers a new architecture pattern, the KCA model tells you where it fits without requiring a rethink from scratch.

## What's in it

- **KCA model** -- Knowledge / Capability / Activity, three layers, six sub-layers. Every component in any AI system belongs in exactly one.
- **3 placement tests** -- consult-vs-fire, authority-record, stability-as-property. Ask them in order and get an unambiguous answer every time.
- **6 feedback arrows** -- how principles constrain capability, how experience drives adaptation, how activity escalates to principles. The loops that keep the system alive and improving.
- **Precedence hierarchy** -- when directives conflict, this tells Claude which one wins.
- **A worked example** -- one real AI-augmented system mapped onto KCA, so you see how the framework applies before you apply it yourself.
- **ADAPTING.md** -- how to create your own Atlas for your specific system.
- **`/qq-arch-add` and `/qq-arch-load` slash commands** -- included in `commands/`. `qq-arch-add` runs the 3 placement tests interactively and registers a new component. `qq-arch-load` routes natural-language questions to the right Atlas section without loading the whole document.

**Add it to Claude Code:**

```
git clone https://github.com/lee-fuhr/atlas ~/atlas
cp ~/atlas/commands/qq-arch-add.md ~/.claude/commands/
cp ~/atlas/commands/qq-arch-load.md ~/.claude/commands/
```

Then create `~/.claude/rules/atlas.md` pointing at `~/atlas/atlas-framework.md`. Claude knows the model every session. Run `/qq-arch-add` when a new component needs placing. Run `/qq-arch-load [question]` to pull the right section without loading everything.

---

## Contributing

AI-augmented systems are still being figured out. New primitives appear, new patterns emerge from production use, new failure modes get discovered. The Atlas framework stays useful by evolving with the field -- but it needs input from people running real systems.

If you've hit a placement decision the 3 tests don't resolve cleanly, found a feedback loop the 6 arrows don't capture, or discovered an architectural pattern that doesn't fit the current model -- open an issue. That's how the framework gets better.

## Part of the stack

| Repo | Role |
|------|------|
| [Build Bible](https://github.com/lee-fuhr/build-bible) | Methodology -- how to build with agents |
| [Memeta](https://github.com/lee-fuhr/memeta) | Memory -- what Claude remembers across sessions |
| [ai-ops-starter](https://github.com/lee-fuhr/ai-ops-starter) | Scaffolding -- the folder structure and templates |

---

MIT -- see [LICENSE](LICENSE)
