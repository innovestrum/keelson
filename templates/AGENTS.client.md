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
  ambiguity, a deviation from the source of truth), **escalate**. See
  *When in doubt*.

**Escalate** = ask the user. A delegated subagent can't, so it stops and returns
`ESCALATE` to the orchestrator, which asks (§10). The skills say "escalate" and mean
whichever applies to you.

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

- **Doc system: {{docs_system}}** (in-repo / external wiki / mixed). Source of truth for
  product/architecture/process lives at **`{{docs_root}}`**. A change that alters
  behaviour updates the relevant doc **in the same {{change}}**.
- **Plans are filed at {{plan_location}}** — reviewers and future-you read them there.
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

## 8 · Gates <!-- keep the rows for adopted gates only; configured by the tune-gates skill -->

Post-change verification beyond CI:
- **`review-gate`** — after CI is green on a {{change}} that produced an artifact,
  review it against its spec; self-heal small drift, escalate structural.
- **`source-change-gate`** — when the {{change}} edits `{{source_of_truth}}` itself,
  realize it, confirm the intended change, escalate collateral drift to other elements.
- **`source-sync-gate`** — check `{{source_of_truth}}` and its implementation haven't
  drifted.
- **`parity-gate`** — at {{milestone}} close, check `{{targets}}` stay equivalent.
- **`sync-docs`** — after a batch of merges, reconcile cross-document contradictions
  in `{{docs_root}}`.

## 9 · When in doubt

Ask — don't invent, don't silently choose. Precedence: **this file** > the more
specific doc > a general one. If you must move to keep momentum, pick the
conservative option and record the assumption on the {{issue}} so it can be ratified.

## 10 · Delegation <!-- keep if delegate-roles adopted -->

The skills are shared by the main session and its subagents; this section says **who**
runs them. Role definitions live in `{{agents_dir}}`.

| Role | Runs | Hands back |
|---|---|---|
| **Main — orchestrator** | pick-next-work, frame-the-change, escalations, merge, track-status close-out, close-a-milestone | the recap; every call that is the user's |
| **loop-runner** | plan-the-work → implement-the-change → open-the-change → gates | a green, gated {{change}} or `ESCALATE` — never merges |
| **gate-reviewer** | one adopted gate, on a fresh context | findings |
| **scout · gate-runner · change-watcher** | no lifecycle skills | compact records; never decide |

- **Delegate code work by default** — one loop-runner per {{issue}}; parallel ones own
  disjoint files and each dispatch names its siblings. Main works inline only on:
  {{inline_exceptions}}.
- **One {{issue}} per dispatch, short lives.** Resume a loop-runner only with an escalation
  answer or a pre-merge fix. The next {{change}} of a stack, a post-merge rebase or a
  follow-up gets a fresh dispatch with a short brief: bundled and long-resumed runs were
  the costliest by far, because every call re-reads the whole accumulated context.
- **Main never reads images.** It links a screenshot, names the region in words, and has
  scout describe one it must judge. An image main reads stays in its context for the rest
  of the run.
- **Escalation contract.** A subagent can't ask the user or message main mid-run. It stops
  and returns `ESCALATE: {question, options, recommendation, blocked step}`; main asks,
  then resumes the *same* agent with the answer (its context survives). A tactical,
  reversible choice isn't an escalation — take the conservative option and record it on
  the {{issue}} (§9). **Never park a strategic call as a {{change}}-body footnote** — that
  is drift surfacing late, the thing this loop exists to stop.
- **Nested dispatch is foreground-only.** A background child reports to the top-level
  session, not to the subagent that dispatched it. Background dispatch is main's alone;
  a notification for an agent main didn't dispatch gets forwarded to its dispatcher.
- **Review on a different model where you can.** Stacked judgment on one model shares
  its blind spots; gate-reviewer runs on {{model.reviewer}}. The same model on a fresh
  context is the cheaper fallback.
- **Context is the bill.** Each call re-reads the role's whole context, so:
  - Give every role an explicit `tools:` list. An all-tools role starts about 3× larger,
    and a missing tool silently breaks a skill step, so list what its skills use.
  - Give long-lived roles that wait on children (loop-runner) the 1-hour prompt cache.
    A 5-minute cache expires during a CI wait, and the next call re-writes the whole
    context. Short leaf roles keep the default, because 1-hour writes cost more.
- **Move a role to a cheaper tier only after calibration.** Run the cheaper model in the
  shadow of the current one on the same inputs: a real failure, a pass, and a run that
  can't start. Switch only if every report matches, and record the table on the
  {{issue}}.

## Tracker notes

<!-- The establisher folds the chosen tracker's section from references/tracker-notes.md here. -->
{{tracker_notes}}

---

*Authoring rule for everything here:* **gaps, gotchas, pointers only** — never tell
an agent how to do what it already knows. Cut generic syntax and obvious procedure;
re-cut after drafting.
