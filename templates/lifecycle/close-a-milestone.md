---
name: close-a-milestone
description: >-
  Activate when every child {{issue}} under a {{milestone}} is closed and its exit
  criteria need final verification. Ties the {{milestone}} off with linked evidence and
  fires the cross-cutting gates (parity-gate, sync-docs). Distinct from track-status
  (moves one {{issue}}) and review-gate (one {{change}}'s artifact).
---

All children closed ≠ the {{milestone}} is done. Walk its exit criteria and tie each
off with evidence before declaring it complete.

1. **Tick each criterion with a link** — a merged {{change}}, a test report, a captured
   artifact, a log. A box with no link is unverified: chase the evidence or leave it
   open. A criterion that only a deliberate procedure can confirm (a field run, a load
   test, a manual check) — run it and link its output; never infer the tick from green
   CI.
2. **Cross-{{target}} {{milestone}}** (it shipped or changed one capability across
   `{{targets}}`) → run parity-gate, attach its verdict. A capability present on one
   {{target}} and absent on another **blocks** the parity tick until acked or carved out
   (carve-out-followup); idiom differences don't.
3. **Post the evidence as a closing note** on the {{milestone}} parent.
4. **Open the next {{milestone}}'s planning {{change}}** only if its outline says to
   expand on this one's close — otherwise leave the backlog to pick-next-work.
5. **Run sync-docs.** A {{milestone}}'s worth of merged work + decisions tends to leave
   the prose in `{{docs_root}}` contradicting itself or the shipped reality — reconcile
   the drift in one docs {{change}}.
6. **Fold a README update into that same docs {{change}}** if user-facing capabilities
   changed — not a separate one.

*(Reading the {{milestone}}'s children, posting the note, and opening the next planning
{{change}} are in `AGENTS.md` → Tracker notes — name the action here, run the tracker's
own commands there.)*
