## What it does

`distill-docs` turns a repo's architecture-docs corpus into the small thing someone will actually read — a README, a blurb, a pitch, an overview — with the corpus as primary source throughout. Every claim in the artifact traces to a doc and section, and nothing outruns the project's real **maturity line**: what's designed versus built versus shipped is read from the repo itself (code, tests, release tags, status headers) and stated in-register, so a spec-complete-but-unbuilt project pitches as exactly that — which is still a strong pitch, and a true one.

The writing follows the house discipline: show, don't tell. A strength arrives as its mechanism or its number ("one binary, laptop to fleet — the same code path, no mode flag"), an adjective is cashed in its own sentence, and the marketing lives in what gets selected and how it flows — never in inflation. A dedicated fact-check pass audits the draft claim by claim before you see it, and unsupported claims get corrected or cut, not hedged.

## When to reach for it

You invoke this by typing `/distill-docs` — the agent won't reach for it on its own.

Reach for it when the docs are rich but nobody outside the project will read forty specs — you need the README, the launch blurb, the pitch page, the one-pager for a stakeholder. Several artifacts can share one run: the corpus is ingested once and each artifact gets its own spine. The boundaries:

| What you want | Reach for |
| --- | --- |
| Outward-facing writing distilled *from* the docs | `distill-docs` |
| The docs checked for conflicts and gaps first | [audit-docs](https://aihero.dev/skills-audit-docs) |
| Verified strengths/weaknesses to feed the pitch | [assess-project](https://aihero.dev/skills-assess-project) |
| A message re-explained mid-conversation, not published writing | [wait-what](https://aihero.dev/skills-wait-what) |

## The spine

The corpus is a thousand times the artifact's size, so the artifact is defined by what it leaves out. [Sub-agent](https://www.aihero.dev/ai-coding-dictionary/subagent) readers extract **story material** — the problem, the mechanism, what's distinctive, the proof points, each doc's best verbatim lines — and the **spine** is the narrative chosen from it: the three to five ideas the audience must leave with. Everything that doesn't make the spine becomes a link into the docs, not a paragraph. The brief (artifact, audience, register, length) is pinned in one round of questions before any reading starts, because the register decides what counts as a proof point — an investor and an evaluating engineer are convinced by different receipts.

## Common questions

**Will it overwrite my existing README?**
Not before you accept. A file that already exists is drafted beside as `<name>.draft.md` and only replaces the original once you approve; new files are written directly.

**What happens when I correct something the docs don't say?**
You outrank the corpus on your own project: the artifact is written your way, and the mismatch is flagged as the docs being behind — which is material for [audit-docs](https://aihero.dev/skills-audit-docs).

## It's working if

- Each delivered artifact comes with a claim-to-source map — every factual claim points at a doc and section you can open.
- The maturity line is visible in the artifact itself, phrased for the audience — nothing claims "battle-tested" that hasn't shipped.
- Strengths read as mechanisms and numbers; you can't find an adjective that isn't cashed in its own sentence.
- The artifact is short and most of the corpus visibly didn't make it — depth lives behind links, not in paragraphs.
- Your existing files survived until you accepted their replacements.

## Where it fits

A reach-for-it-anytime standalone at the end of the docs pipeline: [audit-docs](https://aihero.dev/skills-audit-docs) makes the corpus agree with itself and [assess-project](https://aihero.dev/skills-assess-project) verifies what's real, so `distill-docs` can write outward claims on solid ground — run it after them when the stakes are high, alone when you just need the README. When you're unsure which entry point fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
