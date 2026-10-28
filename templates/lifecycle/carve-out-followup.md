---
name: carve-out-followup
description: >-
  Activate mid-task when scope outside the current {{issue}} surfaces — the {{change}}
  shouldn't grow, but the gap rots if untracked. Files a new {{issue}} instead of
  widening this one. Distinct from frame-the-change (a {{change}} the user asked for
  now) and respond-to-uncertainty (resolve, don't defer).
---

A gap surfaced that isn't this {{issue}}'s job. Default: track it as its own {{issue}}
with a trail back here.

## Three calls before you file

1. **Parent it.** Trace the gap to its owning {{milestone}}/parent {{issue}}. Can't place
   it confidently → parent it to `{{backlog_bucket}}` with `{{triage_label}}`; the
   coordinator places it (run-the-milestone triage). Don't guess, and never leave it
   parentless.
2. **High-level, not detailed.** Scope statement + a **Value** line (what the user or
   product gains — a feature for its own sake is not done) + a few acceptance bullets +
   the *unresolved decisions* the picker-up should make. A plan-comment-style body locks in
   design the next person hasn't agreed to.
3. **Trail back.** End the body with a reference to the {{issue}} open when the gap
   surfaced. {{merge_style}} can erase the branch; the link in the {{issue}} body is what
   survives.

## Don't carve out when

- It fits inside the current {{change}} under {{size_cap}} — just do it here.
- A future {{milestone}}/parent already owns it — find the existing {{issue}} first, file
  only on a real gap.
- It's a drive-by refactor or naming nit nobody will action.

Smaller still — a deferral that fits one line of code or a doc sentence — gets an inline
note at the site, not a fresh {{issue}}.

## Escalate first when the follow-up forces a strategic call

Vendor swap, stack change, abandoning a documented path → escalate and let the user direct
it; filing an {{issue}} pre-commits one branch of a decision they didn't drive. Mechanical
follow-ups (a deferred test, a missing record, a known follow-on under the same
architecture) → just file.

## Do, don't offer

Filing, the parent link, blockers and both {{board}} fields are mechanical — do them, then
list the {{issue}} under `follow-ups` in your hand-back or recap (number + parent, or
`{{triage_label}}`). **Set status and priority in the same step as filing**: a tracker that
auto-adds new {{issues}} to the {{board}} leaves both unset, which hides it from every view
and the queue.
- Status tracks **readiness**: the default/backlog column, or **{{status.deferred}}** when
  parked behind data or a decision; never **{{status.in_progress}}** for one you aren't
  starting.
- Priority follows the triage placement: `{{priority.next}}` in the active {{milestone}}
  (`{{priority.now}}` only for a regression or a blocker of work in flight),
  `{{priority.later}}` in a bucket.

*(How to create the {{issue}}, link the parent, and set its fields is in `AGENTS.md` →
Tracker notes — name the action here, run the tracker's own commands there.)*
