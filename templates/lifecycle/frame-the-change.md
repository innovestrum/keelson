---
name: frame-the-change
description: >-
  Activate the moment the user asks for any change — "fix X", "add Y", "tweak Z", a
  screenshot with a callout. Forms a tracked {{issue}} with a real plan before any
  code. Distinct from pick-next-work (claims an existing {{issue}}) and plan-the-work
  (plans the one this skill just created).
---

User asked for a change. No code yet. Drive the work from a tracked {{issue}}, not a
dirty working tree.

1. **Restate scope in 1–2 lines** before any tool call — confirms understanding and
   lets the user redirect cheaply. If the request and the docs in `{{docs_root}}`
   disagree, use respond-to-uncertainty first.
2. **Find parent + sibling.** Look up the owning {{milestone}}/parent {{issue}} and a
   recent sibling; mirror its labels and body shape rather than inventing a new one.
3. **Trade-off check against recent merges.** Skim the history of the files/area
   you're about to touch — a value you're about to change may have been set
   deliberately last week. If your change is in tension with one, **call it out in the
   {{issue}} body under Context**, don't bury it.
4. **Create the {{issue}}** with:
   - **Context** — what surfaces, known constraints, which recent change(s) it
     interacts with, the trade-off if any.
   - **Out of scope** — what this explicitly does not touch.
   - **Acceptance** — checkable, user-visible outcomes (boxes).
   - **Refs** — the parent {{issue}}/{{milestone}}.
5. **Hand to plan-the-work** — post the 3–7 bullet plan as a comment and set
   `{{status_field}}` → **{{status.planning}}**. The plan is what the user reviews to
   redirect scope; the body is what reviewers and future-you read.
6. **Wait for acknowledgement before coding** — unless the user already said "go" in
   the same turn (that counts as ack). Tight loops are fine; a one-word ack is fine.
7. **Continue the chain** once acked: implement-the-change → open-the-change → on
   merge the {{issue}} closes. Verify it flipped to **{{status.done}}**; fall back to
   track-status if it didn't.

Skip only for: existing-{{issue}} work (pick-next-work → plan-the-work); throwaway
exploration the user framed as untracked; a one-character fix already approved
verbally this turn.

*(How to create the {{issue}}, link the parent, and set status is in `AGENTS.md` →
Tracker notes — name the action here, run the tracker's own commands there.)*
