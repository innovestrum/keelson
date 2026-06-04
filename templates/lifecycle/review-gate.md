---
name: review-gate
description: >-
  Activate after open-the-change, once CI is green, when the {{change}} produced a
  reviewable artifact (rendered UI, generated output, a schema, a report). Reviews the
  artifact against its spec and resolves drift before merge: self-heals small
  deviations, escalates structural ones. No-op for changes that produce no artifact.
---

CI green ≠ correct. When a {{change}} produces an artifact a human would eyeball, this
gate eyeballs it against the spec **before** merge. Single-{{change}}, one direction
(spec → implementation).

## When to skip
- The diff produced no reviewable artifact (pure logic / refactor / docs). No-op.
- The CI run has **no artifact** (harness failed before producing it) — surface the
  run URL and stop. Never claim "verified" on absent evidence.

## Run
1. **Scope.** From the diff, which artifact(s) did this change affect? Pull only those
   from the CI run; don't broaden later.
2. **Read against the spec.** Open each in-scope artifact and judge it against its
   source of truth — `{{source_of_truth}}` if the repo has one, else the {{issue}}'s
   acceptance criteria. Read the evidence first, then summarize findings.
3. **Triage each finding:**
   - **Self-heal** — drift the spec already implies (a value off by a tier, a token
     that should be reused, a missing label/`aria` hook). Fix on the same branch, push,
     let CI reproduce the artifact, re-read it. Note what you healed.
   - **Escalate** — structural deviation: a shape/flow/field the spec doesn't define, a
     renamed or relocated primary action, anything that would warrant an ADR or a
     change to `{{source_of_truth}}`. Stop, quote the artifact + the rule it breaks,
     ask. No merge until acked.
   - Unsure which bucket → **escalate** (respond-to-uncertainty). Self-heal authority
     does not extend to design/spec decisions.
4. **Coverage gap.** The diff adds a new surface but no artifact captures it → the gate
   has nothing to review. Prompt: extend the capture in this {{change}}, or
   carve-out-followup.

## Hand back
- No findings → one-line note ("N artifacts reviewed, no deviations") and continue the
  chain. Self-heal applied → re-read after CI, then note the fix. Escalation → blocks
  merge until the user redirects.

*Boundary:* this is spec → implementation for one {{change}}. Canonical-source ↔
implementation drift is `source-sync-gate`; {{target}} ↔ {{target}} equivalence is
`parity-gate`.
