---
name: domain-modeling
description: Build and sharpen a project's domain model. Use when discussing codebase terminology, writing or editing a CONTEXT.md, or recording or editing an ADR.
---

# Domain Modeling

Actively build and sharpen the project's domain model as you design — challenging terms, probing boundaries with scenarios, and writing the glossary and decisions down the moment they crystallise. (Merely *reading* `CONTEXT.md` for vocabulary is not this skill — that's a one-line habit any skill can do. This skill is for when you're changing the model, not just consuming it.)

The bar is ruthless: every domain noun and verb that appears in the session ends it in exactly one of three states — **canonical** in `CONTEXT.md`, **retired** under `_Avoid_`, or an **open question** put to the user. Nothing floats undefined.

## File structure

Most repos have a single context:

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

If a `CONTEXT-MAP.md` exists at the root, the repo has multiple contexts. The map points to where each one lives:

```
/
├── CONTEXT-MAP.md
├── docs/
│   └── adr/                          ← system-wide decisions
├── src/
│   ├── ordering/
│   │   ├── CONTEXT.md
│   │   └── docs/adr/                 ← context-specific decisions
│   └── billing/
│       ├── CONTEXT.md
│       └── docs/adr/
```

Create files lazily — only when you have something to write. If no `CONTEXT.md` exists, create one when the first term is resolved. If no `docs/adr/` exists, create it when the first ADR is needed.

## During the session

### A term earns its place

A term becomes canonical by surviving challenge, never by being asserted — the user's word and your own proposals face the same bar. Before a term resolves, sweep the repo for the concept: the term, its likely synonyms, the names the code actually uses — and bring the evidence. "The code says `account` in 14 places and `customer` in 3, and they point at two different tables." Where the field has an established vocabulary — a standard, an RFC, the domain's literature — check the term against it; diverging from the standard meaning is allowed, deliberate, and recorded. And a name that changes no scenario's outcome and no code's shape is furniture, not vocabulary — leave it out.

### Reconcile against the glossary

The moment a term lands — the user's or yours — reconcile it against every existing entry: a conflict with the standing language, an overlap with a neighbouring term, a boundary it blurs. Call it out immediately: "Your glossary defines 'cancellation' as X, but you seem to mean Y — which is it?" A collision jumps the queue — settle it before anything is built on top of it.

### Sharpen fuzzy language

When the user uses vague or overloaded terms, propose a precise canonical term. "You're saying 'account' — do you mean the Customer or the User? Those are different things." One name per concept, one concept per name: two names for one thing, and one name for two things, are defects to hunt, not accidents to note in passing.

### Probe until the boundary is exact

Stress-test every definition and relationship with concrete scenarios, inventing them until one breaks the definition or three in a row fail to. Aim at the borderline: "Is a paused subscription an active Customer?" A definition that cannot classify a borderline case is not yet a definition — sharpen it and probe again.

### Cross-reference with code

When the user states how something works, check whether the code agrees — and when a term resolves, check where the code disagrees with it. Surface every contradiction with its evidence attached: "Your code cancels entire Orders in `orders/cancel.ts`, but you just said partial cancellation is possible — which is right?" Report the disagreement's full extent — the files, the count; whether to rename anything is the user's decision.

### Update CONTEXT.md inline

When a term is resolved, update `CONTEXT.md` right there. Don't batch these up — capture them as they happen. Use the format in [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md).

Thoroughness spends on coverage and challenge, never on entry length: definitions stay one or two sentences, and the glossary gets shorter as often as it gets longer. `CONTEXT.md` stays totally devoid of implementation details. Do not treat it as a spec, a scratch pad, or a repository for implementation decisions. It is a glossary and nothing else.

### Offer ADRs sparingly

Only offer to create an ADR when all three are true:

1. **Hard to reverse** — the cost of changing your mind later is meaningful
2. **Surprising without context** — a future reader will wonder "why did they do it this way?"
3. **The result of a real trade-off** — there were genuine alternatives and you picked one for specific reasons

If any of the three is missing, skip the ADR. When one clears the bar, it carries its receipts: gate 3 guarantees alternatives existed, so record which lost and why. Use the format in [ADR-FORMAT.md](./ADR-FORMAT.md).

## Done

The session's language is closed when every domain noun and verb it used has landed in one of the three states — canonical, retired, or explicitly open with the user; every new entry has survived a borderline scenario; and every contradiction surfaced between the language and the code has been resolved or recorded as open. An open item is the user's to close, never silently dropped.
