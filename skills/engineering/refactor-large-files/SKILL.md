---
name: refactor-large-files
description: Find files over a line threshold and propose a split into modules or subpackages that follows the repo's own conventions and a real seam. Takes the threshold as an argument, e.g. 1000.
disable-model-invocation: true
---

Find the oversized files an agent-built codebase accumulates, and propose the split each one actually wants.

The threshold is the argument (`/refactor-large-files 1000` = files over 1000 lines of code). With no argument, derive one from the repo's own distribution — roughly three times the median file, rounded to something legible — and state the number you picked and why before scanning.

**Line count finds candidates; it never designs the split.** A file is cut at a **seam**, never at a line budget: splitting a 1200-line file into three 400-line files that each reach into the others' internals produces three shallow modules and a tangle of imports, which is worse than the file you started with. A file with no seam stays one file, and the report says so.

Call the Skill tool with "codebase-design" for the vocabulary this skill speaks — **module**, **interface**, **depth**, **seam**, **leverage**, **locality**, and the **deletion test**. Use those terms exactly.

## 1. Measure

List candidates over the threshold across the tracked tree (`git ls-files`, so ignored files never appear). Prefer `tokei` or `scc` if installed — they separate code from blanks and comments; otherwise `wc -l` with the caveat stated in the report.

Exclude by nature, not by size: generated output (protobuf, codegen, migrations, `*.pb.*`, `*_generated.*`), vendored and bundled code, minified assets, lockfiles, snapshots and fixtures. Say what you excluded.

Report the distribution — median, p90, largest — so the threshold has context. A repo whose median file is 800 lines has a convention problem, not a file problem, and that is the finding.

## 2. Establish conventions

The split has to look like it belongs in this repo, so derive the house style before proposing anything, from the repo's own well-organized multi-file modules:

- **Grouping** — by feature, by layer, by type? Look at existing directories that hold several files and name the axis they're grouped on.
- **Naming** — file and directory casing, suffix idioms (`*.service.ts`, `handlers.go`, `_impl.rs`).
- **Re-export style** — barrel files, `index` modules, `mod.rs`, `__init__.py`: does the repo present a directory as one importable unit, or import deep paths directly?
- **Test layout** — beside the source, or in a mirrored tree? A split multiplies test files the same way.

Where the repo is silent, fall back to the language's module unit — and where the repo has a convention, **the repo overrides**:

- **TypeScript / JavaScript** — file is the module; a directory plus a barrel `index` presents several files as one import path.
- **Python** — module is the file; a package is a directory with `__init__.py` re-exporting the public names.
- **Go** — package is the directory, so splitting a file *within* its package costs no import changes anywhere. The cheapest split in any language; take it freely.
- **Rust** — a `mod` grows from `name.rs` into `name/` with `mod.rs` (or `name.rs` plus a directory), `pub use` restating the public surface.
- **Java / C#** — one top-level type per file, so the split is type extraction and the file layout follows the namespace.
- **Ruby** — one class or module per file, directories mirroring the namespace for autoload.
- **C / C++** — header and implementation pair; the split decides what moves into the public header.

Write the profile down in the report. Every proposal is checked against it.

## 3. Triage

Large is not automatically wrong. Sort candidates into:

- **Justified** — a closed exhaustive match or dispatch table, a data or constant table, a single genuinely cohesive deep module whose interface is already small. Name it, say why it stays, move on.
- **Offender** — carrying several unrelated responsibilities, or a small interface's worth of public surface buried in a pile of unrelated private helpers.

The strongest evidence for an offender is **Divergent Change**: the file gets edited for several unrelated reasons. Git shows it — `git log --oneline -- <file>` over a good stretch, read for distinct reasons rather than count. Several unrelated reasons means several responsibilities, and responsibilities are where the seams are.

Rank offenders by the pain they actually cause, not by size: churn (how often it changes), fan-in (how many files import it), and how far past the repo's median it sits. A 3000-line file untouched in a year outranks nothing.

## 4. Diagnose

Fan out sub-agent readers, one per offender, each writing to `.scratch/refactor-large-files/<file-slug>.md`:

- **Public surface** — what the outside world imports from this file, and which callers use each export. Often a handful of the exports carry all the callers; the rest are internal and should never have been visible.
- **Cohesion clusters** — groups of functions and types that touch the same state and don't reference the rest of the file. These are the seam candidates.
- **Responsibilities** — the distinct reasons this file exists, in the repo's domain language (`CONTEXT.md` where it exists).
- **Coupling to the rest of the file** — what each cluster would still need if it moved out. This is what kills bad splits, so it must be measured before proposing, not after.
- **Test coverage** — which tests exercise this file, and through what interface.

## 5. Propose

Per offender, a split plan. Each proposed module must stand on its own:

- **Name** — in the repo's domain language, following the naming convention from step 2.
- **What moves** — the functions and types, by name.
- **Interface** — what it exports after the move, which must be *narrower* than the surface it hides. State it explicitly.
- **The deletion test** — deleting this module would concentrate complexity, not just relocate it.
- **What stays** — the file that remains, and its interface after the cut. The remainder is a module too, and it gets the same scrutiny.

Two rules bind every plan:

- **No seam, no split.** If the clusters all need each other's internals, report the file as large-but-cohesive and name what would have to change first to make a seam possible. A file left whole with a reason is a real result.
- **Splitting must not cause Shotgun Surgery.** Walk the file's most recent real changes against the proposed layout: if a typical change now touches four files instead of one, the cut is in the wrong place. Say which change you walked.

Each plan carries its risk: the callers that must be updated, whether tests cover the moved code (and if not, that characterization tests come first), and whether the move is mechanical or requires editing logic.

## 6. Report, and get the plan approved

Write `REFACTOR_PLAN.md` in the working directory: the distribution and threshold, the convention profile, the justified list, then the offenders ranked, each with its split plan. Show the before and after layout as a small file tree per offender. Change no code.

Then put the plan up for approval as a **goal**, not a menu — the whole ranked batch, or whichever subset the user selects. This is the decision point: judgment belongs on *which splits are right*, made once against the full plan, while the tests and the commit history are what keep each move safe. Where the harness has a plan-approval mode, this is what it is for.

## 7. Execute the approved batch

Work the approved files in rank order, unattended, until the batch is done. Refactoring is behavior-preserving, and the test suite is the gate on every file:

- Run the repo's tests first and record green. No coverage over the moved code means characterization tests come first — otherwise nothing distinguishes a refactor from a rewrite.
- Move code without editing it, and keep pure moves in their own commit, separate from any commit that changes logic. A reviewer must be able to see that nothing changed.
- Update callers, run the tests again, and confirm green against the recording.
- Close out each file by calling the Skill tool with "code-review" against the commit that file started from.

Stop and come back to the user on a **surprise** — tests that will not go green, a move that turns out to need logic edits, a split that opens a real design question, or a file whose diagnosis no longer matches what is on disk. A surprise is worth interrupting for; finishing a planned file is not. Report what landed and what stopped when the batch ends.

## Done

- Threshold stated (given or derived), distribution reported, exclusions named.
- The convention profile is written down and every proposal obeys it.
- Every candidate is triaged justified or offender, with Divergent Change evidence from git behind each offender.
- Every proposed module names its interface and passes the deletion test; every plan names the change it walked for Shotgun Surgery.
- Files with no seam are reported as such rather than shredded to hit the number.
- `REFACTOR_PLAN.md` written and no code changed until the plan is approved — and every executed split landed with tests green before and after, moves separated from edits.
- The approved batch ran to completion, or stopped on a named surprise and said so.
