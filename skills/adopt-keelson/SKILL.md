---
name: adopt-keelson
description: >-
  Adopt keelson's issue-driven agentic operating model in this repo. Runs a
  deliberate, multi-round interview (≈8–20 rounds: it maps your SDLC, then makes you
  decide each pillar), then writes a tracker-agnostic client AGENTS.md plus the selected
  lifecycle skills — provider-neutral across GitHub, GitLab, Jira, Linear, or any
  API/CLI/MCP. The gate pillar defers to the tune-gates skill (run it standalone later to
  add/tune a gate). Trigger on "adopt keelson", "set up the agentic workflow", "establish
  the issue-driven flow", or a repo that wants an AGENTS.md.
---

# Adopt keelson

Port a disciplined, issue-driven flow — **frame → plan → implement → open-change → gate
→ close**, with *tactical autonomy* (the agent does the mechanical moves) and a
*strategic human loop* (it escalates the moment a move touches the original design,
plan, or strategy) — into **this** repo, against **its** tracker and SDLC.

You produce two things in the target repo:
1. **`AGENTS.md`** — a questionnaire-organized substrate the skills defer to.
2. **A lifecycle skill-pack** — the adopted ideas, written as the repo's own skills in
   plain language (they name actions; they read tracker bindings from `AGENTS.md`, never
   hardcode them).

Everything you need is in this skill folder: `templates/AGENTS.client.md`,
`templates/lifecycle/*.md`, `references/tracker-notes.md`. Fill, don't invent.

## 1 · How to run the interview

**This is a configuration session, not a quick yes/no.** Expect **≈8–20 rounds** — fewer
for a simple repo, more for a complex one; use as many as it takes to make the flow
unambiguous. Don't rush it; the payoff is an `AGENTS.md` the user actually agreed to.

- **One topic per round.** State what the reference flow does, then make the **user
  decide** for their repo. Don't silently default the consequential calls and move on —
  surface the choice and get an explicit nod before advancing.
- **The user owns the flow.** keelson's defaults are a starting point, not the answer.
  Where they're unsure, offer the conservative option, mark it to revisit, don't bury it.
- **Discover before you propose.** Run the discovery rounds first; learn what they have,
  lack, or want — then propose only the ideas that fit the gaps.
- **Record as you go; confirm at the end.** The final round replays every decision for
  one ack before you write anything.

## 2 · The rounds

Discovery first, then a round per pillar, then the optional families the discovery
surfaced, then standards + delivery, then confirm. Each round names the skill(s) it
configures and the placeholders (§3) it fills. Merge rounds for a simple repo, split them
for a complex one.

**Discovery**
- **D1 · SDLC map.** Open questions: what delivery stages/pillars exist today (intake →
  triage → design → build → review → QA → release → ops)? Which are solid, which are
  ad-hoc or missing, which do they want keelson to establish or tighten? This drives
  which ideas you later propose — listen before prescribing.
- **D2 · Tracker & vocabulary.** Tracker; what they call a work item / a change / the
  board; the `{{status_field}}` columns and the lifecycle moment each maps to.
- **D3 · Repo & branching.** `{{default_branch}}`, `{{branch_pattern}}`, `{{isolation}}`
  (worktree or branch-only), `{{merge_style}}`, mono- vs poly-repo.

**Pillars — decide how each works (adopt the skill + capture its conventions)**
- **P1 · Intake / framing** (`frame-the-change`) — does every change start as a tracked
  {{issue}}? what must it contain (context / out-of-scope / acceptance)?
- **P2 · Planning & where the plan lives** (`plan-the-work`) — when is a plan required,
  and **where is it filed**: an {{issue}} comment, an in-repo design doc, a wiki page, or
  the {{change}} body? → `{{plan_location}}`.
- **P3 · Docs & decision records** — the doc home(s): in-repo `{{docs_root}}`, an external
  wiki (e.g. Confluence/Notion), or a mix → `{{docs_system}}`; ADRs — used, and where
  (`{{adr_dir}}`)? the doc-update rule (which changes must update which docs, same
  {{change}}).
- **P4 · Implementation discipline** (`implement-the-change`) — code + doc + tests
  together? never hardcode (surface in config)? scope limited to the {{issue}}?
- **P5 · Opening changes** (`open-the-change`) — `{{change_title_format}}`, link the
  {{issue}} in the body, `{{size_cap}}` + `{{oversize_label}}`, self-review.
- **P6 · CI quality gates** (via **tune-gates** G1) — `{{quality_gates}}`; CI-owned (don't
  pre-run before opening), with a local-verify carve-out during red-CI debugging.
- **P7 · Status flow** (`track-status`) — when status moves and who moves it; board vs.
  labels as source of truth; close only with cited evidence.
- **P8 · Pick-up rule** (`pick-next-work`) — the backlog/priority ordering for claiming
  the next {{issue}}.

**Optional families — propose only what D1 surfaced; adopt or skip with the user**
- **O1 · Milestones / releases** (`close-a-milestone`) — stages / milestones / epics /
  sprints with exit criteria? release model? → `{{milestone}}`.
- **O2 · Verification gates** (via **tune-gates** G2) — a reviewable artifact → `review-gate`;
  a canonical `{{source_of_truth}}` → the `source-change-gate` + `source-sync-gate` pair;
  multiple `{{targets}}` → `parity-gate`. tune-gates owns the gate rounds, selection rules, and
  templates — run it for this pillar; it returns the adopted set for the unified write (§4).
- **O3 · Doc consistency & source updates** — `sync-docs` (reconcile after batches) and
  `update-source-of-truth` (flow a source-of-truth change through the loop).

**Discipline — the human loop**
- **H1 · Tactical vs. strategic boundary** — *the crux.* Have the user draw the line:
  which moves the agent does autonomously vs. must escalate for acknowledgement.
- **H2 · Uncertainty & precedence** (`respond-to-uncertainty`) — resolving ambiguity and
  doc conflicts; ask vs. proceed-conservatively.
- **H3 · Follow-ups, prior-art, ops** (`carve-out-followup`, `check-prior-art`,
  `automate-or-runbook`) — adopt each?

**Standards & delivery**
- **S1 · Engineering standards** — present the InnoVestrum standards embedded in
  `templates/AGENTS.client.md → Engineering standards`; adopt as-is, trim, or extend.
- **S2 · Where skills are written** — Claude `.claude/skills/`, Codex `.agents/skills/`,
  or both; an existing `AGENTS.md` → merge into it, don't overwrite.

**Confirm**
- **C1 · Replay & confirm.** Summarize every decision, the adopted vs. skipped ideas, and
  any gap the user chose to leave open. Get one final ack — then fill & write.

## 3 · Placeholders (the only things you fill)

Only **repo conventions** are placeholders; action verbs stay plain. Resolve every one;
leave none unfilled.

- **Vocabulary** — `{{tracker}}`, `{{issue}}` / `{{issues}}`, `{{change}}`
- **Board** — `{{board}}`, `{{status_field}}`, columns used: `{{status.planning}}` ·
  `{{status.in_progress}}` · `{{status.in_review}}` · `{{status.done}}` ·
  `{{status.blocked}}` · `{{status.deferred}}` (omit unused)
- **Branch / change** — `{{default_branch}}`, `{{branch_pattern}}`,
  `{{change_title_format}}`, `{{merge_style}}`, `{{isolation}}`
- **Docs / decisions** — `{{docs_system}}` (in-repo / wiki / mixed), `{{docs_root}}`,
  `{{plan_location}}`, `{{adr_dir}}` (any may be *none*)
- **Scope / gates** — `{{size_cap}}` + `{{oversize_label}}`, `{{milestone}}`,
  `{{source_of_truth}}`, `{{target}}` / `{{targets}}`, `{{quality_gates}}` (any *none*)

## 4 · Fill & write

1. **Client `AGENTS.md`.** Render `templates/AGENTS.client.md`: substitute every
   placeholder, **keep only adopted sections**, embed the standards from S1, and **fold
   the chosen tracker's section** from `references/tracker-notes.md` under
   `## Tracker notes`. Write to repo root (or merge into the existing file).
2. **Lifecycle skills.** For each adopted idea, render `templates/lifecycle/<name>.md`
   (placeholders + frontmatter description) and write it to each client skill dir as
   `<dir>/<name>/SKILL.md`. The **gate** skills + the `§8`/`§3` substrate follow
   **tune-gates §4** — its selection (P6/O2) decides which gates are adopted; render only those.
3. Skipped ideas → don't write the skill, don't leave its `AGENTS.md` section.

## 5 · Verify & hand back

- Grep the written files for `{{` — **zero** leftover placeholders.
- Every adopted idea has both an `AGENTS.md` section and a written skill; every written
  skill's `{{status.*}}` / `{{change}}` / `{{plan_location}}` etc. match the interview.
- Recap: tracker, the SDLC pillars adopted vs. left to the user, files written, and any
  decision still flagged to revisit. Point them at the new `AGENTS.md` to read first.

## Gotchas

- **Make the user decide; don't auto-default the consequential calls.** Vocabulary,
  status columns, where plans and docs live, the tactical/strategic line — these are the
  user's to set. A silently-defaulted `AGENTS.md` is one nobody agreed to.
- **The pack is selectable, gates most of all.** Write only adopted ideas; a backend repo
  with no canonical source and one target skips the source + parity gates entirely. The gate
  selection rules — and the "never write a gate with a null binding" trap (no
  source-change/source-sync-gate when `{{source_of_truth}}` is *none*; no parity for one
  `{{target}}`) — live in **tune-gates**; apply them, don't re-derive them here.
- **Don't transcribe a tracker's commands into the skills.** Name the action; the
  bindings live once, in `AGENTS.md → Tracker notes`. About to write `gh`/`glab` into a
  skill? Stop — push it to the tracker note.
- **The standards are the floor, not decoration** — the pack defers to them for code
  quality; don't also restate them inside the skills.
- **Hold every written line to the golden rule** (keelson `AGENTS.md`): gaps, gotchas,
  pointers only. A filled skill that explains how to open a PR in general is wrong; name
  the step and defer the rest.
