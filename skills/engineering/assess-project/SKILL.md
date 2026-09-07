---
name: assess-project
description: Grade a project's implementation against its architecture docs — implementation gaps, ranked improvements, patterns and structure, and strengths and weaknesses with pitch-ready framing.
disable-model-invocation: true
---

Write the report an incoming tech lead would deliver after their first deep week: what the docs promise, what the code delivers, where the two diverge, and what the project is genuinely strong and weak at.

One rule defines a verdict: **graded evidence**. "Built" means the code exists *and* meets the doc's own acceptance bar — the bar the docs set is the bar the code is graded against, never generic best practice. A strength reaches the pitch section only after surviving that grading.

## 1. Two inventories

- **The promises** — locate the docs corpus: `docs/`, `specs/`, `architecture/`, `adr/`, `CONTEXT.md`, the README, any path the user names.
- **The reality** — the code tree: languages, module layout, build and test entry points, CI config.

Degenerate cases exit in one paragraph, before any fan-out. No code: the assessment is one fact — every promise is ungraded because nothing is built — and the useful run today is `/audit-docs`. No docs: there is no promise ledger to grade against, and the code-only survey is `/improve-codebase-architecture`.

## 2. The promise ledger

Fan out sub-agent readers over the docs, each writing to `.scratch/assess-project/promises/<doc-slug>.md`: every capability the doc promises, each with the doc's own acceptance bar — acceptance criteria, test matrices, stated bounds — quoted verbatim with file and section. This ledger is the checklist the whole assessment grades against.

## 3. The reality pass

Fan out readers over the code, one per module or area, each writing to `.scratch/assess-project/reality/<area-slug>.md`: what the area actually implements; the tests that cover it; the patterns and idioms in use, named concretely (the error type, the concurrency shape, the layering); and load-bearing behavior no doc owns. Run the repo's own test command where one exists and runs cheaply; otherwise grade on the tests' existence and CI config — and the report says which evidence each grade used.

## 4. Grade every promise

Match ledger to reality. Four verdicts, each carrying receipts from both sides — the doc quote and the code path:

- **Built** — code exists and the doc's own bar is met: the acceptance criteria have tests, and the tests pass.
- **Partial** — code exists, but the bar is unmet or only part of the promise landed. Name the missing part precisely.
- **Absent** — promised, no code.
- **Undocumented** — the reverse drift: load-bearing code no doc owns.

Before any Absent or Partial verdict is final, search for the code under other names — a module rarely spells itself the way its spec does. A gap the reader disproves with one grep discredits the whole report.

## 5. Patterns and structure

Draw the map as it actually is: module boundaries versus the boundaries the docs draw, and the recurring patterns, good and bad. A pattern needs three sightings to earn the name; below that it is an incident. Where structure and docs diverge, that divergence is itself a finding — either the code drifted or the doc was always aspirational, and the evidence says which.

## 6. Improvements

Ranked suggestions, each traced to a graded finding — a gap to close, a duplicated pattern to consolidate, a boundary to move — with the consequence walked: "collapsing the two retry policies deletes one config surface and one class of double-send." An improvement that traces to no finding is opinion, and dies before the report.

## 7. Strengths, weaknesses, and the pitch line

- A **strength** is a promise graded Built that is also distinctive — the docs' own comparisons, numbers, or cited sources say why it beats the obvious alternative. Write each twice: the **engineer's line**, mechanism and number ("merges are rebase-canonical, so history stays linear and the merge gate holds the invariant"), and the **pitch line**, the outward phrasing — and the pitch line may contain nothing the engineer's line doesn't back.
- A **weakness** is its walked consequence, stated plainly: "no auth story — any process that can reach the socket owns the cluster."
- Partial and Undocumented work is the honesty section: what a demo would show that the docs can't back, and what the docs claim that a demo would embarrass.

## 8. Deliver

Write `ASSESSMENT.md` in the working directory (or where the user says): the grade table first — every promise, its verdict, its receipts — then patterns and structure, improvements, strengths and weaknesses. State coverage: docs read, areas read, tests run or not. Change nothing; the report is the deliverable. Point the user onward inside it: gaps worth building go through `/grill-with-docs` and `/to-spec`; pitch material feeds `/distill-docs`; deepening candidates go to `/improve-codebase-architecture`.

## Done

- Every promise in the ledger graded with receipts on both sides; every Absent and Partial verdict survived the search for the code under other names.
- Every named pattern has three sightings; every improvement traces to a finding.
- Every pitch line is backed by its engineer's line; every weakness is a walked consequence.
- `ASSESSMENT.md` written, coverage stated, nothing in the repo changed.
