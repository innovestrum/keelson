---
name: establish
description: >-
  Establish keelson's issue-driven agentic operating model in this repo. Runs a
  short interview (one round per idea distilled from a reference flow), then writes
  a tracker-agnostic client AGENTS.md plus the selected lifecycle skills — provider-
  neutral, working with whatever tracker the repo uses (GitHub, GitLab, Jira, Linear,
  or any API/CLI/MCP). Trigger on "adopt keelson", "set up the agentic workflow",
  "establish the issue-driven flow", or when a repo has no AGENTS.md and wants one.
---

# Establish keelson

Port a disciplined, issue-driven flow — **frame → plan → implement → open-change →
gate → close**, with *tactical autonomy* (the agent just does mechanical moves) and
a *strategic human loop* (it escalates the moment a move touches the original
design, plan, or strategy) — into **this** repo, against **its** tracker.

You produce two things in the target repo:
1. **`AGENTS.md`** — a questionnaire-organized substrate the skills defer to.
2. **A lifecycle skill-pack** — the selected ideas below, written as the repo's own
   skills, in plain language (they name actions; they never hardcode a tracker's
   commands — they read the tracker bindings from `AGENTS.md`).

Everything you need is in this skill folder: `templates/AGENTS.client.md`,
`templates/lifecycle/*.md`, and `references/tracker-notes.md`. Fill, don't invent.

## 1 · Orient (before the interview)

- **Tracker.** Which one backs this repo's work items? Read its section in
  `references/tracker-notes.md` — those are the only bindings you might get wrong.
- **Where do skills go?** Ask the user's agent tools → client skill dir(s):
  Claude Code → `.claude/skills/`, Codex/other → `.agents/skills/`, or both. One
  filled `SKILL.md` per adopted idea per dir.
- **Existing `AGENTS.md`?** Merge into it (keep their rules, add keelson's sections)
  rather than overwrite. No file → create from the template.

## 2 · Interview — one round per idea

Keep it light: state each idea in a line, ask **adopt?** and capture only the
conventions it needs. Batch the yes/nos; don't interrogate. The first 10 are
near-universal (default **on**); the last 6 depend on repo shape — offer them only
when the repo actually has a milestone model / a canonical source / multiple targets
/ a docs set. **Round 0** and the **standards** round always run.

**Round 0 — Foundations.** Capture the placeholders in §3: tracker, vocabulary,
branch + default branch, board + status columns, change-title format + merge style,
isolation, docs root.

**Standards round.** Present the InnoVestrum engineering standards (embedded in
`templates/AGENTS.client.md` → *Engineering standards*). Adopt as-is, trim, or
extend. These land verbatim-ish in the client `AGENTS.md` and are what the pack's
"defer to AGENTS.md" leans on for code quality.

**Idea rounds** — the pack:

| # | Idea (skill name) | Essence | Adds / needs | Default |
|---|---|---|---|---|
| 1 | `frame-the-change` | every change starts as a tracked {{issue}} (Context / Out-of-scope / Acceptance) **before** code | — | on |
| 2 | `pick-next-work` | claim the next {{issue}} by the backlog rule; clean `{{default_branch}}` first | backlog/priority rule | on |
| 3 | `plan-the-work` | post a plan on the {{issue}} before code; ADR for non-trivial calls | `{{adr_dir}}` | on |
| 4 | `implement-the-change` | work in {{isolation}}; code + doc + tests together; never hardcode a value | — | on |
| 5 | `open-the-change` | open the {{change}}, link the {{issue}} in the body, watch CI to green, recap | `{{change_title_format}}`, `{{merge_style}}` | on |
| 6 | `track-status` | move the {{issue}} across `{{status_field}}` at each lifecycle moment; close with evidence | status columns | on |
| 7 | `respond-to-uncertainty` | precedence (AGENTS.md wins; specific > general); ask vs. proceed-conservatively | — | on |
| 8 | `carve-out-followup` | track out-of-scope work as a new {{issue}}; don't grow the {{change}} | — | on |
| 9 | `check-prior-art` | search the web / tracker before hand-rolling or deep-debugging | — | on |
| 10 | `automate-or-runbook` | after a long manual procedure, capture a runbook or automate it | runbook dir | on |
| 11 | `close-a-milestone` | verify a {{milestone}}'s exit criteria with linked evidence | `{{milestone}}` | shape |
| 12 | `review-gate` | after CI green, review the produced artifact against its spec; self-heal small drift, escalate structural | artifact kind | shape |
| 13 | `source-sync-gate` | canonical `{{source_of_truth}}` ↔ implementation drift | `{{source_of_truth}}` | shape |
| 14 | `parity-gate` | `{{targets}}` ↔ each other: same capability, idioms allowed | `{{targets}}` | shape |
| 15 | `sync-docs` | after a batch of changes, reconcile cross-document contradictions | `{{docs_root}}` | shape |
| 16 | `update-source-of-truth` | sync a new/changed `{{source_of_truth}}` element through the loop | `{{source_of_truth}}` | shape |

## 3 · Placeholders (the only things you fill)

Only **repo conventions** are placeholders. Action verbs stay plain. Resolve every
one from the interview; leave none unfilled.

- **Vocabulary** — `{{tracker}}`, `{{issue}}` / `{{issues}}`, `{{change}}`
- **Board** — `{{board}}`, `{{status_field}}`, and the columns you use:
  `{{status.planning}}` · `{{status.in_progress}}` · `{{status.in_review}}` ·
  `{{status.done}}` · `{{status.blocked}}` · `{{status.deferred}}` (omit unused)
- **Branch / change** — `{{default_branch}}`, `{{branch_pattern}}`,
  `{{change_title_format}}`, `{{merge_style}}`, `{{isolation}}`
- **Docs / decisions** — `{{docs_root}}`, `{{adr_dir}}` (or *none*)
- **Scope / gates** — `{{size_cap}}` + `{{oversize_label}}`, `{{milestone}}`,
  `{{source_of_truth}}`, `{{target}}` / `{{targets}}`, `{{quality_gates}}` (any may
  be *none*)

## 4 · Fill & write

1. **Client `AGENTS.md`.** Render `templates/AGENTS.client.md`: substitute every
   placeholder, **keep only adopted sections**, embed the standards from the
   standards round, and **fold the chosen tracker's section** from
   `references/tracker-notes.md` under `## Tracker notes`. Write to repo root (or
   merge into the existing file).
2. **Lifecycle skills.** For each adopted idea, render
   `templates/lifecycle/<name>.md` (placeholders + frontmatter description) and write
   it to each client skill dir as `<dir>/<name>/SKILL.md`.
3. Skipped ideas → don't write the skill and don't leave its `AGENTS.md` section.

## 5 · Verify & hand back

- Grep the written files for `{{` — **zero** leftover placeholders.
- Every adopted idea has both an `AGENTS.md` section and a written skill; every
  written skill's `{{status.*}}` / `{{change}}` etc. match Round 0.
- Recap: tracker, ideas adopted vs. skipped, files written, and the one or two
  conventions you defaulted (so the user can correct). Point them at the new
  `AGENTS.md` as the thing to read first.

## Gotchas

- **Don't transcribe a tracker's commands into the skills.** The pack is portable
  because it names actions; the bindings live once, in `AGENTS.md` → *Tracker notes*.
  If you're about to write `gh`/`glab` into a lifecycle skill, stop — push it to the
  tracker note instead.
- **Selectable means selectable.** A backend-only repo with no design source and one
  deploy target adopts ideas 1–10 and `sync-docs`, and nothing else. Don't write a
  `source-sync-gate` with `{{source_of_truth}} = none`.
- **The standards are the floor, not decoration.** They're what `implement-the-change`
  means by "clean code"; if the user trims them, the pack still defers to whatever
  remains — so don't also restate code-quality rules inside the skills.
- **Hold every written line to the golden rule** (`AGENTS.md` of keelson): gaps,
  gotchas, pointers only. A filled skill that explains how to open a PR in general is
  wrong; it should name the step and defer the rest.
