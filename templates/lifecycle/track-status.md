---
name: track-status
description: >-
  Activate at each lifecycle moment that moves an {{issue}}'s state, and after any unit
  of work finishes — including admin/infra ops that bypass a {{change}} (CLI calls,
  console clicks). Sets the {{status_field}} and, at close, ticks acceptance with
  evidence. Also owns the merge + recap once a {{change}} is green and gated.
  plan-the-work and open-the-change call this; it never starts run-the-milestone.
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

## Merge and recap
When the {{change}} is green and its gates have settled:
- **Merge yourself, unless it needs an ack first** — then ask (batched in the ledger) and
  merge on the answer: the hand-back lists ack items (an artifact to judge — pass its link
  and the region in words, never open the image yourself — or the body's open questions), an
  ADR is *Proposed*, the diff touches `{{frozen_paths}}`, or the user asked to review it.
- **You are the doc-update reviewer** (`AGENTS.md` §5): map the diff's areas to the docs
  they should touch, compare with the hand-back's docs list; a gap goes back to the
  implementer before merge, not into a follow-up.
- **Merge per `{{merge_style}}` as its own step.** Clean up, comment or close only after the
  tracker reports the {{change}} merged — a cleanup chained after a rejected merge removes
  the {{isolation}} and posts a false "shipped". Then remove the {{isolation}}.
- **Watch `{{default_branch}}` once per merge batch** ("up to <sha>"), plus any downstream
  workflow it triggered — not once per {{change}}; same after a direct push.
- The auto-close keyword should flip the {{issue}} — verify its state, else set it.
- Merged work CI can't verify (a device, a manual check) → its box stays open with
  `{{needs_user_label}}`.

End with a **chat recap** (not in the {{change}} body) — the user is deciding whether to
dig in, so make it a scannable index, not a narrative:
- The {{change}} URL on its own line.
- One sentence on *why* the change exists.
- Bulleted moving pieces, each anchored to a file/component path, one tight line each.
- Follow-ups, deferred scope, or merge risks — only if any.

Markdown bullets + `backticked paths`, no headings/tables, ~150 words. A delegated
loop-runner's `DONE` report is the raw material; the recap is yours.

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
Once the {{issue}} is set/closed and monitoring is done, the recap (or one line, with no
{{change}}) is the hand-back. Inside a live run-the-milestone run, return to its hand-back
steps; outside one, stop — the next cycle starts when the user asks.

*(Which columns exist, how to set them, and how to close an {{issue}} with its acceptance
is in `AGENTS.md` → Tracker notes — name the move here, run the tracker's own commands
there.)*
