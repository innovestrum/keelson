---
name: implement-the-change
description: >-
  Activate after plan-the-work posts the plan. Proceed immediately when the plan is
  mechanical; wait for user ack only if plan-the-work flagged a strategic call (open
  question, new dependency, ADR, scope split, ambiguity). Chains to open-the-change once
  the work is green.
---

Plan is posted. Write the {{change}} in {{isolation}}, scoped to exactly what the
{{issue}} asked for. The rules below are what an agent gets wrong without being told.

1. **Work in {{isolation}}.** Before the first edit, enter it — creating it per
   `{{branch_pattern}}` if plan-the-work didn't. Every write (code, docs, ADRs) for this
   {{issue}} lands there; the primary checkout stays clean on `{{default_branch}}`.
2. **Code → doc → tests, same {{change}}.** A {{change}} that ships behaviour without the
   doc and test that prove it is incomplete — don't defer either to a follow-up.
3. **Touch only files the {{issue}} needs.** A drive-by refactor or unrelated fix you spot
   in passing is a separate {{issue}} → carve-out-followup; don't grow this {{change}}.
4. **Never hardcode a value to make something work.** Tempted to inline a magic number,
   path, threshold, or credential — stop, surface it in config, and if the *right* value
   is unclear, ask on the {{issue}} (respond-to-uncertainty) before committing.
5. **Red CI mid-iteration → reproduce locally before re-pushing.** When the failure isn't
   a one-line typo (a flake, a timeout, an async/scheduler-ordering or environment
   failure), run the failing {{quality_gates}} locally and fix from the real signal —
   push-and-watch loops burn minutes per round and often chase the wrong cause. This is
   the local-verify carve-out to the CI-owns-the-gates rule that open-the-change states.

When the {{change}} is mergeable and clean, chain into open-the-change — don't stop
between mechanical steps; escalate only on a strategic call.

*(Branch / {{change}} / commit conventions are in `AGENTS.md` → Tracker notes — name the
action here, run the tracker's own commands there.)*
