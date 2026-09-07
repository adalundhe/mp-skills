---
name: distill-docs
description: Distill a repo's architecture docs into outward-facing writing — a README, a blurb, a pitch, an overview — every claim traced to a source doc and honest about what's built versus designed.
disable-model-invocation: true
---

Turn a docs corpus into the small thing someone will actually read. The corpus is the primary source and the artifact is a distillation: nothing in it the docs don't back, and nothing that outruns the project's real maturity.

## 1. The brief

Pin these from what the user already said, asking one round of questions for only what's genuinely missing:

- **Artifact(s)** — README, blurb, pitch, overview, landing page. Several can share one run.
- **Audience and register** — engineers evaluating, a directory listing, investors. The register decides what counts as a proof point.
- **Length** and **home** — the file path each artifact lands at.

Then read the repo for the **maturity line** — what is designed, built, tested, shipped: code present, tests, release tags, status headers, a gap ledger. Every artifact sits on the correct side of that line. "Spec-complete, pre-implementation" is a strong and honest pitch; "battle-tested" with no production behind it is fiction.

## 2. Extract story material

Locate the corpus — `docs/`, `specs/`, `architecture/`, `adr/`, `CONTEXT.md`, any path the user names — and fan out sub-agent readers, each writing to `.scratch/distill-docs/<doc-slug>.md`:

- The **problem** the doc addresses, in one plain sentence.
- The **mechanism** — how it actually works, concrete enough to explain to an outsider.
- The **distinctive** — what it does differently from the obvious alternative, and the doc's own stated reason.
- **Proof points** — numbers, bounds, named sources, test counts. Verbatim, with file and section.
- **Status** — accepted, draft, built.
- The doc's **best lines** — verbatim sentences worth stealing.

Story material, never summaries: a reader that returns "this doc describes the scheduler" has returned nothing.

## 3. The spine

Choose each artifact's narrative from the extractions: the problem, the approach, what's distinctive, the proof. Most material dies here — the corpus is a thousand times the artifact's size, and the artifact is defined by what it leaves out. Pick the three to five ideas the audience must leave with; everything else becomes a link into the docs, not a paragraph.

## 4. Draft

Write in the register, plainly, showing rather than telling: a strength is its mechanism or its number ("one binary, laptop to fleet — the same code path, no mode flag"), and an adjective is cashed in its own sentence — "fast" arrives with the bound, "simple" with the count. The marketing lives in the selection and the rhythm, never in inflation: state the maturity line in-register, and make every claim one the fact-check will find in the corpus.

## 5. Fact-check

A fresh sub-agent takes the draft and the extraction files and audits it claim by claim: every factual claim traced to a doc and section, every untraceable claim listed. Unsupported claims are corrected or cut — hedging a false claim keeps it false. The maturity line gets the same check.

## 6. Deliver and correct

Deliver each draft with its claim-to-source map in your message — inside the artifact only where the genre wants citations. A file that already exists is replaced only after the user accepts; until then the draft lives beside it as `<name>.draft.md`. The user corrects, and the user outranks the corpus on their own project: a correction the corpus can't back gets written as they say it and flagged as the docs being behind — material for `/audit-docs`.

## Done

- The brief pinned; the maturity line established from the repo, never assumed.
- Every doc in the corpus extracted; each artifact's spine chosen; each draft in-register.
- Every claim in the delivered artifact traced to its source, and none outruns the maturity line.
- The user has accepted, and no pre-existing file was overwritten before acceptance.
