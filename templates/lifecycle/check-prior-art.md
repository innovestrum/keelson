---
name: check-prior-art
description: >-
  Activate when stuck on a stubborn bug, or about to hand-roll something complicated —
  timebox a web / {{tracker}} search for the existing fix, library, or tool before going
  deep. Feeds carve-out-followup when the answer is a new runtime dep; uses
  respond-to-uncertainty when the bespoke-vs-dependency call is unclear.
---

Don't reinvent. Assume someone already hit your bug or built your tool — a few minutes
searching beats hours of self-debugging or bespoke code. A *reflex before the deep dive*,
not a research project.

**Two triggers:**

- **Stuck on a bug** — failure right after a version / runner / toolchain bump you didn't
  author; a stack frame pointing into a dependency; green locally, red in the {{change}}'s
  CI. → likely a known upstream bug with a fix already shipped.
- **About to hand-roll something complicated** — parsing, scheduling, retries, a state
  machine, a wire format. → a maintained library probably already does it better.

**Quick check (timebox ~5 min):**

- Search the web **and** the {{issues}} for the *exact* symptom or capability — open bug
  reports, alternatives, version-specific behaviour. Use whatever docs/version lookup the
  repo's tooling provides for library APIs.
- **Bug:** match the symptom to a *phase, not a vibe* — a progress bar that hits 100% then
  stalls points at a post-transfer step, not the network. Compare the **resolved** version
  (lockfile, not the manifest range) against the "fixed-in" version — usually a one-line bump.
- **Tool:** prefer a maintained, well-adopted dependency over bespoke code (defer to the
  engineering standards in `AGENTS.md`). A new runtime dep is a **strategic call**, not a
  silent import — flag it (an ADR if the repo keeps them, or carry it into the {{issue}} via
  carve-out-followup), don't smuggle it in.

Record the upstream link / chosen tool + version on the {{issue}} or {{change}} so nobody
re-investigates. No clear hit fast → fall back to normal work; don't rabbit-hole the search.

*(Where to search and record — the {{issues}}, the {{change}} body — is in `AGENTS.md` →
Tracker notes; name the action here, run the tracker's own commands there.)*
