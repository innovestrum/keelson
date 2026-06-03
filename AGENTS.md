# AGENTS.md — working on keelson itself

keelson ports one repo's disciplined, issue-driven agentic flow into a portable,
**tracker-agnostic** skill any repo can adopt. It ships as a single skill — the
**establisher** (`skills/establish/SKILL.md`) — which interviews the user and fills
provider-neutral templates into their repo: a questionnaire-organized client
`AGENTS.md` plus a selectable lifecycle skill-pack. This file governs work **on
keelson**; it is not the template the establisher writes (that is
`skills/establish/templates/AGENTS.client.md`).

## The golden rule (holds for every file here)

**Never tell the agent how to do what it already knows.** Write only **gaps**
(what's missing where an agent would expect it), **gotchas** (silent failures,
exit-code lies, auto-behaviours, paywalls), and **pointers** (use X here, not Y).
Cut generic CLI/API syntax, config-file formats, and obvious procedure. Read like
a senior engineer's terse handover note, not a tutorial. Aim short; re-cut after
drafting. This rule is itself ported from the InnoVestrum standards the
establisher carries into client repos.

## How the pieces fit

- **One skill, self-contained.** Everything the establisher needs at runtime lives
  **inside `skills/establish/`** (templates, `references/tracker-notes.md`, Codex
  metadata). A plugin ships the skill folder, not the repo root — so nothing the
  skill reads may live in `docs/` or rely on a repo-root file. Root files
  (`README.md`, this `AGENTS.md`, `LICENSE`, packaging) are repo docs only.
- **Templates are plain language with `{{placeholders}}`.** Name the action
  ("create the issue", "open the change", "set status to `<column>`"); never
  hardcode `gh`/`glab`/a tracker's commands. The only `{{placeholders}}` are repo
  conventions (vocabulary, status columns, branch pattern, size cap, paths),
  filled from the interview. The agent derives the tracker's actual commands from
  the chosen tracker + the folded **Tracker notes**.
- **Tracker specifics live once**, in `references/tracker-notes.md` — traps,
  bindings, endpoints only. The establisher folds the chosen section into the
  client `AGENTS.md`.
- **Dual delivery, single copy.** Canonical skill at `skills/establish/` (Claude
  plugin reads it directly); `.agents/skills/establish` is a symlink to it (Codex
  follows symlinks — documented; Claude's symlink-following is not, so the real
  directory is on the Claude side).

## Distribution & updates (read before "fixing" an install)

- **`marketplace.json` uses a `url` source object**, not the bare string `"."` — older
  Claude Code versions reject `"."` with *"source type … your Claude Code version does
  not support"*.
- **`plugin.json` omits `version` on purpose.** For a git/url source Claude Code then
  uses the **commit SHA** as the version, so every push to `main` is a new version and
  reaches users. **Don't pin a `version` while iterating** — an unchanged version *is*
  the update cache key, so new commits stay invisible to anyone already installed (they
  keep the cached copy, e.g. a stale single-round establisher). Pin a semver only at a
  deliberate release, and bump it every change thereafter.
- **Updates are manual** on a third-party marketplace (auto-update is off by default):
  `/plugin marketplace update keelson` then `/plugin update keelson`. Cache lives at
  `~/.claude/plugins/cache/<marketplace>/<plugin>/<version>/`; if an update won't take,
  uninstall + reinstall forces a clean fetch.

## When changing the lifecycle pack

- Keep each template a **thin** port of its cruiso origin — the discipline, not the
  Cruiso-specific detail. If a placeholder isn't a repo convention, it doesn't
  belong; push tracker mechanics to `tracker-notes.md`.
- The 16 ideas are **selectable**: a client may adopt the 7 core-loop skills and
  none of the gates. Each template must read sensibly on its own and defer to the
  client `AGENTS.md` for anything configurable.
