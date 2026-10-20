---
name: open-the-change
description: >-
  Activate after implement-the-change, when you're ready to open the {{change}}.
  Opens it, links the {{issue}} in the body, watches CI to green, runs the adopted
  gates, and stops there. Merge, recap and close-out are track-status's.
---

Implementation is done. Open the {{change}} and prove it green and gated. Tracker bindings (open, link, set status, merge) live in `AGENTS.md` → Tracker
notes — name the action here, run them there.

## Open
1. **Title** per `{{change_title_format}}`; **link the {{issue}} in the body**, not only
   the title — most trackers' auto-close scans body text, so a title-only reference
   leaves the {{issue}} open after merge.
2. **Body lists every out-of-code change** — each doc updated, each ADR added under
   `{{adr_dir}}`. Review-enforced, not CI-enforced; an unlisted doc change is the one a
   reviewer misses.
3. **Self-review the diff first** — strip debug output, dead code, and any stray `TODO`
   without a linked {{issue}}.
4. **Over `{{size_cap}}` → reach for `{{oversize_label}}` first, not after a trim
   cycle.** The cap is a split-or-justify trigger, not a number to shave under — don't
   delete docstrings or split cohesive tests to drop the last lines. Note in the body
   *why* it was bypassed.

## CI owns the gates
- **Don't run the gates locally before opening** — opening doesn't wait on a local pass.
- **But local-verify during a red-CI loop is expected.** When CI fails in a way that
  isn't a one-line typo (timeout, flake, async/scheduler interaction — any failure whose
  mechanism is unclear), reproduce it locally before the next push. Can't reproduce →
  comment with logs and ask; never blind-push-retry.
- **Watch CI to terminal before handing back** — after open and after every push to the
  {{change}}. Never report "done" while checks are pending or red.

## Gate, then flip
- CI green → chain the adopted gates before the recap: a reviewable artifact → **review-gate**;
  the {{change}} **edits `{{source_of_truth}}` itself** → **source-change-gate**; it touches an
  **implementation of `{{source_of_truth}}`** → **source-sync-gate**. A self-heal a gate pushes
  re-triggers CI — re-watch and re-enter until findings settle. No artifact / pure logic → skip.
- Set `{{status_field}}` → **{{status.in_review}}** when the {{change}} opens (or
  **{{status.in_progress}}** if that's where your board parks open changes) via
  track-status.

## Done here
CI green and the gates settled → stop. The merge, the recap and the close-out belong to
track-status — the orchestrator's moves when the loop is delegated (`AGENTS.md` §10).
