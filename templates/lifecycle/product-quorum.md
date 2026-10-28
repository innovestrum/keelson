---
name: product-quorum
description: >-
  Activate (orchestrator only, delegate-roles adopted) when an {{issue}} needs a product ×
  engineering view before anyone builds — root cause, options, the owner's decisions.
  A product lead and an engineer analyse it independently, cross-examine, rebut, and
  converge on options + a recommendation. Analysis only; not for implementation.
---

One {{issue}}, two perspectives, four rounds. Each view runs in its **own fresh subagent
context** — one agent playing both sides shares one blind spot, which is the thing this
exists to break.

**Inputs** each prompt carries: the {{issue}} (read it with its comments), a one-paragraph
problem, and when you have them the owner's priorities, data pointers and docs to read. Every
prompt also carries the rules: analysis only — no edits to tracked files, commits, {{change}}s
or tracker writes; scratch files only under a named scratch dir; cite `file:line`, data ids
and numbers.

**Roles**
- **Engineer** — reproduce from the data, locate where it enters, rank hypotheses by
  evidence; check every affected `{{target}}`; how often it occurs; at least four realistic
  fixes (minimal guard → principled), each with mechanism, where it lands, cost, risk,
  effort, and what it would *not* fix.
- **Product lead** — what the user experienced and the damage; what "correct" means here;
  product-level directions beyond "fix the math" (suppress, mark, correct after the fact,
  flag confidence); value vs. risk of hiding a real signal, fit with the owner's priorities;
  the acceptance that would convince them; questions for the engineer.

**Rounds** (the two in each round run in parallel)
1. **Independent views** — each answers its brief alone, with an evidence section.
2. **Cross-examination** — each gets its own round-1 output plus the other's. Product
   challenges which fixes leave user-visible damage or over-suppress, ranks them by value,
   asks ≤ 5 sharp questions; the engineer costs each product direction, names its failure
   modes, answers with evidence, says where the framing is unrealistic.
3. **Rebuttal** — each answers the critique of *its* round 1: concede where right, hold
   where the evidence or the value demands it, then state a revised position (engineer: an
   ordered fix set; product: must-have / nice-to-have / won't do).
4. **Consensus** — a third fresh context gets the full exchange and writes the joint
   report: agreed root cause + confidence, frequency per {{target}}, an options table, the
   joint recommendation (first, next, not at all), remaining disagreements one line per side,
   owner decisions as simple questions with a recommended answer, acceptance criteria.
   **Don't invent agreement that isn't in the exchange.**

Cap each output (a few hundred words) so the later rounds stay readable. The owner decisions
go into the question ledger (run-the-milestone); the implementer is dispatched only after
the user picks.

*(On Claude Code a `Workflow` script can run these rounds deterministically; the procedure
above is the contract either way.)*
