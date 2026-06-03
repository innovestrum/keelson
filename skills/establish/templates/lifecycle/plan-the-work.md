---
name: plan-the-work
description: >-
  Activate immediately after the {{issue}} exists and is claimed — handed from
  frame-the-change (it just created one) or pick-next-work (it just claimed one) —
  before any code. Plan in the open, then chain into implement-the-change, or stop to ask
  if a strategic call surfaced.
---

Plan on the {{issue}}, in the open, before code. The plan is what the user reviews to
redirect cheaply; a wrong turn caught here costs a comment, caught in the diff costs a
rewrite.

1. **Re-read the {{issue}} body and every doc it references.** The plan answers the
   acceptance criteria. If the body and a doc in `{{docs_root}}` disagree, resolve it via
   respond-to-uncertainty first.
2. **Post a 3–7 bullet plan as a comment on the {{issue}}.** Each bullet is a concrete
   move (area + what changes), not a restatement of the goal. Fewer than 3 → the
   {{issue}} was over-scoped for one change; more than 7 → it should split.
3. **Non-trivial call → ADR.** New dependency, a deviation from `{{source_of_truth}}`, a
   cross-cutting structural choice → author one in `{{adr_dir}}` marked *Proposed* and
   link it from the plan. The ADR is a file write: open the work's `{{isolation}}`
   **first** and author it there, or it strands outside the {{change}}.
4. **Scope past `{{size_cap}}` → split into linked {{issues}} before coding**, not
   mid-implementation. The cap gates *net new hand-written* lines — deletion-heavy
   refactors pass and generated/vendored paths are exempt, so estimate that, not the raw
   diff. Reach for `{{oversize_label}}` only when the change is genuinely one cohesive
   deliverable, never to dodge a split that wants to be separate {{issues}}.
5. **Set `{{status_field}}` → {{status.planning}}.**
6. **Hand off — the crux.** Mechanical vs. strategic:
   - **Mechanical** (execution within a settled design) → chain into
     implement-the-change this same turn. Don't wait for an explicit "go".
   - **Strategic** (any bullet carries an open question, a new dependency, an ADR still
     *Proposed*, a deviation from `{{source_of_truth}}`, a scope split, or an unresolved
     ambiguity from respond-to-uncertainty) → **stop and ask in chat, quoting the
     bullet.** Self-heal authority covers execution, never the design call.
   - Unsure which → treat as strategic and ask. The human loop is cheap; an unwound wrong
     direction is not.

*(How to comment on the {{issue}}, open the `{{isolation}}`, and set status is in
`AGENTS.md` → Tracker notes — name the action here, run the tracker's own commands
there.)*
