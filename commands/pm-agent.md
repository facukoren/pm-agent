---
description: Senior PM Agent — discovery, PRD, dependency- and risk-aware roadmap, codebase validation, optional epic-by-epic implementation
argument-hint: [your idea | "implement <epic>" | "status" | "resume <feature>"]
---

Invoke the `pm-workflow` skill to handle this request:

$ARGUMENTS

Routing:
- `implement <epic>` → jump to Phase 6 (Implementation) — verify its preconditions first
- `status` → run Phase 0 only: report every feature's progress and stop
- `resume <feature>` → continue that feature at its first incomplete phase
- anything else → start at Phase 0 with the request as the feature idea
