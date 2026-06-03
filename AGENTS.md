# AGENTS.md — working on keelson itself

keelson ports one repo's disciplined, issue-driven agentic flow into a portable,
**tracker-agnostic** pack any repo can adopt. It ships **two skills**:
**`adopt-keelson`** (the establisher — interviews the user and writes the whole operating
model) and **`tune-gates`** (establishes or refines just the quality-gate layer, standalone;
`adopt-keelson` defers its gate pillar to it). Both fill provider-neutral templates into the
target repo: a questionnaire-organized client `AGENTS.md` plus a selectable lifecycle
skill-pack. This file governs work **on keelson**; it is not the template the skills write
(that is `templates/AGENTS.client.md`).

## The golden rule (holds for every file here)

**Never tell the agent how to do what it already knows.** Write only **gaps**
(what's missing where an agent would expect it), **gotchas** (silent failures,
exit-code lies, auto-behaviours, paywalls), and **pointers** (use X here, not Y).
Cut generic CLI/API syntax, config-file formats, and obvious procedure. Read like
a senior engineer's terse handover note, not a tutorial. Aim short; re-cut after
drafting. This rule is itself ported from the InnoVestrum standards the
establisher carries into client repos.

## How the pieces fit

- **Two skills, one shared template set.** The lifecycle templates + tracker notes are read
  by **both** skills, so they live **once** at the plugin root — `templates/`
  (`AGENTS.client.md` + `lifecycle/*.md`) and `references/tracker-notes.md` — and each skill
  folder symlinks them in (`skills/<skill>/templates → ../../templates`, same for
  `references`). A skill reads `templates/lifecycle/<name>.md` as if local, the source keeps
  one copy (no drift), and each folder stays a portable self-contained unit. Don't add a
  second copy of a template inside a skill folder.
- **`adopt-keelson` owns the full model; `tune-gates` owns the gates.** The gate rounds, the
  which-gate-when selection, and the four gate templates (`review-gate`, `source-change-gate`,
  `source-sync-gate`, `parity-gate`) are `tune-gates`'. `adopt-keelson` points its gate pillar
  (P6/O2) there rather than re-deriving the rules — change gate logic in `tune-gates`, not in
  both.
- **Templates are plain language with `{{placeholders}}`.** Name the action
  ("create the issue", "open the change", "set status to `<column>`"); never
  hardcode `gh`/`glab`/a tracker's commands. The only `{{placeholders}}` are repo
  conventions (vocabulary, status columns, branch pattern, size cap, paths),
  filled from the interview. The agent derives the tracker's actual commands from
  the chosen tracker + the folded **Tracker notes**.
- **Tracker specifics live once**, in `references/tracker-notes.md` — traps,
  bindings, endpoints only. The establisher folds the chosen section into the
  client `AGENTS.md`.
- **Dual delivery, single copy.** Claude installs the whole plugin to its cache (in-plugin
  symlinks are preserved/dereferenced, so each skill resolves its `templates/`); Codex
  discovers each skill under `.agents/skills/<skill>` — a symlink to the real `skills/<skill>/`
  (Codex follows the nested `templates` symlink within the clone). Root files (`README.md`,
  this `AGENTS.md`, `LICENSE`, packaging) are repo docs only.

## Distribution & updates (read before "fixing" an install)

- **`marketplace.json` uses a `url` source object**, not the bare string `"."` — older
  Claude Code versions reject `"."` with *"source type … your Claude Code version does
  not support"*.
- **Bump `plugin.json` `version` (semver) on every release — it is the update signal.**
  Claude Code keys updates on the version string: an unchanged version means installed
  users keep the cached copy (the stale single-round establisher we hit). Omitting
  `version` (commit-SHA versioning) is documented but proved unreliable in practice —
  pin an explicit semver and **bump it every change**, so the new value out-ranks what
  users have installed.
- **Cut a matching `vX.Y.Z` git tag + GitHub release** per version. Release hygiene, and
  it gives an anchor if the marketplace source is ever pinned to a ref/release channel.
- **Updates are manual** on a third-party marketplace (auto-update is off by default):
  `/plugin marketplace update keelson` then `/plugin update keelson`. Cache lives at
  `~/.claude/plugins/cache/<marketplace>/<plugin>/<version>/`; if an update won't take,
  `/plugin uninstall keelson` + reinstall forces a clean fetch.

## When changing the lifecycle pack

- Keep each template a **thin** port of its cruiso origin — the discipline, not the
  Cruiso-specific detail. If a placeholder isn't a repo convention, it doesn't
  belong; push tracker mechanics to `tracker-notes.md`.
- The 17 ideas are **selectable**: a client may adopt the 7 core-loop skills and
  none of the gates. Each template must read sensibly on its own and defer to the
  client `AGENTS.md` for anything configurable.
- **The four gates are a family** — `review-gate` (spec → implementation),
  `source-change-gate` (the source itself changed; ported from cruiso `design-visual-gate`),
  `source-sync-gate` (source ↔ implementation), `parity-gate` (`{{target}}` ↔ `{{target}}`).
  Each template's boundary footer says which pair it is *not* — keep those in sync when you
  touch one. Gate **selection** logic belongs in `tune-gates`, not the templates.
