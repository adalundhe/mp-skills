---
name: audit-docs
description: Audit a repo's architecture docs as one corpus — cross-doc conflicts, gaps, and missed opportunities, every finding verified against the sources and delivered with receipts.
disable-model-invocation: true
---

Read the corpus the way an incoming principal engineer would before betting a year on it: every doc, assuming nothing, hunting the places where the design disagrees with itself, trails off, or leaves value on the table.

One rule defines a finding: **a finding is news**. Anything the corpus already records about itself — an open-questions list, a gap ledger, a TODO, a decision explicitly deferred — is the corpus working, not a finding. You hunt what the docs don't know about themselves.

## 1. Inventory

Locate the corpus: `docs/`, `specs/`, `architecture/`, `adr/`, `rfcs/`, `design/`, plus `CONTEXT.md`, the README, and any path the user names. List every file with its size, and note the corpus's own conventions — status headers, a glossary, an existing gap ledger. The ledger defines what is already known (and therefore not news); the glossary anchors the terminology check.

State the plan before dispatching: how many docs, how many readers, where extractions land. Every doc gets read; if you cap anything, the report says so.

## 2. Extract claims, never summaries

A summary drops exactly the details that conflict. Dispatch sub-agent readers — one doc each (a few small docs may share a reader, sized so the reader quotes everything relevant rather than skimming) — and have each write an extraction to `.scratch/docs-audit/<doc-slug>.md` with this schema, every entry a **verbatim quote plus file and section** (a paraphrase launders the very wording a conflict lives in):

- **Exports** — every contract, invariant, or guarantee this doc declares that the rest of the system must honor.
- **Imports** — every claim this doc makes about another component or doc: what it assumes, asserts, or cites, and the subject it names.
- **Terms** — terms this doc defines; terms it uses with a load-bearing meaning.
- **Deferred** — what the doc itself records as open, TODO, or out of scope: the not-news list.
- **Status** — acceptance state, dates, anything it claims to supersede.

## 3. Reconcile

Build the **edge index** from the extraction files: group every import by its subject, pair each group with the subject's own exports. Work from the extractions; reopen a source doc only to verify. Check every edge for:

- **Conflicts** — an import contradicts the subject's export ("SCHEDULER assumes the queue delivers at-least-once; QUEUE guarantees at-most-once"); two docs settle the same decision differently; a doc contradicts itself across sections; a cited section that doesn't exist or no longer says what's cited. Where both sides carry status dates, the newer accepted doc stands and the stale citation is the finding.
- **Gaps** — a subject imported by two or more docs that no doc exports: named everywhere, owned nowhere. And lifecycle holes: a thing created but never destroyed, an error path entered but never exited, a migration from the old world unaddressed.
- **Terminology drift** — one term carrying different meanings in different docs, checked against the glossary when one exists.

## 4. Hunt missed opportunities

The reconciled map is a vantage no single doc has. Walk two passes over it:

- Every gap and every deferred item against the full export list: does machinery already specified elsewhere close it? ("FANOUT already guarantees ordered delivery; SESSIONS §4's hand-rolled sequence numbers duplicate it.")
- Every pair of docs solving the same-shaped problem differently: two retry policies, two ad-hoc caches, two permission checks. One mechanism doing both jobs is a spec deleted.

The bar: every hole checked against every capability before you conclude nothing composes.

## 5. Verify

Every candidate finding goes back to the primary sources before it reaches the report: re-read the exact sections cited and try to refute it. A third doc may resolve the conflict; a "gap" may be recorded as deferred (not news); the "duplicate machinery" may exist for a reason the doc states. A finding survives only with quotes from both sides attached; a finding you cannot evidence dies here, not in front of the user.

## 6. Report

Write `DOCS_AUDIT.md` in the working directory (or where the user says): `## Conflicts`, `## Gaps`, `## Missed opportunities`, ranked within each section by blast radius — how much of the corpus sits on the answer. Every finding shows, not tells:

- The claim, in one plain sentence.
- The receipts: both quotes, file and section each.
- The consequence, walked: "a builder following SCHEDULER retries the send; a builder following QUEUE deduplicates nothing; the message double-fires at the first crash."
- For a conflict: which docs must bend, and what bending costs each side.

Close with coverage — every doc read, every edge checked, anything capped — so silence reads as "covered" only when it is true. Findings are the whole deliverable: change no doc. Point the user onward: conflicts and gaps are decisions, and `/grill-with-docs` is where they get settled.

## Done

- Every doc in the inventory extracted; every import edge reconciled against its subject's exports; every gap checked against the export list for an existing answer.
- Every reported finding verified against the sources, quotes attached, and news — nothing the corpus already records about itself.
- `DOCS_AUDIT.md` written, findings ranked by blast radius, coverage stated, no doc changed.
