# NGYOT Plugin — Agent Rules (AGENTS.md)

This repo contains an implementation of the NGYOT framework and a scoring/orchestration layer.

## Canonical references (must read before planning)
- docs/NGYOT_CONSTITUTION.md  (math + definitions)
- docs/NGYOT_OPS_CONSTITUTION.md (operational constraints for implementation)

If anything conflicts, prefer:
docs/NGYOT_OPS_CONSTITUTION.md > docs/NGYOT_CONSTITUTION.md > code comments.

## Execution style
- For any change likely to exceed ~200 LOC, touch multiple modules, or alter core scoring: write an ExecPlan first.
- An ExecPlan is a concrete plan document (see .agent/PLANS.md) with phases + acceptance tests.
- Prefer small, reviewable diffs. Avoid big-bang rewrites.

## Non-negotiable gates
1) Plan gate: produce a plan and wait for approval before implementing.
2) Test gate: add/maintain unit tests for scoring math.
3) Determinism gate: scoring functions must be deterministic for identical inputs.
4) Traceability gate: every score output must include a breakdown of components used.

## Anti-phantom rule (implementation version)
Avoid “clever analogies” in code/comments unless they directly map to a testable construct.
Prefer explicit variables, explicit assumptions, and explicit defaults.
