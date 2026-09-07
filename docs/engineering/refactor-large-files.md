## What it does

`refactor-large-files` finds the oversized files an agent-built codebase accumulates — you give it a line threshold, `/refactor-large-files 1000` — and proposes the split each one actually wants: into modules, subpackages, or sibling files, shaped the way this repo already shapes things. Line count finds the candidates and never designs the split. A file is cut at a **seam**, never at a line budget, because splitting a 1200-line file into three 400-line files that reach into each other's internals produces three shallow modules and a tangle of imports — worse than the file you started with. A file with no seam stays one file, and the report says so with the reason.

It establishes your conventions before it proposes anything. The split has to look like it belongs, so it derives the house style from the repo's own well-organized multi-file modules — how directories group (by feature, by layer, by type), file naming and suffix idioms, whether directories are presented as one import path through a barrel or imported deep, and where tests live — then falls back to the language's module unit only where the repo is silent. Where the repo has a convention, the repo overrides.

## When to reach for it

You invoke this by typing `/refactor-large-files <N>` — the agent won't reach for it on its own. With no number it derives one from your repo's distribution and tells you what it picked.

Reach for it when files have quietly grown past the point where anyone reads them whole — the usual signature of a codebase built fast with agents. Its sibling covers the opposite failure:

| What's wrong | Reach for |
| --- | --- |
| One file does everything | `refactor-large-files` |
| Many files each do almost nothing | [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) |
| You want the vocabulary, not a scan | [codebase-design](https://aihero.dev/skills-codebase-design) |

Those first two are the same axis from opposite ends, which is exactly why this skill refuses to shred files to hit a number — a careless split manufactures the shallow modules the other skill exists to clean up.

## Seams, not line budgets

The evidence that a real split exists is **Divergent Change** — the file gets edited for several unrelated reasons — and `git log` shows it, read for distinct reasons rather than commit count. Several unrelated reasons means several responsibilities, and responsibilities are where the seams are. [Sub-agent](https://www.aihero.dev/ai-coding-dictionary/subagent) readers then map each offender's public surface (which exports actually have callers), its cohesion clusters (functions that touch the same state and ignore the rest), and — the part that kills bad splits — what each cluster would still need if it moved out.

Every proposed module states the **interface** it exports, which must be narrower than the surface it hides, and passes the **deletion test**: deleting it would concentrate complexity, not just relocate it. Every plan also names the recent real change it walked against the new layout, because a cut that turns a one-file edit into a four-file edit is Shotgun Surgery and the cut is in the wrong place.

Large is not automatically wrong, either. Exhaustive dispatch tables, data tables, and genuinely cohesive deep modules with small interfaces get listed as justified and left alone. If the repo's *median* file is enormous, that's a convention problem rather than a file problem, and the report says that instead.

## Common questions

**Does it refactor, or just report?**
Both, in that order. It reports first — `REFACTOR_PLAN.md`, no code touched — then puts the plan up for approval as a goal: the whole ranked batch or the subset you pick, approved once. On Claude Code that approval is [plan mode](https://www.aihero.dev/ai-coding-dictionary/agent-mode) doing what it is for. Then it works the approved files unattended, with the test suite gating every one: green recorded before, pure moves committed separately from logic edits so a reviewer can see nothing changed, green confirmed after, then `/code-review` against the commit that file started from. Where tests don't cover the moved code, characterization tests come first — without them nothing distinguishes a refactor from a rewrite.

Your judgment goes on the plan, once, where it decides which splits are right. Re-approving each mechanical move afterwards wouldn't add safety — the tests and the commit history are the safety — so it doesn't ask.

**When does it interrupt me?**
On a surprise, never on a schedule: tests that won't go green, a move that turns out to need logic edits, a split that opens a real design question, or a file whose diagnosis no longer matches what's on disk. Finishing a file that went to plan isn't worth an interruption; any of those is. It reports what landed and what stopped when the batch ends.

**Why did it leave my biggest file alone?**
Either it's justified — a data table, a closed dispatch, one cohesive deep module — or its clusters all need each other's internals, so there's no seam to cut at. The second case reports what would have to change first to make a seam possible. A file left whole with a reason is a real result, not a miss.

**What about Go, where files inside a package are basically free to split?**
That's called out explicitly: a package is the directory, so splitting a file within its package changes no imports anywhere. It's the cheapest split in any language, and the skill takes it freely rather than agonizing over the boundary.

## It's working if

- The report opens with your repo's own distribution and a written convention profile — and the proposals visibly follow it, down to naming and re-export style.
- Every offender's case cites `git` evidence of several unrelated reasons to change, not just its size.
- Every proposed module names the interface it will export, and that interface is smaller than what it hides.
- At least some big files come back marked justified or seamless-for-now, with reasons — a run that proposes splitting everything is measuring lines, not seams.
- Nothing in your repo changed before you approved the plan, and every split you approved landed with tests green on both sides and moves separated from edits.
- Once approved, it works the batch through without asking again — and when it does stop, it names the surprise that stopped it.

## Where it fits

Periodic maintenance, and the size-axis counterpart to [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) — run it every so often on a fast-moving codebase, the way you'd run a linter you actually read. It speaks [codebase-design](https://aihero.dev/skills-codebase-design)'s vocabulary throughout because a split is a module-design decision wearing a file-size trigger, and it closes an executed refactor through [code-review](https://aihero.dev/skills-code-review). When a proposed split turns out to need real design discussion rather than a move, take it into [grill-with-docs](https://aihero.dev/skills-grill-with-docs). When you're unsure which entry point fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
