---
name: run-the-milestone
description: >-
  Activate (orchestrator only) at session start with no assigned task, when the user asks
  for the next {{issue}} or to run the {{milestone}}, and on every loop-runner hand-back
  while a run is live. Keeps implementers busy, triages and re-plans as work lands, stops
  only when nothing is in flight and the rest needs the user.
---

Runs continuously across a {{milestone}} (days, dozens of {{issues}}), not in waves.

**Active {{milestone}}** = the lowest open one in `AGENTS.md` §7 order — "open" is tracker
state, not its exit boxes; buckets (§4) are never active; never skip one; none defined → the
open backlog. Primary checkout clean on `{{default_branch}}`: dirty → escalate, wrong branch
→ switch and fast-forward.

## Sweep
The active {{milestone}}'s open children + every `{{triage_label}}` {{issue}} (via scout when
delegating): criterion served, Value line, open blockers, status, priority, likely paths.
**Field audit** each sweep: an open child missing status or priority gets them
(carve-out-followup values) — unset fields hide it from the queue; a closed one not at
**{{status.done}}** moves there.

**Eligible:** open, no open blocker, not **{{status.deferred}}**, no `{{needs_user_label}}`,
not held by another agent, not overtaken by recent merges, has a Value line (add it when the
body makes it obvious, else a ledger question). A body needing a check or workflow not built yet is blocked
— link the blocker. **Order:** `{{priority.now}}` → `{{priority.next}}` →
`{{priority.later}}` → unset, then the exit criterion served, then oldest.

## Triage
`{{triage_label}}` {{issues}} and each hand-back's `follow-ups` / `blockers found` — after
checking no existing {{issue}} covers it.

| Kind | Goes to |
|---|---|
| polish of this {{milestone}} | active {{milestone}} |
| "later", unsure | `{{backlog_bucket}}` |
| a named later horizon | its bucket |
| needs data or a capability not there yet | a task to get it now; the rest blocked on it, **{{status.deferred}}** |
| scattered small items | one umbrella |

Inside the active {{milestone}}: set parent, blockers, priority, drop `{{triage_label}}` — do,
don't offer. A move out of it or a not-planned close → a ledger question.

## Dispatch (delegate-roles; without it, one {{issue}} at a time inline)
- Up to the parallel cap in `AGENTS.md` §10, in the background, on disjoint files; never two
  in flight on one collision hot spot listed there.
- **One {{issue}} per dispatch.** Resume a runner only with an escalation answer or a
  pre-merge fix; a stacked {{change}}, rebase or follow-up is a fresh dispatch with a short
  brief — long-lived contexts are the cost sink.
- Needs research first → researcher, its open questions into the ledger, then the
  implementer. A product × engineer view → product-quorum.
- Claim the {{issue}} (assign, **{{status.in_progress}}**) — "held by another agent" reads
  this. Name it, the starting skill (plan-the-work, or implement-the-change if the plan is
  acked), its {{isolation}}, a private scratch dir, and the siblings with their files.
- An escalated runner holds no slot; its files stay claimed until it's resumed.

## On every hand-back
1. `DONE` → track-status (merge, or ack first). `ESCALATE` → a ledger item.
2. Rebuild the ledger; triage `follow-ups` + `blockers found`; re-plan blockers and
   priority. Post the ledger or the order only if it changed.
3. Refill the free slot unless draining. Then wait quietly — a watcher notification gets no
   "still waiting" reply.

## Question ledger
Rebuilt from scratch, never from memory: `{{needs_user_label}}` {{issues}} + unresumed
`ESCALATE`s; also whenever the user asks for the questions. One numbered list in
{{question_language}}, answerable as "1a 2b":

`N. <{{issue}}> — what and why it's asked, 1–2 lines · (a) … / (b) … · recommend a`

An artifact to judge gets its link and the region in words; never open the image yourself —
scout describes it. Mirror each item on its {{issue}} with `{{needs_user_label}}`; drop the
label once answered.

## Modes
- **Drain** (e.g. "stop for today", "no new ones"): no refill; finish in-flight, summarize.
- **Resume after a usage limit:** one standard message to each stopped runner — check your
  {{isolation}}, branch, {{change}} and CI state, continue from your step, hand back as usual.
- **Day boundary** (drained): a short handover — ledger, queue, what waits on the user — and
  a fresh session over compaction. Background agents don't outlive the session; drain first.

## Stop
Only when nothing is in flight and every remaining {{issue}} needs the user; all children
closed → close-a-milestone. **End-of-run summary:** merged ({{change}} links), in flight,
waiting on you (the ledger), the queue if it changed.

*(Children, blockers, priority, labels and status are in `AGENTS.md` → Tracker notes — name
the action here, run the tracker's own commands there.)*
