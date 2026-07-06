# Changelog

## 0.4.0 — 2026-07-06

### Changed (breaking)
- Artifacts now live in `.pm-agent/<feature-slug>/` — one directory per feature — instead of a shared `.pm-agent/` root. A second feature in the same repo no longer overwrites the first. Existing flat `.pm-agent/` layouts are not migrated automatically; move them into a slug directory to resume them.
- RICE scoring replaced by rule-based sequencing: dependencies first, then risk, then value density. RICE required portfolio-level reach data the agent had to invent, producing rankings that looked objective but weren't. Every remaining estimate is labeled `[interview]` or `[assumption]`, and assumptions flow into the PRD's Open Questions.

### Added
- Phase 0 (Intake): feature slug, progress inference from existing artifacts, resume support, and `status` / `resume <feature>` command arguments.
- Every PRD section now has a named source artifact; inventing content at assembly time is banned. After assembly, `prd.md` is the single source of truth and `sections/` files are frozen.
- Interview now collects Evidence (who/how many/how often) and a Success metric, and is asked in rounds of 3-4 questions instead of a single wall.
- Validation ends in an explicit verdict: PASS / PASS WITH CORRECTIONS / BLOCKED, with defined behavior for each.
- Delivery detects available targets (git repo, GitHub remote, `gh` auth) and offers file/branch fallbacks instead of failing silently.
- Stage C adversarial verification capped at three rounds per FR, then escalates to the user.
- Stage D runs the project's lint/typecheck/build gates (recorded during Phase 1a) alongside the full test suite; degrades to a local branch when there is no GitHub remote.
- NFRs must cite their source (discovery fact, stack constraint, or explicit requirement); non-applicable categories are omitted instead of padded.
- README, LICENSE (MIT), and this changelog.

## 0.3.0 and earlier
Predate this changelog: initial discovery→delivery pipeline, greenfield mode, opt-in epic-by-epic implementation.
