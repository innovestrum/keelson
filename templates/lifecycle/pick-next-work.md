---
name: pick-next-work
description: >-
  Activate at session start with no assigned task and no in-flight {{change}}, or
  right after track-status closes a cycle and the tree is clean on
  `{{default_branch}}`. Distinct from frame-the-change (forms a new {{issue}} from a
  user request); chains into plan-the-work.
---

No assigned work. Claim the next {{issue}} from the backlog — don't invent one, and
don't start from a dirty tree.

1. **Precondition: clean tree on `{{default_branch}}`.** Dirty tree → ask the user
   (commit / stash / discard); wrong branch → switch and fast-forward. Picking off a
   half-done tree strands the previous work.
2. **Pick by the backlog rule** in `AGENTS.md` — the repo's documented ordering over
   open, unassigned {{issues}} (priority / sequence / parent-then-child). Honor it;
   don't cherry-pick a more interesting item out of order.
3. **Read the candidate before claiming it.** The body may name prerequisites that
   don't exist yet — an upstream {{issue}} not yet landed, a gate not yet built. If
   it's not actually ready, defer it and take the next; claiming a blocked {{issue}}
   just parks it under your name.
4. **Claim it** — assign yourself and set `{{status_field}}` → **{{status.in_progress}}**.
5. **Chain into plan-the-work in the same turn** — picking is mechanical, no ack
   needed. Stop only if a precondition above fails.

Backlog empty, or every candidate taken / blocked → **ask the user**. Don't fabricate
work or pull something out of order to have something to do.

*(Reading open/unassigned state, the ordering, assigning, and setting status is in
`AGENTS.md` → Tracker notes — name the action here, run the tracker's own commands
there. Read backlog/readiness from the {{tracker}}'s structured state, not by parsing
{{issue}} prose; if "ready" depends on anything beyond plain state, that rule lives
there too.)*
