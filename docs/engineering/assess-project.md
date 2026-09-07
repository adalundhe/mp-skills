## What it does

`assess-project` grades a project's implementation against its own architecture docs and delivers the report an incoming tech lead would write after their first deep week: every promise in the docs graded **Built**, **Partial**, **Absent**, or **Undocumented** (the reverse drift — load-bearing code no doc owns), the patterns and structure as they actually are, ranked improvement suggestions, and strengths and weaknesses framed for the outside world. Every verdict is graded evidence — the bar a promise is graded against is the doc's *own* acceptance criteria and test matrix, never generic best practice, and "Built" means those criteria have passing tests.

The outward framing is the same discipline applied to marketing. A strength is a promise that graded Built *and* is distinctive, and it's written twice: the **engineer's line** — mechanism and number — and the **pitch line**, which may contain nothing the engineer's line doesn't back. Weaknesses arrive as walked consequences ("no auth story — any process that can reach the socket owns the cluster"), never softened.

## When to reach for it

You invoke this by typing `/assess-project` — the agent won't reach for it on its own.

Reach for it when docs and code have both existed long enough to drift — before a release, a demo, a pitch, or a new hire ramping — and you want the honest delta between what's promised and what's real. The boundaries with its siblings:

| Your situation | Reach for |
| --- | --- |
| Docs and code both exist — grade one against the other | `assess-project` |
| Docs only, no code yet — check the design against itself | [audit-docs](https://aihero.dev/skills-audit-docs) |
| Code only, no docs — survey for deepening opportunities | [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) |
| You have the assessment and want the pitch written | [distill-docs](https://aihero.dev/skills-distill-docs) |

## The promise ledger

The mechanism is a two-sided match. [Sub-agent](https://www.aihero.dev/ai-coding-dictionary/subagent) readers fan out over the docs and build the **promise ledger** — every capability promised, with the doc's own acceptance bar quoted verbatim — while a second fan-out reads the code and writes the reality: what each module implements, what its tests cover, which patterns it uses. Grading is matching ledger to reality with receipts on both sides (the doc quote and the code path), and no Absent or Partial verdict is final until the code has been searched for under other names — a module rarely spells itself the way its spec does. Patterns need three sightings to earn the name, and every improvement suggestion must trace to a graded finding or it dies as opinion.

## Common questions

**What if there's no code yet?**
The skill exits in one paragraph: every promise is ungraded because nothing is built, and it points you at [audit-docs](https://aihero.dev/skills-audit-docs) — with no implementation, the useful audit is the design against itself.

**Does it run my test suite?**
When the repo has its own test command and it runs cheaply, yes — "Built" wants passing tests, not present tests. When running them isn't practical, it grades on test existence and CI config, and the report says which evidence each grade used.

**Isn't the "marketing speak" section just asking for inflation?**
It's the opposite: the pitch line is constrained to what the engineer's line backs, and only Built-and-distinctive promises reach it. That's what makes the framing usable — every outward claim traces to a receipt someone can check.

## It's working if

- Every promise appears in the grade table with a verdict and receipts on both sides — a doc quote and a code path you can open.
- Gaps survive your own grep: nothing the report calls Absent turns out to exist under another name.
- Each pitch line sits beside an engineer's line that backs it, and each weakness reads as a consequence you can picture, not a hedge.
- Improvements each point at the finding that motivated them; nothing reads as free-floating advice.
- Nothing in the repo changed; you got a report, not silent fixes.

## Where it fits

A reach-for-it-anytime standalone — the docs-versus-reality check that pairs with [audit-docs](https://aihero.dev/skills-audit-docs) (docs versus docs) to cover both drift axes. Its output feeds three directions: gaps worth building enter the main flow at [grill-with-docs](https://aihero.dev/skills-grill-with-docs) because a gap is a decision before it's a ticket, pitch material feeds [distill-docs](https://aihero.dev/skills-distill-docs), and structural findings hand candidates to [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture). When you're unsure which entry point fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
