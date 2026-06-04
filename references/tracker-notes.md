# Tracker notes

Per-tracker traps, bindings, and endpoints only — the non-obvious things an agent
that already knows `gh`/`glab`/the tracker's MCP would still get wrong. The
establisher folds the **chosen** tracker's section into the client `AGENTS.md`
(`## Tracker notes`); the lifecycle skills name actions in plain language
("create the issue", "set status to <column>") and rely on this section for the
bindings. Generic syntax is deliberately absent — only surprises are here.

Every section answers the same four questions, because the lifecycle reduces to
them: **Status** (how a work-item moves columns) · **Hierarchy** (child→parent
link) · **Change** (where the PR/MR lives, how it links back) · **Endpoint** (the
API/MCP to drive when there's no CLI verb). Add a section when you adopt a
tracker; keep it to surprises.

## GitHub — `gh` + GitHub MCP
- **Status** lives on a Projects v2 **single-select field**, not on the issue — the
  issue must be on the board first. Set it via the MCP project-item update or
  `gh project item-edit`; don't mirror it to labels.
- **Sub-issue link** wants the **REST database id**, not the GraphQL node id
  (`I_kwDO…` 422s as "not of type integer") — resolve parent and child with
  `gh api repos/<o>/<r>/issues/<n> --jq .id` first. Falls back to a `Refs #<n>` line.
- `gh pr checks <n> --watch --required` exits instantly with "no required checks"
  when branch protection defines none — watch without `--required`.
- MCP endpoint: `https://api.githubcopilot.com/mcp/`.

## GitLab — `glab` + GitLab MCP
- **Board state** is backed by **scoped labels** — the `workflow::<column>` label
  *is* the column; a plain label won't move the card.
- **Change = Merge Request**, not PR. `Closes #<n>` in the MR description auto-closes.
- **Hierarchy**: epic link on Premium; else a `Refs #<n>` line (no sub-issues on Free).
- MCP: `npx -y @modelcontextprotocol/server-gitlab`, env `GITLAB_PERSONAL_ACCESS_TOKEN`.

## Jira — Atlassian MCP / REST
- **Status is gated by workflow transitions** — you can't write the status field;
  fetch the legal transitions from the current status and apply one (an illegal jump
  is rejected). Transition names ≠ status names.
- **Change lives in a linked VCS** — open/merge there, put the key (`PROJ-123`) in the
  branch and PR title; Smart Commits (`PROJ-123 #close`) advance the issue only if the
  integration is enabled.
- **Hierarchy**: the sub-task parent field, or the Epic Link.
- MCP endpoint: `https://mcp.atlassian.com/v1/sse` (via `mcp-remote`).

## Linear — Linear MCP
- **Status** is a first-class **workflow state** — set it directly (no transition
  graph, unlike Jira) — but states are **per-team**; resolve the state id in the
  issue's team.
- **Change lives in the linked GitHub/GitLab repo**: name the branch
  `<identifier>-<slug>` (e.g. `eng-123-…`) and Linear auto-links it and advances state
  on merge — the binding is the branch name, not a body keyword.
- **Hierarchy**: `parentId` for sub-issues; projects/initiatives above.
- MCP endpoint: `https://mcp.linear.app/sse`.

## Other / custom (any API/CLI/MCP)
No shipped notes. At establishment capture the same four — status mechanism,
child→parent link, where changes live, endpoint — into the client `AGENTS.md`
"Tracker notes". If the tracker has only a REST/GraphQL API, the agent drives it
directly; record auth and the base URL.
