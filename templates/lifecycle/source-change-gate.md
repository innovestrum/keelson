---
name: source-change-gate
description: >-
  Activate after open-the-change, once CI is green, when the {{change}} edits the canonical
  {{source_of_truth}} itself (a token, a schema/spec, an i18n catalogue) — not an
  implementation of it. Realizes the changed source into its viewable/derived form,
  confirms the intended change with the author, self-heals realization-breaking mechanics,
  and escalates collateral drift to OTHER elements. No-op when the diff edits no source.
---

The other gates watch implementations; this is the **source side**. When the {{change}}
edits `{{source_of_truth}}` itself, this realizes the changed source so the edit is **seen
before it propagates** — and catches the ripple a CI pass can't: a shared primitive (a
token, a base type, a shared key) that quietly shifts *unrelated* elements. CI green proves
the source parses, not that only the intended thing moved.

This gate does **not** approve taste — the source change is the author's call. Its job is to
prove the source realizes as intended and **nothing else moved**.

## When to skip
- The diff edits no `{{source_of_truth}}` element (pure logic / refactor / docs, or an
  *implementation* of the source — that's source-sync-gate). No-op.
- You can't realize the source (build runs elsewhere, the toolchain/network failed) —
  surface that and stop. Never claim "realized" on a failed run.

## Run
1. **Scope.** From the diff, which `{{source_of_truth}}` element(s) changed? Realize **those**
   into their viewable/derived form. A change to a **shared primitive fans out** — realize
   every dependent element, not just the edited file; the unintended drift hides there.
2. **Read the realized output** against the change's intent and the brief. Read the evidence
   first, then summarize findings.
3. **Triage each finding:**
   - **Confirm with the author** — the *intended* new/changed element itself (a new component,
     a restyled token, an added schema field). Surface it; ask it matches intent. Not a defect
     to fix — an ack to get.
   - **Self-heal** — realization-breaking mechanics the source already dictates: a reference
     pointing at a stray local copy instead of the one canonical home, a path that 404s after a
     rename, a structural slip (unclosed markup, a dropped link) that breaks the realized view.
     Fix on the same branch, re-realize, re-read. Note what you healed.
   - **Escalate — collateral drift** (the high-value catch): the fan-out shows an *unrelated*
     element changed, or the edit contradicts the brief / warrants an `{{adr_dir}}` entry. Stop,
     quote the drifted element + the intended edit, ask. No merge until acked.
   - Unsure which bucket → **escalate** (respond-to-uncertainty).
4. **Coverage gap.** The diff adds a source element nothing realizes (no view / generator path
   for it) → note it; extend the realization path in this {{change}} or carve-out-followup.

## Hand back
- No drift → one-line note ("N source elements realized, only the intended change moved") and
  continue the chain. Self-heal applied → re-realize after the fix, then note it. Confirm-only →
  hold for the author's ack. Collateral drift → blocks merge until the user redirects.

Self-heal authority is realization mechanics only — **never make the design/spec decision** the
edit embodies; that's the author's, and propagating it to implementations is downstream
(source-sync-gate, then the {{targets}}).

*Boundary:* the **source itself** changed — this gate. Canonical-source ↔ one implementation is
`source-sync-gate`; spec → one implementation against acceptance is `review-gate`; {{target}} ↔
{{target}} equivalence is `parity-gate`. Bringing a new canonical element *in* is
`update-source-of-truth`, not this audit.

*(How to realize the source, and where its derived output lands, is in `AGENTS.md` → source
conventions — name the action here, run the repo's own commands there.)*
