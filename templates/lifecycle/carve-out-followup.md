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

1. **Parent it.** Trace the gap to its owning {{milestone}}/parent {{issue}} so it lands
   in the right place. Can't place it confidently → leave the parent off and say so in
   the body for a maintainer to reparent. Don't guess.
2. **High-level, not detailed.** Scope statement + a few acceptance bullets + the
   *unresolved decisions* the picker-up should make. A full plan-comment-style body
   locks in design the next person hasn't agreed to.
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

## Ask first when the follow-up forces a strategic call

Vendor swap, stack change, abandoning a documented path — surface it in chat and let the
user direct it; filing an {{issue}} pre-commits one branch of a decision they didn't
drive. Mechanical follow-ups (a deferred test, a missing record, a known follow-on under
the same architecture) → just file.

Status on the new {{issue}} tracks **readiness, not** urgency of discovery: most sit at
the default/backlog column; never **{{status.in_progress}}** for an {{issue}} you aren't
starting.

*(How to create the {{issue}}, link the parent, and set its status is in `AGENTS.md` →
Tracker notes — name the action here, run the tracker's own commands there.)*
