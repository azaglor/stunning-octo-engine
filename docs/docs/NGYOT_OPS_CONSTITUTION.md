# NGYOT Plugin — Operational Constitution (for implementation)

Goal: implement NGYOT_actual scoring + orchestration helpers in a way that is:
- deterministic
- auditable (component breakdowns)
- domain-agnostic at the interface
- robust against “phantom” drift (pretty outputs without functional structure)

## Required artifacts for any non-trivial change
- A written plan (ExecPlan) with:
  - phases
  - acceptance tests (what proves it works)
  - what is explicitly out-of-scope
- Unit tests for math and invariants
- A small example dataset + expected outputs (goldens)

## Implementation invariants
1) Every computed score must return:
   - scalar value
   - breakdown of terms (A, ALF*, CF, E, Φ, γ, Ls, Ld, ξ, Ch, D_ext)
   - warnings when inputs are missing / low-confidence
2) No hidden heuristics:
   - if a proxy is used, it must be named + documented + configurable
3) No “analogy-based logic”:
   - code paths must map to explicit NGYOT terms (or be clearly labeled experimental)

## Data model (minimum viable)
- ActionRecord:
  - id, timestamp, domain(optional), description
  - A, ALF, CF, beta
  - E, Phi, gamma
  - Ls, Ld, xi
  - V, L, Vstar (or D_ext directly)
  - FlipCount, Load, Recovery (if updating churn)
- State:
  - Ch
  - kappa, lambda, alpha, eta, rho defaults

## Scope guidance (MVP)
MVP should implement:
- ALF_star(ALF, CF, beta)
- rcy_raw(action)
- churn_update(state, proxies)
- rcy_eff(rcy_raw, Ch, kappa)
- ngyot_actual(action/state or window aggregate)

Defer:
- fancy ML estimation of A/CF/etc
- multi-agent scheduling
- UI
