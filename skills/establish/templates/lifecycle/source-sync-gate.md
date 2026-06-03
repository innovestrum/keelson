---
name: source-sync-gate
description: >-
  Activate after open-the-change, once CI is green, when the {{change}} touches an
  implementation of a canonical {{source_of_truth}} (design tokens, a schema/spec, an
  i18n source). Pairs that implementation against a fresh view of the source and resolves
  drift: self-heals small slips, escalates structural mismatch — never edits the source.
  No-op when the diff touches no such implementation. Differs from review-gate (spec →
  implementation against acceptance) and parity-gate ({{target}} ↔ {{target}}).
---

The {{source_of_truth}} is canonical; the diff is one implementation of it. CI green
proves the implementation runs, not that it still **agrees** with the source. This gate
puts a fresh view of the source next to what the {{change}} produced and judges them as a
pair, before merge.

Comparison is **structural / semantic equivalence, not a byte or pixel diff** — a token
table and a generated stylesheet, a spec and a serializer, a catalogue and its bound
strings never match literally, so a raw diff is all-noise. Judge: same fields / keys /
states present and named, same shapes and constraints, same meaning.

## When to skip
- The diff touches no implementation of a {{source_of_truth}} (pure logic, refactor,
  docs). No-op.
- review-gate already escalated and the {{change}} is paused on a spec decision — let it
  settle; the implementation isn't final yet.
- You can't get a current view of the source (it builds elsewhere, the fetch failed) —
  surface that and stop. Never claim "in sync" on absent evidence.

## Run
1. **Scope.** From the diff, which implementation(s) of which {{source_of_truth}} element
   moved? Take a **fresh** view of *those* source elements from the working tree — what
   the implementation is meant to match, not a stale cache. Don't broaden later.
2. **Pair and read.** Put each in-scope source element beside its implementation: every
   source field / key / state present and correctly named? Shapes, constraints,
   thresholds carried through? Nothing in the implementation the source doesn't define?
3. **Triage each finding:**
   - **Self-heal** — a slip the source already dictates: a literal where the source names
     a value, a hard-coded copy of a source entry that should reference it, a dropped
     constraint, a missing key the source lists. Fix **the implementation** on the same
     branch, push, let CI rebuild, re-pair. Note what you healed.
   - **Escalate** — structural mismatch: a field / state / shape in the source absent from
     the implementation or vice-versa, a renamed or relocated element, anything that would
     warrant changing the {{source_of_truth}} or an {{adr_dir}} entry. Stop, quote both
     sides + the divergence, ask. No merge until acked.
   - The **source** itself looks wrong → an update-source-of-truth matter, not a fix you
     make here. Escalate.
   - Unsure which bucket → **escalate** (respond-to-uncertainty).
4. **Coverage gap.** The diff adds an implementation for a source element nothing pairs
   against → note it; extend coverage in this {{change}} or carve-out-followup. Don't
   fabricate a pairing.

## Hand back
- No findings → one-line note ("N elements paired, in sync") and continue the chain.
  Self-heal applied → re-pair after CI, then note the fix. Escalation → blocks merge until
  the user redirects.

Self-heal authority is implementation nudges only — **never edit the {{source_of_truth}}
from this gate**; source changes go through update-source-of-truth.

*Boundary:* canonical-source ↔ implementation, one {{change}}. Spec → implementation
against acceptance is `review-gate`; {{target}} ↔ {{target}} equivalence is `parity-gate`.

*(How to take a current view of the source, and where its build output lands, is in
`AGENTS.md` → source conventions — name the action here, run the repo's own commands
there.)*
