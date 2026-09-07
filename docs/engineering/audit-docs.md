## What it does

`audit-docs` reads a repo's entire architecture-docs corpus — specs, architecture docs, ADRs, `CONTEXT.md` — as one system and reports where the design disagrees with itself (**conflicts**), where it trails off (**gaps**), and where it leaves value on the table (**missed opportunities**). A finding only counts when it is news: anything the corpus already records about itself — an open-questions list, a gap ledger, a TODO, an explicitly deferred decision — is the corpus working, not a finding, so the report contains only what the docs don't know about themselves.

It gets there without trusting summaries. A corpus of forty specs is far bigger than one [context window](https://www.aihero.dev/ai-coding-dictionary/context-window), and a summary drops exactly the details that conflict — so each [sub-agent](https://www.aihero.dev/ai-coding-dictionary/subagent) reader extracts verbatim **claims** instead: the contracts a doc *exports* to the rest of the system, and the assumptions it *imports* about other components, each a quote with file and section. Reconciling imports against exports is what surfaces the conflict a summary would have smoothed over, and every candidate finding is then re-verified against the source docs — an attempt to refute it — before it reaches the report.

## When to reach for it

You invoke this by typing `/audit-docs` — the agent won't reach for it on its own.

Reach for it when a docs corpus has grown past what anyone holds in their head and you want to know whether it still agrees with itself — before building on it, after a long design push, or when two specs smell like they're describing different systems. It audits docs against docs only:

| What you want checked | Reach for |
| --- | --- |
| Docs against each other — internal consistency of the design | `audit-docs` |
| Docs against the code — what's actually built | [assess-project](https://aihero.dev/skills-assess-project) |
| Docs turned into a README, blurb, or pitch | [distill-docs](https://aihero.dev/skills-distill-docs) |

## Claims, not summaries

The unit of the audit is the **claim**: a verbatim, located quote of something one doc asserts. Exports are the guarantees a doc makes ("the queue delivers at-most-once"); imports are the assumptions other docs stake on it ("SCHEDULER retries because delivery is at-least-once"). Grouping every import under its subject and pairing the group with the subject's exports produces the **edge index** — and each edge either reconciles or is a finding: a conflict when the claims collide, a gap when a subject is imported everywhere and exported nowhere, terminology drift when one term carries two meanings. Missed opportunities come from the same map read the other way: every hole checked against every capability, hunting the machinery one doc already specified that another doc hand-rolls or leaves open.

## The report

Findings land in `DOCS_AUDIT.md`, ranked by blast radius — how much of the corpus sits on the answer. Each finding is one plain sentence, the quotes from both sides, and the consequence walked as a scenario ("a builder following SCHEDULER retries the send; a builder following QUEUE deduplicates nothing; the message double-fires at the first crash") rather than an adjective. The audit changes no docs: findings are decisions waiting to be made, and the report points them at [grill-with-docs](https://aihero.dev/skills-grill-with-docs) to be settled.

## Common questions

**My repo already keeps its own gap ledger — what happens to it?**
The audit reads it first, and everything it records is excluded from the findings: a gap the corpus already knows about is not news. The report is the delta against the corpus's self-knowledge; folding confirmed findings back into your own ledger is your move afterwards.

**How is this different from just asking the agent "are my docs consistent?"**
An unstructured pass reads a few docs, summarizes, and misses the cross-references — conflicts live in details summaries drop. The skill forces verbatim claim extraction over every doc, an exhaustive edge check, and refutation of each finding against the sources before it's reported, with receipts you can check yourself.

## It's working if

- Every finding carries two verbatim quotes with file and section — you can open both and see the collision yourself.
- Every finding is news to the corpus: nothing in the report duplicates your own TODO lists, open-questions sections, or gap ledger.
- Conflicts arrive as walked scenarios — what two builders would each do, and where they'd collide — never as "these docs are inconsistent."
- The report ends with coverage: every doc read, every edge checked, and anything capped said out loud.
- No doc was edited; you got findings, not silent fixes.

## Where it fits

A reach-for-it-anytime standalone — periodic docs upkeep, the docs-side sibling of [improve-codebase-architecture](https://aihero.dev/skills-improve-codebase-architecture)'s code survey. Its findings are raw decisions, which makes [grill-with-docs](https://aihero.dev/skills-grill-with-docs) the natural next step because conflicts and gaps need settling, not just listing; [assess-project](https://aihero.dev/skills-assess-project) is the sibling that grades the same corpus against the implementation instead. When you're unsure which entry point fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
