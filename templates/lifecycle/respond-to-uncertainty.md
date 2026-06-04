---
name: respond-to-uncertainty
description: >-
  Activate when a requirement is ambiguous, or two docs in {{docs_root}} (or a doc and
  the {{issue}}) appear to disagree. The strategic-human-loop arbiter the other
  lifecycle skills defer to whenever a move is underdetermined.
---

Resolve by precedence; only ask when precedence can't.

1. **`AGENTS.md` wins** over everything — it's the substrate the rest defers to.
2. **Specific beats general** — the doc scoped to the exact thing outranks the broad
   overview; the {{issue}}'s acceptance criteria outrank a general guideline.
3. **Still ambiguous → ask** on the {{issue}}. Don't infer silently; a wrong silent
   guess costs more than the question.
4. **If you must move to keep momentum**, take the **conservative** option (easiest to
   reverse / smallest blast radius) and **record the assumption on the {{issue}}** so
   the user can ratify or reject. A logged assumption is recoverable; an unlogged one is
   a latent bug.

This is the line between tactical autonomy and the strategic loop: mechanical moves you
just make; anything turning on a design, plan, or spec call you escalate here.

*(Leaving the assumption / question on the {{issue}} is in `AGENTS.md` → Tracker notes —
name the action here, run the tracker's own commands there.)*
