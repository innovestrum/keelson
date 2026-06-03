<!--
  TEMPLATE — the establisher fills {{placeholders}} from the interview, keeps only
  adopted sections, embeds the chosen standards, and folds the chosen tracker's
  section under "## Tracker notes". Delete this comment on write. Hold the result to
  the golden rule at the bottom: gaps, gotchas, pointers — not a tutorial.
-->
# AGENTS.md — rules for AI agents working on this repo

The single authoritative briefing for any AI agent acting here. Read it first.
**If a rule here conflicts with anything else, this file wins** — find a conflict,
fix it in a {{change}}, don't silently work around it.

## 1 · How we work — the loop

Every change rides the loop: **frame → plan → implement → open-change → gate →
close**. Two modes inside it:
- **Tactical autonomy** — mechanical moves (claim the {{issue}}, branch, code+doc+
  test, open the {{change}}, move status) need no permission; just do them and chain.
- **Strategic human loop** — the moment a move touches the **original design,
  strategy, or plan** (new dependency, a decision worth an ADR, scope split,
  ambiguity, a deviation from the source of truth), **stop and ask**. See
  *When in doubt*.

## 2 · Vocabulary & tracker

- Tracker: **{{tracker}}**. Work item = **{{issue}}**. Change request = **{{change}}**.
- Board: **{{board}}**; status field **{{status_field}}**. The board is the source of
  truth for workflow state — don't mirror it to labels (unless *Tracker notes* says
  the board *is* labels).

## 3 · Branch & change rules

- Trunk-based: **`{{default_branch}}`** stays green and releasable. Work on
  **`{{branch_pattern}}`**. Isolation: **{{isolation}}**.
- Every {{change}} **references its {{issue}}** in the body (the auto-close keyword,
  e.g. `Closes #<n>`), and uses the title format **`{{change_title_format}}`**.
- Merge style: **{{merge_style}}**.
- Required green checks before merge: **{{quality_gates}}**.
- **Size is a conversation trigger, not a target.** Past ~{{size_cap}} added lines,
  split separable concerns into linked {{issues}}; if the work is one cohesive thing
  (a sweeping rename, a vendored import, an ADR adoption), label it
  **`{{oversize_label}}`** and say why in the body. Never strip content that earns
  its keep just to fit.

## 4 · Status flow

Move the {{issue}} on **{{status_field}}** at each lifecycle moment:
planning → **{{status.planning}}**, work opens → **{{status.in_progress}}**,
{{change}} up for review → **{{status.in_review}}**, merged & criteria met →
**{{status.done}}**; blocked → **{{status.blocked}}**, dropped →
**{{status.deferred}}**. Tick acceptance boxes only with cited evidence.

## 5 · Docs & decisions

- Source of truth for product/architecture/process: **`{{docs_root}}`**. A change
  that alters behaviour updates the relevant doc **in the same {{change}}**.
- Non-trivial decisions (new framework/service, major library swap, divergence from a
  documented path) get an ADR in **`{{adr_dir}}`**, numbered monotonically; mark
  superseded ones.

## 6 · Engineering standards

<!-- InnoVestrum standards — adopt as-is, trimmed, or extended in the interview. -->
- **Code quality** — clean-code, DRY, SOLID, KISS, YAGNI, GRASP. Keep cyclomatic and
  cognitive complexity, coupling, and inheritance depth low. Comments explain only
  the non-obvious.
- **Architecture** — OOP via composition/inheritance/polymorphism; dependency
  injection & inversion of control; extensible but not over-engineered; prefer modern
  libraries.
- **Process** — ask before hardcoding a value to fix a bug (surface it in config, not
  inline). Keep `README.md` current. Tag temporary workarounds `TODO(ref):` with a
  removal trigger.
- **Cloud / infra** — 12-factor, cloud-native; Alpine-based images unless there's a
  hard reason.
- **Authoring** — write gaps, gotchas, pointers; cut what a competent agent already
  knows. Senior-engineer handover note, not a tutorial.

## 7 · Milestones <!-- keep if close-a-milestone adopted -->

Work groups under **{{milestone}}s**, worked in order. Each has an **exit criterion**
met with evidence before the next opens. Don't pull future-{{milestone}} scope
without recorded approval on the {{issue}}.

## 8 · Gates <!-- keep the rows for adopted gates only -->

Post-change verification beyond CI:
- **`review-gate`** — after CI is green on a {{change}} that produced an artifact,
  review it against its spec; self-heal small drift, escalate structural.
- **`source-sync-gate`** — check `{{source_of_truth}}` and its implementation haven't
  drifted.
- **`parity-gate`** — at {{milestone}} close, check `{{targets}}` stay equivalent.
- **`sync-docs`** — after a batch of merges, reconcile cross-document contradictions
  in `{{docs_root}}`.

## 9 · When in doubt

Ask — don't invent, don't silently choose. Precedence: **this file** > the more
specific doc > a general one. If you must move to keep momentum, pick the
conservative option and record the assumption on the {{issue}} so it can be ratified.

## Tracker notes

<!-- The establisher folds the chosen tracker's section from references/tracker-notes.md here. -->
{{tracker_notes}}

---

*Authoring rule for everything here:* **gaps, gotchas, pointers only** — never tell
an agent how to do what it already knows. Cut generic syntax and obvious procedure;
re-cut after drafting.
