## What it does

`grilling` is the interview loop that stress-tests a plan, a decision, or a system design before anyone acts on it. It maps the subject as a **design tree** — every decision branches into the decisions that hang off it — and works the tree with you like a well-tenured architect: one decision at a time, evidence first, across as many [sessions](https://www.aihero.dev/ai-coding-dictionary/session) as the design takes. Every recommendation and every pushback it makes carries **receipts** — a measurement, a bound, a cited result, a production number it went and found — never an unsupported opinion. And a decision does not count as settled when you pick a direction: it counts as settled when you have accepted its **spec** — a practical implementation example, test cases with their relevance explained, and exacting acceptance criteria.

It researches without being asked. Before arguing a substantial decision it dispatches [sub-agents](https://www.aihero.dev/ai-coding-dictionary/subagent) to search peer-reviewed work (arXiv and published venues), academic texts, and engineering blogs from companies operating at scale (Netflix, Uber, Google, Meta, AWS, Cloudflare), alongside the local [environment](https://www.aihero.dev/ai-coding-dictionary/environment). It weighs sources in that order: a well-cited paper outranks a textbook, and a company blog counts as an experience report — one data point from one workload, valued for the production numbers it contains. Your own empirical data joins the picture as its own source — the only one describing your actual workload, and scrutinized like the rest. It asks you to bring your benchmarks and profiles when a decision turns on them, then asks how they were collected — sample bias, what was warmed, what was mocked — and investigates when your numbers disagree with well-established results, since a flawed benchmark and a genuinely unusual workload look identical until checked. When you have none, it offers to procure the numbers itself, by setting up profiling locally or running the deeper research. Throughout, trust tracks verifiability: a number earns weight from a source it can name or a run that can be repeated, and data offering neither stays suspect — yours, the literature's, or its own.

## When to reach for it

Type `/grilling`, or the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) reaches for it on its own when a task fits. It is the only [skill](https://www.aihero.dev/ai-coding-dictionary/skill) in the grilling family that is model-invoked, which is why you rarely type it: usually a skill you *did* type is running it for you.

Typing `/grilling` directly gets you the plain interview and nothing else. Where you want something more than that:

| What you have | Reach for |
| --- | --- |
| You aren't working in a working directory | [grill-me](https://aihero.dev/skills-grill-me) — the same session, under a name the agent will never fire by itself |
| You are in a working directory | [grill-with-docs](https://aihero.dev/skills-grill-with-docs) — the same session, and it writes `CONTEXT.md` and ADRs as it goes |
| An effort too big to hold in one session | [wayfinder](https://aihero.dev/skills-wayfinder) — it charts a map and runs grilling inside the decision tickets |
| A question that talking cannot settle — how something should look or feel | [prototype](https://aihero.dev/skills-prototype) — build the throwaway version, then come back |
| A skill of your own that needs an interview | Invoke `/grilling` from it, rather than writing another interview |

## The exchange

The unit of the session is an **exchange**, not a questionnaire. The skill picks the open decision with the most riding on it and brings you: the decision, plainly stated; the evidence its research found, numbers attached; and one or more recommendations, argued from practical impact — latency added, failure modes removed, code avoided, with theoretical bounds as supporting justification rather than the headline. When more than one option is credible it presents several, side by side, with the advantages, disadvantages, and tradeoffs of each stated clearly and its own favorite named. Then it stops and waits. Your answer reshapes the tree and unblocks the next exchange. Small, genuinely independent questions may arrive grouped, but a load-bearing decision always travels alone.

## Settling a decision

Picking a direction chooses it; accepting its **spec** settles it. Once you pick, the skill drafts the item's spec and puts it in front of you for correction: a practical implementation example in your stack, small enough to review; test cases, each with its relevance explained — what failure it catches and why that failure matters here; and acceptance criteria that are exacting, numerous, and checkable — a bound, a behavior under a named condition, an invariant, never "works correctly". You correct, it redrafts, and the item is settled only when you accept. Feedback that reopens the direction reopens the branch — that is the process working, not failing. A design worth this rigor rarely settles in one sitting: a session that ends with open branches writes the tree to `GRILLING.md` in the working directory and resumes from it next time.

The split between facts and decisions still carries half the design. Facts are the skill's job: anything the environment or the literature can settle, it goes and finds rather than asking you, and it researches the likely next decisions in the background while you weigh the current one. Decisions are yours, and it must wait for them. An agent running `grilling` that answers its own decisions has broken the skill, not interpreted it liberally.

## The ledger

Beside each settled decision the design tree records the costs you accepted with it — the latency added, the consistency given up, the operational burden taken on. Every new answer is checked against that **ledger** the moment it lands, for two things: a contradiction — "the API is stateless," then three exchanges later, "keep sessions in an in-memory cache" — and compounding tradeoffs, where downsides that are fine alone stack past a budget together. Either one jumps the queue: it comes back as the very next exchange, both decisions named and the joint cost quantified, and you choose which one bends — while both sides are still cheap to reopen, rather than in a wrap-up after work has piled on top.

Reconciliation looks backward; each answer also grows the tree forward. A decision that lands — a reframe above all — spawns child decisions: the lifecycle it now needs, the states, the failure semantics, the monitoring. The skill enumerates those children on the spot and enters them as open branches; hunting the gap a change just opened is its job, before it becomes yours.

## What it optimizes for

Correctness first, then robustness, then efficiency and performance — and against all four, size. The best design meets the requirements with the fewest moving parts, so every component, layer, and abstraction in your design has to pay for itself; when one doesn't, the skill challenges it with what it costs. Expect pushback whenever the evidence disagrees with you, in plain language and with the receipts. You can overrule it — the decisions are yours — and when you do, it records the decision and your reasoning and moves on.

## Common questions

**Can I get the old batched rounds back?**
Yes. Earlier versions asked in rounds — the whole set of unblocked questions at once. If you preferred that rhythm, add this to your global `CLAUDE.md`:

```
When grilling, batch all currently-unblocked questions into one round.
```

The one-decision-at-a-time exchange is the default because a round of thirteen questions reads as a form to fill in, and pushback buried in item nine goes unread.

**Where did `/batch-grill-me` go?**
Into history. Round-based questioning shipped briefly as a separate skill, then moved into `grilling` itself, and has since been replaced by the evidence-first exchange. There is no `batch-grill-me` to install; the `CLAUDE.md` line above is the way back to batching.

**Does it really research every question?**
Substantial decisions, yes — that is the point. Small independent questions arrive grouped and unresearched. If the evidence is thin or conflicting, it is supposed to say so plainly rather than manufacture confidence; a session citing sources it cannot name is a session to challenge.

**It enthusiastically agreed with my whole design and asked me to ratify it as a bundle.**
That is the skill not running — sycophancy dressed as synthesis. The tells: a verdict in the opening sentence, no citations or numbers anywhere, several load-bearing decisions bundled into one "ratify", and no spec — no implementation example, test cases, or acceptance criteria — ever put up for correction. The skill's rules are the remedy, and you can quote them back at it: one decision at a time, receipts attached before an exchange is sent, agreement argued as rigorously as pushback with the verdict landing last, and a spec owed for every direction chosen. A session showing none of those fingerprints may not have loaded this skill at all — ask it directly.

The same disease has an input-side variant: you raise a point or correction, and it replies with instant validation ("correct — and it snaps into place") followed by a flurry of edits — no research run, and the follow-on decisions your change opened (the lifecycle, the states, the monitoring) left for you to notice. The rule it skipped: a point you raise is impetus, never a verdict — it gets investigated and answered with an exchange, and an accepted reframe comes back with the child decisions it spawns.

**It ran out of questions and started building.**
A confirmation gate exists precisely for this: the skill is not finished when the tree is settled, it is finished when you say the understanding is shared. Weaker and faster [models](https://www.aihero.dev/ai-coding-dictionary/model) still break it. If yours does, the reliable fix is a line in your own `AGENTS.md` or `CLAUDE.md` telling the agent not to implement without permission.

**It answered its own questions instead of asking me.**
That is a bug in the run, not the intended behaviour, and it is the reason facts and decisions are separated in the skill's text. It shows up most when another skill runs `grilling` inside a resolve-this-ticket frame, where the surrounding task reads as licence to keep moving.

**Can I cap the number of exchanges?**
No, and a cap is deliberately out of scope. Some designs need three exchanges and some need fifty. Steering in plain language is the intended control — tell it to wrap up, or stop and accept the design where it stands. A session running very long usually means the scope was too big; break the work up and grill the pieces.

**I installed `grill-me` on its own and nothing happens.**
`grill-me` is a one-line skill whose whole body is "run a `/grilling` session", so it needs this skill installed too. The same is true of `grill-with-docs`, which additionally needs [domain-modeling](https://aihero.dev/skills-domain-modeling). Installing the whole set avoids the problem.

**`grill-with-docs` ran, but it never loaded `grilling`.**
A real and unfixed rough edge, reported across [harnesses](https://www.aihero.dev/ai-coding-dictionary/harness) and models: a skill that names another skill does not reliably cause that skill to load, and `grill-with-docs` names two. The tell is a session that asks everything at once with no evidence or recommendations attached — that is the model improvising an interview rather than running this one. Asking the agent directly whether it loaded `grilling` and `domain-modeling` usually recovers it.

## It's working if

- Decisions arrive one at a time, each with the evidence and one or more recommendations — and when there are several, the tradeoffs of each are laid out side by side with a favorite named.
- A decision you pick comes back as a spec — implementation example, test cases with their relevance explained, acceptance criteria you could hand to a tester — and it waits for your corrections before calling the item settled.
- A session that runs out of time leaves a `GRILLING.md` you can resume from, with settled specs and open branches both visible.
- Pushback comes with a number, a bound, or a citation you could go and check, and it names its sources.
- Agreement is argued like pushback — the receipts, the alternative your idea beats, the risks it still carries — with the verdict at the end of the exchange, never the start.
- Your own suggestions get investigated before they get endorsed, and an accepted reframe comes back with the child decisions it opens — the lifecycle, the states, the failure modes — before you have to point them out.
- It asks you to settle decisions one at a time; nothing arrives as a bundle to ratify.
- Recommendations lead with practical impact — what it costs or saves you — with the theory in support, not in front.
- It goes and looks facts up — the repo, the literature, a sub-agent dispatch — rather than asking you something it could have found out.
- When a decision turns on numbers nobody has, it asks for your benchmarks and profiles first, then offers to measure locally or research the closest published equivalents — the decision waits on data, never on a guess.
- Your own numbers get questioned too: it asks how a benchmark was collected before leaning on it, and digs in when your data disagrees with the literature instead of siding with either by default.
- Research runs in the background; the conversation does not stall waiting for it.
- A choice that contradicts something you settled earlier — or stacks costs past a budget you named — comes back at you in the very next exchange, both decisions named, instead of surfacing at the end.
- It challenges components and abstractions that don't pay for themselves, and accepts your overrule by recording it rather than re-arguing it.
- It stops at the end, summarizes the settled design with the evidence and the rejected alternatives, and asks you to confirm the understanding is shared instead of starting work.

## Where it fits

`grilling` is a **primitive**, not a step you schedule: the single source of truth for the interview technique, kept in one place so every skill that needs an interview reaches for it instead of inventing one. [grill-me](https://aihero.dev/skills-grill-me) and [grill-with-docs](https://aihero.dev/skills-grill-with-docs) are its two user-invoked front doors, and `grill-with-docs` is where the main build chain begins, ahead of [to-spec](https://aihero.dev/skills-to-spec). [wayfinder](https://aihero.dev/skills-wayfinder) runs it to resolve decision tickets, [triage](https://aihero.dev/skills-triage) to grill a vague report into a workable one, and [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture) to walk the tree once you have picked a candidate to deepen. When you are unsure which entry point fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
