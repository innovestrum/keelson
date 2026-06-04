---
name: parity-gate
description: >-
  Activate at {{milestone}} close (from close-a-milestone) or when a sizeable feature has
  landed on two or more {{targets}}. The only gate that compares {{targets}} to each other —
  pairs equivalent surfaces and checks same capability / states / content, idiomatic
  per-{{target}} differences allowed. Escalate-only; self-heals nothing. No-op for
  single-{{target}} work. review-gate is the per-{{change}} spec→implementation arm.
---

review-gate runs per-{{change}}, one direction (spec → one implementation). This runs at a
coarser cadence (milestone-end / big feature) and asks one narrow question: *do the {{targets}}
expose the same thing?* It runs **after** each side already passed its own review-gate — don't
re-litigate single-{{target}} drift here. `{{source_of_truth}}` is the **arbiter** (which
surface/state *should* exist on each side), not the comparison target.

## When to skip
- **Single-{{target}}** — that's review-gate. Never run parity on a half-landed feature; the
  unshipped side reads as drift.
- **No equivalent surface changed** across {{targets}}.
- **One side's evidence absent** (capture missing / past retention) — surface the gap and stop.
  Never infer parity from one side.

## Run
1. **Pair equivalent surfaces** by capability, not by file. Keep the pairing map current: a new
   top-level surface or state on either side **adds a row**; a missing row is a silent blind spot.
2. **Read each pair together** and judge **equivalence, never byte-identity** — same capabilities
   and ordering, same states, same primary actions and readout content. **Pass idiomatic
   per-{{target}} differences** (framework-native widgets, layout, platform/locale conventions);
   these are *never* findings.
3. When the sides **disagree on whether a surface/state should exist**, consult
   `{{source_of_truth}}` — it says which side is wrong, not just that they differ.

## Taxonomy — classify on the **source**, not the artifact
A surface on one {{target}} and missing on another is one of three things; the artifact can't
tell them apart, so check each {{target}}'s source:
- **Capability gap → escalate.** Exists in `{{source_of_truth}}` and on {{target}} A, but
  {{target}} B has **no source** for it (no module / entry point / route). Real parity drift; the
  fix is a {{target}} feature owned by a new {{issue}}. Quote the paired surfaces (or the missing
  side), the `{{source_of_truth}}` rule it breaks, and the source evidence that this is a
  capability gap (not a capture gap). Recommend the {{issue}} under the closing {{milestone}};
  don't fix inline — self-heal authority here is **none**.
- **Capture gap → don't escalate as drift.** Source exists on both, but one side has **no
  test / capture**, so its evidence omits the surface. Route to extending the capture harness
  (carve-out-followup if out of scope). Don't claim parity *verified* for that surface either.
- **No twin yet → out of scope.** Defined in `{{source_of_truth}}` but on **neither** {{target}}.
  Note it, don't pair.

Unsure which bucket → escalate (respond-to-uncertainty).

## Hand back
- **All pairs equivalent** → one-line note ("N surfaces paired across {{targets}}, capability +
  states equivalent") → continue close-a-milestone.
- **Capture gaps only** → note each + its follow-up, mark those surfaces "evidence absent";
  parity otherwise green, {{milestone}} can close if the user accepts the harness debt.
- **Capability gap** → escalate; **block** the {{milestone}} exit-criteria tick until the user
  acks or opens the {{target}} {{issue}}.

*Boundary:* spec → one implementation is review-gate; `{{source_of_truth}}` ↔ one implementation
is source-sync-gate; {{target}} ↔ {{target}} equivalence is this gate.

*(Creating the {{issue}} under the {{milestone}} — name the action here, run the tracker's own
commands per `AGENTS.md` → Tracker notes.)*
