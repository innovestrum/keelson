---
name: tune-gates
description: >-
  Establish or refine a repo's quality gates **independently of the full adopt-keelson
  interview** — both the CI quality-gate policy (which checks block merge) and the
  verification gates (review, source-change, source-sync, parity). Runs in ≈2–4 rounds,
  is re-runnable to add/tune/drop a gate on a repo that already adopted keelson, and is
  the canonical gate spec adopt-keelson defers to. Trigger on "set up the quality gates",
  "add a gate", "tune the gates", "refine the verification gates".
---

# Tune gates

Two gate families ride keelson's loop; this skill owns both, end to end, **without the
8–20 round adopt-keelson interview** — so a repo can set its gates up first, or come back
later to add one, tighten the checks, or drop a gate that no longer fits.

1. **CI quality gates** — the green checks that block merge (lint, test, type, coverage…)
   → `{{quality_gates}}`, plus the *policy* (CI owns them: don't pre-run locally before
   opening; local-verify only during a red-CI debug loop). The checks-string lives in
   `AGENTS.md §3`; the policy lives in `open-the-change`.
2. **Verification gates** — the post-CI, agent-run reviews, each a selectable skill:
   - **review-gate** — spec → one implementation; the {{change}} produced a reviewable
     artifact (rendered UI, generated output, a schema, a report).
   - **source-change-gate** — the {{change}} edits the `{{source_of_truth}}` *itself*;
     realize it, confirm intent, catch collateral drift to other elements.
   - **source-sync-gate** — `{{source_of_truth}}` ↔ one implementation drift.
   - **parity-gate** — {{target}} ↔ {{target}} equivalence, at {{milestone}} cadence.

Templates live at `templates/lifecycle/<gate>.md`; the gate substrate is
`templates/AGENTS.client.md §8` (+ the `§3` checks line). Fill, don't invent.

## 1 · Orient & pick the mode
- **Read the repo's `AGENTS.md` if it has one.** Pull vocabulary (`{{change}}`, `{{issue}}`),
  `§3` checks, `§7` `{{milestone}}`, `§8` gates already adopted, `{{adr_dir}}`. This is
  **refine mode** — show the user what's gated now and what's missing; change only the delta.
- **No keelson `AGENTS.md`** → **establish mode**. The gates need a substrate to defer to:
  offer adopt-keelson for the full model, or — if they only want gates — capture the minimum
  (`§3` checks + `§8` rows + the source/target vocabulary the gates read) so the written gate
  skills aren't dangling.
- **Invoked by adopt-keelson** → it has already gathered vocabulary, `{{source_of_truth}}`,
  `{{targets}}`, `{{milestone}}`; skip orientation and run G1–G2 as that interview's gate
  pillar, returning the adopted set for its unified write.

## 2 · Read the repo shape (drives which gates fit)
Before proposing, learn — don't prescribe:
- **Canonical source?** Design tokens, a schema/spec, an i18n catalogue with a single home →
  `{{source_of_truth}}`. Present → the **source-change + source-sync pair** is in play.
- **How many targets?** Equivalent surfaces shipped across ≥2 `{{targets}}` (apps, platforms,
  services) → parity is in play; one target → it isn't.
- **Reviewable artifacts?** Do changes routinely produce something a human eyeballs (UI,
  generated output) → review-gate is in play.
- **CI checks today.** What blocks merge now (read the CI config / branch protection) — the
  starting point for `{{quality_gates}}`.

## 3 · The rounds
- **G1 · CI quality gates.** Which checks must be green before merge → `{{quality_gates}}`.
  Confirm the policy: CI owns the gates (don't pre-run them locally before opening); a
  local-verify carve-out is expected while debugging a red CI run whose mechanism is unclear.
- **G2 · Verification gates.** Walk the four; **adopt or skip each with the user** per §2:
  - review-gate ← changes produce reviewable artifacts.
  - source-change-gate **and** source-sync-gate ← a `{{source_of_truth}}` exists (adopt as a
    **pair** — preview the source change *and* audit source↔implementation drift; one without
    the other half-covers the source loop — flag it if they pick only one).
  - parity-gate ← ≥2 `{{targets}}`; confirm the cadence ({{milestone}} close, or on-demand
    after a feature lands on both).
  Resolve `{{source_of_truth}}`, `{{target}}`/`{{targets}}` for every adopted gate.
- **G3 · Confirm.** Replay: the checks string, each gate adopted vs skipped, and any gap left
  open. One ack, then write.

## 4 · Fill & write
1. **`AGENTS.md §3`** — set *Required green checks before merge:* `{{quality_gates}}`.
2. **`AGENTS.md §8`** — keep one row per **adopted** gate (render from
   `templates/AGENTS.client.md §8`). **Refine mode: edit `§8` in place** — add/remove rows
   only; never rewrite or reorder the other sections.
3. **Gate skills** — for each adopted gate, render `templates/lifecycle/<gate>.md`
   (placeholders + frontmatter) to each client skill dir as `<dir>/<gate>/SKILL.md`.
   Dropping a gate in refine mode → delete its `§8` row **and** its skill file.
4. **Policy home** — if `open-the-change` is adopted, ensure its *CI owns the gates* section
   carries the don't-pre-run + local-verify-on-red rule; if the repo skipped that skill, fold
   the rule into `§3`.

## 5 · Verify & hand back
- Grep the written/edited files for `{{` — **zero** leftover placeholders.
- Every adopted gate has **both** a `§8` row and a written skill; every skipped/dropped gate
  has **neither**. No gate references a null binding (see Gotchas).
- Recap: the checks string, gates adopted vs skipped, files written/edited/deleted, and any
  gap left open. If standalone-refine, point the user back at `AGENTS.md §3/§8`.

## Gotchas
- **Never write a gate whose binding is empty.** No source-change/source-sync-gate when
  `{{source_of_truth}}` is *none*; no parity-gate for a single `{{target}}`. A gate that
  defers to a blank is worse than no gate.
- **Refine mode is surgical.** Read the existing `AGENTS.md`, patch `§3`/`§8` and the gate
  skill files only. Clobbering a hand-tuned non-gate section is the failure that makes people
  distrust re-running this.
- **The source pair travels together.** source-change-gate (the source itself moved) and
  source-sync-gate (source ↔ implementation) are two arms of one loop — adopting only one
  leaves a blind spot; say so.
- **Stacked judgment gates aren't independent.** review/source-change/source-sync are model
  judgments — two on the same model + context share a blind spot, so drift that slips the
  implementer can slip the review too. They don't multiply like independent checks; the
  strategic-loop escalation (a move touches design → stop, ask) is the non-judgment backstop
  for the misses they'd both wave through.
- **Don't transcribe CI or tracker commands into the gate skills.** The gates *name* actions;
  how to realize the source, fetch a CI artifact, or open a follow-up live once in
  `AGENTS.md` (→ source conventions / Tracker notes). About to write `gh`/a build command into
  a gate? Push it there.
- **Gates are thin and defer to the standards.** Don't restate code-quality rules inside a
  gate; hold every line to the golden rule (`AGENTS.md`): gaps, gotchas, pointers only.
