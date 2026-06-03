---
name: update-source-of-truth
description: >-
  Activate when a NEW or CHANGED canonical element needs syncing into
  {{source_of_truth}}, or when the user wants to plan one before authoring it
  externally. Hands off through frame-the-change → plan-the-work →
  implement-the-change. Distinct from source-sync-gate (audits an already-merged
  {{source_of_truth}} against implementation) — this brings the element in.
---

`{{source_of_truth}}` is canonical. Whatever authored the element (an export, a
generator, an upstream spec) is **not** its home — sync the *delta* into the canonical
layout; never re-import a whole bundle.

## 1. Frame the run
- **Syncing a delta?** Confirm the incoming path carries only the new/changed element,
  not a full re-export. Work in {{isolation}} — never overwrite tracked
  `{{source_of_truth}}` files in place.
- **Planning ahead of authoring?** Scope the element, draft the {{issue}} listing what's
  expected to change, then stop. The agent does not run the external tool — the human
  does, then returns with the delta.

Neither applies → abort and clarify.

## 2. Port the delta into the one canonical home
- Classify each incoming file: **new** (port it), **changed** (review the diff, then
  replace), **identical** (skip). Put the classification in the plan.
- **One home per element.** If the incoming bundle duplicates something that already
  lives canonically elsewhere, drop the copy and point at the canonical one — never add
  a second home.
- **Re-link or it breaks silently.** Rewrite every reference in the ported files to the
  canonical paths; a stale link fails quietly, not loudly.
- An unexpected subtree (scrap, uploads, a competing copy of the canonical root) is a
  full-bundle smell, not a delta — escalate, don't auto-handle it.

## 3. Hand off — don't bypass the loop
End with a tracked {{issue}} + plan, then continue the normal chain (frame-the-change →
plan-the-work → implement-the-change) rather than auto-applying the sync. If the
canonical schema itself grew a new category, reconcile the docs under `{{docs_root}}` in
the same {{change}} (sync-docs).

## 4. Derived copies that don't auto-update
If the element feeds derived copies in `{{targets}}` that don't regenerate
automatically, open a follow-up {{issue}} per translation (carve-out-followup). Note in
the {{change}} that any drift gate against those targets stays red until the
translations land, so reviewers expect it.

*(Creating the {{issue}}, linking the parent, and setting `{{status_field}}` is in
`AGENTS.md` → Tracker notes — name the action here, run the tracker's own commands there.)*
