# PM Agent

A Claude Code plugin that turns "we should add X" into a validated PRD — and, if you opt in, into working code, one epic at a time.

It behaves like a senior Product Manager: it interviews you before writing anything, grounds every requirement in your actual codebase, refuses to invent prioritization data, and only implements when you explicitly ask. Every phase writes its output to disk, so an interrupted run resumes exactly where it stopped.

## Install

As a Claude Code plugin (recommended — gives you the `/pm-agent` command):

```
/plugin marketplace add facukoren/pm-agent
/plugin install pm-agent@pm-agent
```

Or as a portable skill via the universal [`skills`](https://www.skills.sh) installer (works across Claude Code, Codex, Cline, and other agents):

```
npx skills add facukoren/pm-agent
```

## Usage

```
/pm-agent add CSV export to the reports dashboard   # full pipeline from an idea
/pm-agent --fast <idea>       # skip the interview; assumptions logged as Open Questions
/pm-agent implement <epic>    # jump to implementation of an already-validated epic
/pm-agent status              # where every feature in this repo stands
/pm-agent resume <feature>    # pick up an interrupted feature at its first incomplete phase
```

Works on existing codebases and on empty repos (greenfield mode — it will build the app from scratch, walking skeleton first).

## The pipeline

| Phase | What happens | Output |
|---|---|---|
| 0. Intake | Detect existing features, resume or start fresh, pick a feature slug | — |
| 1. Discovery | Codebase exploration (subagent) + structured user interview | `codebase-analysis.md`, `discovery.md` |
| 2. Solution Design | 2-3 approaches with tradeoffs, module and interface design | `solution-design.md` |
| 3. PRD | FR/NFR with testable acceptance criteria, epics and stories, dependency- and risk-aware roadmap | `prd.md` |
| 4. Validation | A subagent checks the PRD against the real code; verdict: PASS / PASS WITH CORRECTIONS / BLOCKED | `validation.md` |
| 5. Delivery | GitHub issue(s), or the PRD as a file if there's no remote | `delivery.md` |
| 6. Implementation (opt-in) | Per epic: failing tests first → parallel implementation by module → adversarial verification (3-round cap) → draft PR | `implementation/{epic}/` |

Checkpoints between every phase — the agent never runs ahead of your buy-in, and never implements unprompted.

## Design choices worth knowing

- **No RICE scoring.** RICE needs portfolio-level reach data an agent doesn't have; a formula over invented inputs produces rankings that look objective but aren't. Epics are ordered by dependencies, then risk, then value-per-effort — and every estimate is labeled `[interview]` (you said it) or `[assumption]` (the agent inferred it, and it lands in Open Questions).
- **Every PRD section has a named source artifact.** Nothing is invented at assembly time; if the material is missing, the agent goes back and gets it.
- **Adversarial verification.** Implemented code is checked by independent verifier agents prompted to find failures (criteria, regression, consistency lenses) — implementer self-reports are never trusted.
- **One directory per feature.** `.pm-agent/<feature-slug>/` — specifying a second feature never overwrites the first, and progress is inferred from which artifacts exist.

## Artifacts

```
.pm-agent/
  <feature-slug>/
    codebase-analysis.md
    discovery.md
    solution-design.md
    sections/                 # PRD build artifacts, frozen after assembly
    prd.md                    # single source of truth
    validation.md
    delivery.md
    implementation/{epic}/    # test-map.md + report.md per epic
```

On first run the agent asks once whether `.pm-agent/` should be committed or added to `.gitignore`.

## Requirements

- [Claude Code](https://claude.com/claude-code)
- `gh` CLI authenticated (optional — delivery and PRs degrade gracefully to files and local branches without it)

## License

[MIT](LICENSE)
