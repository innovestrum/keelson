# Tracker notes

Per-tracker traps, bindings, and endpoints only — the non-obvious things an agent
that already knows `gh`/`glab`/the tracker's MCP would still get wrong. The
establisher folds the **chosen** tracker's section into the client `AGENTS.md`
(`## Tracker notes`); the lifecycle skills name actions in plain language
("create the issue", "set status to <column>") and rely on this section for the
bindings. Generic syntax is deliberately absent — only surprises are here.

Every section answers the same five questions, because the lifecycle reduces to
them: **Status** (how a work-item moves columns) · **Hierarchy** (child→parent
link) · **Queue** (priority + blocked-by, for run-the-milestone) · **Change** (where
the PR/MR lives, how it links back) · **Endpoint** (the API/MCP to drive when there's
no CLI verb). Add a section when you adopt a tracker; keep it to surprises.

## GitHub — `gh` + GitHub MCP
- **Status** lives on a Projects v2 **single-select field**, not on the issue — the
  issue must be on the board first. Set it via the MCP project-item update or
  `gh project item-edit`; don't mirror it to labels.
- **Auto-add** puts a new issue on the board with every field unset, which hides it
  from filtered views — set Status and the queue field in the same step as filing.
- **Sub-issue and `blocked_by` writes** want the **REST database id**, not the GraphQL
  node id (`I_kwDO…` 422s as "not of type integer") — resolve with
  `gh api repos/<o>/<r>/issues/<n> --jq .id` first. Read children from the sub-issue
  API, not `Refs #<n>` text.
- A parent caps at **100 sub-issues, closed ones included**; a full one rejects the
  link. Group a coherent set under an umbrella child — `POST …/sub_issues` with
  `replace_parent=true` moves them.
- **Queue** = a Projects single-select field (e.g. `Priority`: now | next | later), set
  like Status. **Dependencies** = native `blockedBy` (GraphQL `Issue.blockedBy`; write
  `POST …/issues/<n>/dependencies/blocked_by` with the blocker's REST `.id`).
- REST issue creation **silently mints** an unknown label name — copy labels from a real
  sibling, never by guess.
- `gh api graphql --paginate` advances only a cursor variable named **`$endCursor`**
  (with `pageInfo { hasNextPage endCursor }` selected); any other name re-fetches page 1
  forever and drains the hourly quota.
- `gh pr view --json merged` doesn't exist — confirm a merge with `state` (`MERGED`) /
  `mergedAt` before any cleanup.
- gh ≥ 2.99 `pr merge --delete-branch` also removes the branch's linked worktree, but
  warns and skips a dirty or current one — then `git branch -D` fails on the
  still-checked-out branch. Check `git worktree list`, remove it, then delete.
- `gh pr checks <n> --watch --required` exits instantly with "no required checks"
  when the repo defines none — watch without `--required`.
- `gh run watch --exit-status` can exit 0 on a failed run — confirm with
  `gh run view <id> --json conclusion,jobs`.
- MCP endpoint: `https://api.githubcopilot.com/mcp/`.

## GitLab — `glab` + GitLab MCP
- **Board state** is backed by **scoped labels** — the `workflow::<column>` label
  *is* the column; a plain label won't move the card.
- **Change = Merge Request**, not PR. `Closes #<n>` in the MR description auto-closes.
- **Hierarchy**: epic link on Premium; else a `Refs #<n>` line (no sub-issues on Free).
- **Queue**: no priority field — `priority::<value>` labels (mutually exclusive only as
  Premium scoped labels). "Blocked by" links are Premium; on Free, a `Blocked by #<n>`
  body line the coordinator reads.
- MCP: `npx -y @modelcontextprotocol/server-gitlab`, env `GITLAB_PERSONAL_ACCESS_TOKEN`.

## Jira — Atlassian MCP / REST
- **Status is gated by workflow transitions** — you can't write the status field;
  fetch the legal transitions from the current status and apply one (an illegal jump
  is rejected). Transition names ≠ status names.
- **Change lives in a linked VCS** — open/merge there, put the key (`PROJ-123`) in the
  branch and PR title; Smart Commits (`PROJ-123 #close`) advance the issue only if the
  integration is enabled.
- **Hierarchy**: the sub-task parent field, or the Epic Link.
- **Queue**: the built-in Priority field; dependencies are `is blocked by` issue links
  (link-type names are per-instance — read them, don't assume).
- MCP endpoint: `https://mcp.atlassian.com/v1/sse` (via `mcp-remote`).

## Linear — Linear MCP
- **Status** is a first-class **workflow state** — set it directly (no transition
  graph, unlike Jira) — but states are **per-team**; resolve the state id in the
  issue's team.
- **Change lives in the linked GitHub/GitLab repo**: name the branch
  `<identifier>-<slug>` (e.g. `eng-123-…`) and Linear auto-links it and advances state
  on merge — the binding is the branch name, not a body keyword.
- **Hierarchy**: `parentId` for sub-issues; projects/initiatives above.
- **Queue**: native `priority` (0 = none, 1 = urgent … 4 = low — sort 0 last, not first);
  dependencies are `blocked by` issue relations.
- MCP endpoint: `https://mcp.linear.app/sse`.

## Other / custom (any API/CLI/MCP)
No shipped notes. At establishment capture the same five — status mechanism,
child→parent link, queue + dependency mechanism, where changes live, endpoint — into
the client `AGENTS.md` "Tracker notes". If the tracker has only a REST/GraphQL API,
the agent drives it directly; record auth and the base URL.
