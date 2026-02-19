# ExecPlans (how we plan big work)

When implementing complex features or refactors, create an ExecPlan before coding.

## ExecPlan format
1) Problem statement (1-2 paragraphs)
2) Constraints (what must remain true)
3) Proposed design (modules, data flow)
4) Phases (each phase < ~1 hour if possible)
5) Acceptance tests (unit tests, goldens, invariants)
6) Risks & rollback plan
7) Out-of-scope

## Repo convention
Store plans under:
- plans/YYYY-MM-DD_short_title.md
