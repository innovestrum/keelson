---
name: automate-or-runbook
description: >-
  Activate right after finishing a long manual delivery- or maintenance-side
  procedure (multi-step console clicks, an infra one-off spanning several tools,
  hand-rolled secret rotation) — capture the lesson before it evaporates. Chains to
  carve-out-followup when the steps are worth automating. Skip for short or
  single-command actions.
---

Three checks, in order. Leave the next person (or future-you) a path that isn't
"rediscover this from scratch."

1. **Is the procedure captured in a runbook?** Look under `{{docs_root}}`. If not,
   add or extend one — inline now, or as a small docs {{change}}. Capture the parts
   that took longest to figure out, not the parts that worked first try.
2. **Is there an automation path that would have removed steps?** An infra provider, a
   CI workflow or trigger, a vendor CLI subcommand. If none exists — the vendor has no
   API, common for store consoles and ToS click-throughs — say so; the runbook is the
   right artefact and you're done.
3. **If automation is possible, is it worth a follow-up?** Use carve-out-followup
   **only if** the steps will repeat (per-env, per-target, per-rotation) or are
   error-prone in practice. One-time bootstrap or unrecoverable-by-design steps → not
   worth it; the runbook stands alone.

Unsure between "runbook is enough" and "automate it" → pick **runbook**. A documented
manual step is recoverable; a half-automated one is a maintenance burden.

The trigger is *a long manual procedure just ended* — not every mention of a manual
step. Skip short CLI/`git` recoveries, one-command answers, codebase-only work, or
anything the user already wrapped in their own summary.

*(Filing the follow-up {{issue}} is in `AGENTS.md` → Tracker notes — name the action
here, run the tracker's own commands there.)*
