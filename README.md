# Atlas

You've got 40 components in your Claude Code system. You can't tell where new ones go.

---

## The problem

An AI-augmented setup grows fast. You start with a CLAUDE.md. Then a hook. Then three agents, a background job, a memory system, a skill library. Before long you're asking: does this go in `rules/` or in `agents/`? Is this hook behavior or is it a skill? Why does Claude place similar things differently across sessions?

The problem isn't complexity -- it's that you don't have a shared model. Without one, every placement decision is improvised, Claude makes different calls in different contexts, and you spend more time managing structure than doing work.

The drift is subtle at first. You find three versions of the same instruction in different places. A hook that should be a rules file. Knowledge that fires automatically when it should sit quietly. Six months in, the system is working against you.

## What changes

**Every component has an obvious home.** The KCA model gives three layers (Knowledge, Capability, Activity) and six sub-layers. The 3 placement tests give unambiguous answers. You stop debating and start placing.

**Claude makes consistent decisions.** When Claude knows the model, it places new components the same way every session. The 3 tests become a decision procedure that generalizes -- even to components that didn't exist when you first built your system.

**You see how your system improves itself.** The 6 feedback arrows describe how information flows between layers: how principles constrain behavior, how experience drives adaptation, how activity captures knowledge. You stop wondering why things work (or don't).

## What's in it

- **KCA model** -- Knowledge / Capability / Activity, three layers, six sub-layers. Every component in any AI system belongs in exactly one.
- **3 placement tests** -- consult-vs-fire, authority-record, stability-as-property. Ask them in order and get an unambiguous answer every time.
- **6 feedback arrows** -- how principles constrain capability, how experience drives adaptation, how activity escalates to principles. The loops that keep the system alive and improving.
- **Precedence hierarchy** -- when directives conflict, this tells Claude which one wins.
- **A worked example** -- one real AI-augmented system mapped onto KCA, so you see how the framework applies before you apply it yourself.
- **ADAPTING.md** -- how to create your own Atlas for your specific system.

**Add it to Claude Code:** Point `~/.claude/rules/atlas.md` at `atlas-framework.md`. Claude knows the model every session. No setup required -- just a reference.

---

## Part of the stack

| Repo | Role |
|------|------|
| [Build Bible](https://github.com/lee-fuhr/build-bible) | Methodology -- how to build with agents |
| [Memeta](https://github.com/lee-fuhr/memeta) | Memory -- what Claude remembers across sessions |
| [ai-ops-starter](https://github.com/lee-fuhr/ai-ops-starter) | Scaffolding -- the folder structure and templates |

---

MIT -- see [LICENSE](LICENSE)
