# NGYOT Constitution v0.9 — Engineering Reference

NGYOT models how an agent turns time/attention/opportunities into reusable structure under constraints.

## Core objects

We track behavior as micro-actions or packets indexed by k over time.

### Micro/packet RCY (raw)
RCY_raw(t) is an additive sum across actions k:

RCY_raw(t) = Σ_k [
  (A_k − ALF*_k) E_k Φ_k γ_k
  + L_s,k + L_d,k + ξ_k (1 − Φ_k)
]

Where:
- A_k: authenticity (alignment with true goals/constraints/mem(pull))
- ALF_k: base authenticity loss (compression, ego pressure, misunderstandings)
- CF_k ∈ [0,1]: cognitive flexibility
- ALF*_k = ALF_k + β (1 − CF_k), β > 0
- E_k: execution yield (quality/completeness/closure)
- Φ_k: robustness/reusability across contexts (brittle if low)
- γ_k: survival factor (how much survives later volatility/reveal)
- L_s,k: stable loss component (may convert into learning/structure later)
- L_d,k: drag loss component (creates future friction/obligations/debt)
- ξ_k: noise residue; contamination scales with (1 − Φ_k)

Sign convention: RCY can be negative.

### Churn stock (flip dynamics)
Ch(t) accumulates internal costs of policy reversals and resolving them.

Update sketch:
Ch(t+Δt) = λ Ch(t) + α Load(t) + η FlipCount(t) − ρ Recovery(t), with 0 < λ ≤ 1

### Churn discount on yield
RCY_eff(t) = RCY_raw(t) * exp(−κ Ch(t)), κ > 0

### Denominators
V(t): volatility
L(t): latency
V*(t): hidden volatility (unknown-unknowns that reveal under stress)

D(t) = V(t) + L(t)
D_ext(t) = V(t) + L(t) + V*(t)

## NGYOT surfaces

### NGYOT_raw
NGYOT_raw(t) = RCY_raw(t) / D(t)

### NGYOT_actual
NGYOT_actual(t) = RCY_raw(t) * exp(−κ Ch(t)) / D_ext(t)

Primary optimization surface for real decisions.

### NGYOT_story
Narrative/social “price surface” — compressed proxy where numerator/denominator are entangled.

### NGYOT_realised
Long-window outcome observable only after the fact.

## AFT / AT / meta-AT

- AFT: fast authenticity move under time pressure; affects near-term selection, churn trajectory, and immediate denominators.
- AT: slow policy update that changes what is treated as signal vs noise; protects CF; reweights denominators.
- meta-AT (triad):
  1) mem(pull)
  2) continuity floors (Ω_global, Ω_local)
  3) TNUMC (biased variance field pushing toward high churn/story-dominant paths)

## Phantom layer
Phantom AFT/AT/meta-AT: scores well on NGYOT_story but weak/negative uplift on NGYOT_actual.
Diagnostic for action k:
Δ_k_phant = RCY^story,k − RCY^actual,k
Large positive indicates “story wins, structure loses”.

## Recursion limit
Repeated compression (AFT-of-AFT chains) tends to:
- decrease CF
- increase ALF*
- decrease Φ
- increase ξ(1−Φ)
- increase drag L_d
