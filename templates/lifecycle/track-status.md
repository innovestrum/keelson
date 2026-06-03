---
name: track-status
description: >-
  Activate at each lifecycle moment that moves an {{issue}}'s state, and after any unit
  of work finishes — including admin/infra ops that bypass a {{change}} (CLI calls,
  console clicks). Sets the {{status_field}} and, at close, ticks acceptance with
  evidence. plan-the-work and open-the-change call this; pick-next-work does not chain
  from it.
---

The {{board}}'s {{status_field}} is the source of truth for where an {{issue}} stands —
not labels, not the {{change}}'s open/merged state. Two jobs: **move** the column at
each lifecycle moment, and **close out** with evidence when the work is actually done.

## Move the column
Set `{{status_field}}` at the moment the state changes, not in a batch afterwards:
- plan posted → **{{status.planning}}**
- work started / {{change}} opened → **{{status.in_progress}}** (for *every* {{issue}}
  the {{change}} links, not just the first)
- {{change}} in review → **{{status.in_review}}**
- waiting on a paywall, a third party, or a manual step the user owns →
  **{{status.blocked}}**
- shipped → **{{status.done}}**; not now → **{{status.deferred}}**

Omit any column this repo doesn't use. If no column matches the lifecycle moment, add
one on the {{board}} rather than forcing the closest wrong one.

## Close out — only with evidence
The {{change}}-merge moment is the obvious trigger. The one that gets *missed*: admin or
infra work that never touched a {{change}} (CLI mutation, console action, manual
rotation). Run this explicitly after those — nothing else will.

1. **Re-read the acceptance criteria** on the {{issue}}. CI green or a merged {{change}}
   is not the spec; the boxes are.
2. **Tick a box only with cited evidence** — the {{change}}, a commit, a comment, an
   external artifact. No evidence → leave it unticked.
3. **All boxes ticked** → close as *completed* with a one-line summary of what shipped.
4. **Some ticked** → leave open, comment done-vs-left, set **{{status.blocked}}** if one
   of the gates above is what's holding it.
5. **Deferred / won't-do** → comment the reason; **{{status.deferred}}** (open) or close
   as *not-planned*. **Never silently abandon** an {{issue}} — an untracked drop is the
   failure mode this guards against.

## Stop at the cycle boundary
Once the {{issue}} is set/closed and any monitoring is done, confirm in one line and hand
back. **Do not** chain into pick-next-work — picking the next {{issue}} is a deliberate
act the user initiates, not a default tail.

*(Which columns exist, how to set them, and how to close an {{issue}} with its acceptance
is in `AGENTS.md` → Tracker notes — name the move here, run the tracker's own commands
there.)*
