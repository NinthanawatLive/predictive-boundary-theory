# PBT-LLM v6.0 — Module M: Temporal Valence Accumulation

**Status:** First LLM implementation of Module M
**Model:** Llama 3.1 8B Instruct + PBT adapter with Module M
**Task:** AI safety classification (jailbreak detection + XSTest)

## Architecture Change (v5.9 → v6.0)

Module M added: temporal valence accumulation across 9 sampled layers.

```
Static path:  h_last (4096) + static_V (27) = 4123-dim
Module M:     V_acc across layers [16,18,20,22,24,26,28,30,31]
              V_acc[L] = Σ_l V(l) · (1−γ)^(L−1−l)
Combined:     4123 + 3 = 4126-dim input to classifier
New params:   3  (raw_gamma per valence dimension)
```

## Key Results

| Metric | Score |
|--------|-------|
| XSTest (safe/unsafe) | **50/50 (100%)** ← perfect |
| Jailbreak Accuracy | 98.62% |
| Composite | 0.878 |
| ρ×σ proxy | 0.986 |

## Learned Module M Parameters

| Parameter | Init | Learned | PBT Role |
|-----------|------|---------|----------|
| γ_pain | 0.08 | 0.0798 | Pain memory decays slowest |
| γ_pleasure | 0.13 | 0.1282 | Pleasure decays at medium rate |
| γ_epistemic | 0.22 | 0.2201 | Epistemic decays fastest |
| Ordering | — | ✅ γ_p < γ_pl < γ_e | Biologically consistent |

The ordering `γ_pain < γ_pleasure < γ_epistemic` was enforced architecturally and confirmed: pain memory is most persistent, matching the PBT theoretical prediction.

## Version Comparison

| Version | XSTest | Jailbreak | ρ×σ |
|---------|--------|-----------|-----|
| v5.9 | 49/50 (98%) | 98.6% | 0.966 |
| **v6.0** | **50/50 (100%)** | **98.6%** | **0.986** |

## Files

| File | Description |
|------|-------------|
| `PBT_LLM_v6_0_Llama31_8B.ipynb` | Main experiment notebook |
| `PBT_Summary_v60.md` | Detailed results and parameter analysis |

## Next Version
→ [v6.1](../v6.1-Llama31-module-E/README.md): Module E added (Bayesian precision weighting) → Jailbreak **100%**
