<p align="center">
  <img src="assets/logo.svg" alt="keelson — guardrails against AI agent spec-drift" width="560">
</p>

**AI agents drift.** They start on-spec, then quietly make a call that touches the original design, plan, or strategy — and you catch it at review, after merge, or once it has compounded across three more changes. keelson draws a hard line between the mechanical moves an agent should just make and the consequential ones it must hand back to you, so spec-drift surfaces the moment it happens — not three steps later.

> Port a disciplined, issue-driven agentic flow into **any repo**, against **any tracker**.

keelson is a pair of [Agent Skills](https://www.agensi.io/learn/agent-skills-open-standard) — **`adopt-keelson`**, which interviews you and writes a tracker-agnostic operating model into your repo (a questionnaire-organized `AGENTS.md` plus a *selectable* lifecycle skill-pack), and **`tune-gates`**, which establishes or refines just the quality gates, standalone.

It distills the flow a production repo uses to let AI agents ship safely:

```mermaid
%%{init: {"flowchart": {"diagramPadding": 22, "nodeSpacing": 40, "rankSpacing": 36}}}%%
flowchart TD
  S([enter · a change request<br/>or the next backlog item]) --> F
  F[frame] --> P[plan] --> I[implement] --> O[open change] --> G{{gate}} --> C[close]
  C --> D([done · merged, status set,<br/>handed back])
  D -. you start the next .-> S
  P -. strategic call .-> H[/escalate to human/]
  G -. structural drift .-> H
  H -. redirect .-> P
  classDef step fill:#e8eef7,stroke:#33415c,color:#0b1324,stroke-width:1px;
  classDef enter fill:#dfeee4,stroke:#2f6b4f,color:#0b1f16,stroke-width:1.5px;
  classDef done fill:#d7e3f4,stroke:#2c4a73,color:#0b1324,stroke-width:1.5px;
  classDef gate fill:#d6ece6,stroke:#2f6b53,color:#0b1f16,stroke-width:1.5px;
  classDef human fill:#f7e9d0,stroke:#8a5a16,color:#241a0b,stroke-width:1.5px;
  class F,P,I,O,C step;
  class S enter;
  class D done;
  class G gate;
  class H human;
  linkStyle 7 stroke:#2f6b4f,stroke-width:1.5px,stroke-dasharray:4 3;
  linkStyle 8,9,10 stroke:#8a5a16,stroke-width:1.5px,stroke-dasharray:4 3;
```

## Tactical autonomy, human-in-the-loop on drift

**Tactical autonomy** — the agent just does the mechanical moves (claim the issue, branch, code + doc + test, open the change, move status). **Strategic human loop** — it escalates the moment a move touches the original design, plan, or strategy. That balance is the whole point: momentum on the mechanical, a human gate on the consequential.

```mermaid
%%{init: {"flowchart": {"diagramPadding": 22, "nodeSpacing": 48, "rankSpacing": 40}}}%%
flowchart TD
  M([a move in the loop]) --> Q{touches the original<br/>design, plan, or strategy?}
  Q -- no --> A[agent just does it<br/>· tactical autonomy ·]
  Q -- yes --> H[escalate to the human<br/>· strategic loop ·]
  classDef start fill:#eef1f6,stroke:#33415c,color:#0b1324,stroke-width:1px;
  classDef decision fill:#ece8fb,stroke:#5b4b8a,color:#1e1633,stroke-width:1.5px;
  classDef agent fill:#e8eef7,stroke:#33415c,color:#0b1324,stroke-width:1.5px;
  classDef human fill:#f7e9d0,stroke:#8a5a16,color:#241a0b,stroke-width:1.5px;
  class M start;
  class Q decision;
  class A agent;
  class H human;
  linkStyle 0 stroke:#33415c,stroke-width:1.5px;
  linkStyle 1 stroke:#33415c,stroke-width:1.5px;
  linkStyle 2 stroke:#8a5a16,stroke-width:1.5px;
```

It's **provider-neutral**. The skills name actions — *"create the issue", "open the change", "set status to `<column>`"* — and your agent runs whatever your tracker uses: GitHub, GitLab, Jira, Linear, or anything with an API/CLI/MCP. The only repo-specific values are filled from your interview answers.

## What you get

`adopt-keelson` writes two things into your repo:

1. **`AGENTS.md`** — the substrate every skill defers to: your vocabulary, branch/change rules, status columns, the InnoVestrum engineering standards, and the chosen tracker's traps folded in under *Tracker notes*.
2. **A lifecycle pack** — the ideas you pick (below), written as your repo's own skills in plain language.

Thin templates plus your interview answers become your repo's own artifacts:

```mermaid
%%{init: {"flowchart": {"diagramPadding": 22, "nodeSpacing": 40, "rankSpacing": 44}}}%%
flowchart TD
  T[keelson templates<br/>AGENTS substrate · 17 lifecycle · tracker-notes]
  A[/your answers ·<br/>one round per idea/]
  T --> E(((adopt-keelson)))
  A --> E
  E --> G[your AGENTS.md<br/>substrate + standards + tracker notes]
  E --> L[your selected skills<br/>plain-language, defer to AGENTS.md]
  classDef tmpl fill:#ece8fb,stroke:#5b4b8a,color:#1e1633,stroke-width:1.5px;
  classDef ans fill:#f7e9d0,stroke:#8a5a16,color:#241a0b,stroke-width:1.5px;
  classDef act fill:#d6ece6,stroke:#2f6b53,color:#0b1f16,stroke-width:1.5px;
  classDef out fill:#e8eef7,stroke:#33415c,color:#0b1324,stroke-width:1.5px;
  class T tmpl;
  class A ans;
  class E act;
  class G,L out;
  linkStyle 0,1 stroke:#5b4b8a,stroke-width:1.5px;
  linkStyle 2,3 stroke:#33415c,stroke-width:1.5px;
```

## Install

### Claude Code (marketplace plugin)

```sh
/plugin marketplace add innovestrum/keelson
/plugin install keelson
```

Then run `/keelson:adopt-keelson` (or `/keelson:tune-gates` to set up just the gates).

### Codex / any SKILL.md-aware agent (Agent Skill)

The skills are discovered under [`.agents/skills/`](https://developers.openai.com/codex/skills). Vendor them once at user level so they're available in every repo:

```sh
git clone https://github.com/innovestrum/keelson
ln -s "$PWD/keelson/skills/adopt-keelson" ~/.agents/skills/adopt-keelson   # Codex follows symlinks
ln -s "$PWD/keelson/skills/tune-gates"    ~/.agents/skills/tune-gates      # each folder symlinks the shared templates/
```

Then run `$adopt-keelson` (or just ask: *"establish the agentic workflow"*). The same path works for Gemini CLI, Copilot, Cursor, and other SKILL.md adopters.

## Use

Run the establisher and work through the interview — a **deliberate, multi-round** session (≈8–20 rounds) that first maps your SDLC, then makes *you* decide each pillar: vocabulary, where plans and docs live, the tactical-vs-strategic line, which gates apply. It then writes `AGENTS.md` and the skills you selected. **Read the new `AGENTS.md` first**; it's the briefing everything else defers to.

```mermaid
%%{init: {"flowchart": {"diagramPadding": 22, "nodeSpacing": 40, "rankSpacing": 38}}}%%
flowchart TD
  O([orient · tracker, tools,<br/>existing AGENTS.md]) --> Iv{{interview}}
  Iv -. ≈8–20 rounds · you decide .-> Iv
  Iv --> Fl[fill templates ·<br/>substitute your conventions]
  Fl --> W[write into your repo ·<br/>AGENTS.md + selected skills]
  W --> V[verify · no blanks left,<br/>skills match the substrate]
  V --> R([recap · adopted vs skipped,<br/>read AGENTS.md first])
  classDef step fill:#e8eef7,stroke:#33415c,color:#0b1324,stroke-width:1px;
  classDef terminal fill:#d7e3f4,stroke:#2c4a73,color:#0b1324,stroke-width:1.5px;
  classDef gate fill:#d6ece6,stroke:#2f6b53,color:#0b1f16,stroke-width:1.5px;
  class Fl,W,V step;
  class O,R terminal;
  class Iv gate;
  linkStyle 1 stroke:#8a5a16,stroke-width:1.5px,stroke-dasharray:4 3;
```

## The 17 ideas (selectable)

| | Core loop *(default on)* | Discipline *(default on)* | Milestone & gates *(by repo shape)* |
|---|---|---|---|
| | `frame-the-change` | `respond-to-uncertainty` | `close-a-milestone` |
| | `pick-next-work` | `carve-out-followup` | `review-gate` |
| | `plan-the-work` | `check-prior-art` | `source-change-gate` |
| | `implement-the-change` | `automate-or-runbook` | `source-sync-gate` |
| | `open-the-change` | | `parity-gate` |
| | `track-status` | | `sync-docs` |
| | | | `update-source-of-truth` |

A backend-only repo with one deploy target might adopt the first ten plus `sync-docs`; a multi-platform app with a design source adopts the gates too. Pick what fits.

The four gates — `review-gate` (spec → implementation), `source-change-gate` (the source itself changed), `source-sync-gate` (source ↔ implementation), `parity-gate` (target ↔ target) — are set up, first time or later, by the standalone **`tune-gates`** skill; `adopt-keelson` runs it for you during the interview, or invoke it on its own to add or tune a gate.

## Quality gates — the `tune-gates` skill

A second skill that sets up **or refines** your quality gates on their own — the CI checks that block merge, plus the verification gates (`review` · `source-change` · `source-sync` · `parity`) — without re-running the whole `adopt-keelson` interview.

```mermaid
%%{init: {"flowchart": {"diagramPadding": 18, "nodeSpacing": 36, "rankSpacing": 34}}}%%
flowchart LR
  A([tune-gates]) --> P[pick CI checks<br/>+ which gates fit] --> W[write / patch<br/>AGENTS.md + gate skills] --> D([gates live])
  D -. refine later .-> A
  classDef enter fill:#dfeee4,stroke:#2f6b4f,color:#0b1f16,stroke-width:1.5px;
  classDef step fill:#e8eef7,stroke:#33415c,color:#0b1324,stroke-width:1px;
  classDef done fill:#d7e3f4,stroke:#2c4a73,color:#0b1324,stroke-width:1.5px;
  class A enter;
  class P,W step;
  class D done;
  linkStyle 3 stroke:#2f6b4f,stroke-width:1.5px,stroke-dasharray:4 3;
```

**Use it:** `/keelson:tune-gates` (or *"add a gate"* / *"tune the quality gates"*) on a repo that already adopted keelson — it reads your `AGENTS.md` and changes only the delta. `adopt-keelson` also runs it for you during first-time setup.

## Layout

```
keelson/
├── templates/                     shared by both skills (symlinked into each)
│   ├── AGENTS.client.md           the AGENTS.md substrate the skills write
│   └── lifecycle/*.md             the 17 selectable skill templates
├── references/tracker-notes.md    per-tracker traps, folded into the client AGENTS.md
├── skills/
│   ├── adopt-keelson/             the establisher (interview → fill → write)
│   │   ├── SKILL.md
│   │   ├── agents/openai.yaml     Codex metadata
│   │   └── templates → ../../templates · references → ../../references
│   └── tune-gates/                establish/refine the quality gates, standalone
│       ├── SKILL.md
│       ├── agents/openai.yaml
│       └── templates → ../../templates · references → ../../references
├── .agents/skills/{adopt-keelson,tune-gates} → ../../skills/…   (Codex discovery)
├── .claude-plugin/                Claude marketplace + plugin manifests
└── AGENTS.md  README.md  LICENSE
```

The lifecycle templates and tracker notes live **once** at the plugin root; each skill folder symlinks them in, so a skill reads `templates/…` as if local while the source keeps a single copy.

Two skills, **one physical copy** of the shared templates — Claude reads the plugin directly, Codex through per-skill symlinks:

```mermaid
%%{init: {"flowchart": {"diagramPadding": 22, "nodeSpacing": 44, "rankSpacing": 42}}}%%
flowchart TD
  CC[Claude Code · marketplace plugin] --> S
  CX[Codex · any SKILL.md agent] -. per-skill symlinks .-> S
  subgraph S [skills/]
    direction LR
    AK[adopt-keelson]
    TG[tune-gates]
  end
  S -. templates symlink .-> T[(templates/ + references/<br/>the one physical copy)]
  classDef tool fill:#e8eef7,stroke:#33415c,color:#0b1324,stroke-width:1.5px;
  classDef skill fill:#d6ece6,stroke:#2f6b53,color:#0b1f16,stroke-width:1.5px;
  classDef real fill:#ece8fb,stroke:#5b4b8a,color:#1e1633,stroke-width:1.5px;
  class CC,CX tool;
  class AK,TG skill;
  class T real;
  style S fill:#eef6f2,stroke:#2f6b53,color:#0b1f16;
```

## The golden rule

Every file here, and every file the establisher writes, obeys it:

> **Never tell the agent how to do what it already knows.** Write only **gaps**, **gotchas**, and **pointers**. Cut generic CLI/API syntax, config formats, and obvious procedure. A senior engineer's terse handover note, not a tutorial.

That rule, and the engineering standards keelson carries into client repos, come from [innovestrum/agent-skills](https://github.com/innovestrum/agent-skills).

## Trackers

GitHub, GitLab, Jira, and Linear ship with their non-obvious traps and bindings in [`references/tracker-notes.md`](references/tracker-notes.md); anything else with an API/CLI/MCP is handled by the *custom* path (capture how status moves, how a child links to a parent, where changes live, and the endpoint).

## License

[MIT](LICENSE) © InnoVestrum
