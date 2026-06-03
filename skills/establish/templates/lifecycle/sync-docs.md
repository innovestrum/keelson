---
name: sync-docs
description: >-
  Activate at {{milestone}} close (from close-a-milestone) or after a large
  multi-{{change}} story lands — when several merged changes + ADRs may have left the
  prose docs in {{docs_root}} contradicting each other or the shipped reality.
  Cross-document logical-consistency audit of {{docs_root}} + {{adr_dir}} + runbooks:
  finds contradictions, stale stage/tense claims, and superseded-decision references,
  reconciled in one docs {{change}}. NOT a spec↔implementation gate (that's review-gate
  / source-sync-gate / parity-gate) and NOT a single-file proofread.
---

The textual sibling of the gates. It catches what per-{{change}} review can't: drift
that only appears **across** docs after a batch of work — an ADR supersedes a decision
but three prose docs still cite the old one; a feature moves {{milestone}} but the
roadmap, index, and catalogue disagree; an exit criterion gets descoped on the tracking
{{issue}} but the doc set never hears about it. Each {{change}} looked fine alone; the
*set* is now inconsistent.

## When to run
- **{{milestone}} close** — close-a-milestone hands off here, after exit-criteria
  verification.
- **End of a big story** — a multi-{{change}} feature touched ADRs, contracts, or
  cross-cutting behaviour.
- **Skip** a single small {{change}} (its own review owns that) and docs-only typo passes.

## How to run

1. **Establish ground truth first.** List what actually shipped: closed {{issues}},
   merged ADRs (note which *supersede* earlier ones), descoped/reclassified exit
   criteria, new bridging {{milestone}}s. The audit judges the docs *against this* — a
   wrong baseline yields wrong findings. Pull it from the {{milestone}}'s close-out
   record + the ADR log, not memory.

2. **Fan out, one cluster per theme** (independent → parallel, read-only). Audit by
   *theme*, not file-by-file, so one reviewer sees every side of a contradiction at once:
   - **Roadmap / status / tense** — index, vision, roadmap, architecture overview: stale
     "not started / will / future", missing new {{milestone}}s, stage-count framing.
   - **Contract lock-step** — every representation of a shared schema must agree
     field-for-field (canonical definition ↔ its prose spec ↔ generated artifacts). The
     canonical source usually names its lock-step partners; follow them.
   - **One per cross-cutting decision** (auth, deployment, consent, wire format, …) — the
     deciding ADR vs every prose doc + runbook that mentions it.
   Give each reviewer the ground truth; demand **file:line + one-line fix per finding**,
   severity-tagged (contradiction | stale | minor), and an explicit "clean" for sub-areas
   with no drift.

3. **Verify before trusting.** Parallel reviewers misattribute line numbers — re-grep
   each high-severity finding against the live file before editing.

4. **Reconcile in one {{change}}.** Land every fix in a single docs-sync {{change}} so
   the doc set is internally consistent at one commit. Likely trips `{{size_cap}}` →
   expect `{{oversize_label}}`.

## Gotchas
- **Distinguish doc↔doc from {{issue}}↔doc.** An {{issue}} that disagreed with the docs
  (an exit criterion that never matched the roadmap) is not a doc bug — note it, don't
  "fix" the docs to match a stale {{issue}}.
- **A historical decision record is not drift.** A superseded ADR still describing its
  original (now-reversed) decision *inside its own body*, behind a status / "superseded
  by" banner, is correct. Only flag the superseded decision where a **non-ADR** doc
  presents it as current.
- **Projections aren't the canonical contract.** A read-view or storage schema that
  renames/omits fields vs `{{source_of_truth}}` is a deliberate projection, not lock-step
  drift — check intent before flagging.
- **Cross-{{target}} idiom differences** are equivalence, not contradiction — parity-gate
  owns that across `{{targets}}`, not this skill.

*(Finding the closed {{issues}}, the {{milestone}} close-out record, and the ADR log is
in `AGENTS.md` → Tracker notes — name the action here, run the tracker's own commands
there.)*
