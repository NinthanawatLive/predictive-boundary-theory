# PBT-LLM v6.1 — Scale-Up on Llama 3.1 8B

**Status:** ✅ Scale-up confirmed — Jailbreak 100%, XSTest 98%
**Model:** Llama 3.1 8B Instruct + PBT adapter (0.29% of parameters)
**New in v6.1:** Module E added (Bayesian prediction error, 9 precision scalars)

## What this experiment does
Scales PBT from TinyLlama 1.1B (Phase 1–3) to **Llama 3.1 8B**.
Adds Module E (prediction error extraction) anchored at layer 15 with
precision scalars `Π_l` across layers 16–31.

## Architecture change (v6.0 → v6.1)
| Component | v6.0 | v6.1 |
|---|---|---|
| Module E | Not implemented | ε_l = Π_l·(h(l)−h(l−1)), 9 precision scalars |
| Module V input | h(l) absolute state | ε_l prediction error |
| Module M | EMA | EMA (unchanged) |
| New params | 0 | 9 (log_precision) |

## Key results
| Metric | v5.9 | v6.0 | **v6.1** |
|---|---|---|---|
| Overall Val Acc | 74.7% | 73.6% | 73.4% |
| XSTest | 49/50 (98%) | 50/50 (100%) | **49/50 (98%)** |
| **Jailbreak** | 98.6% | 98.6% | **100.0% ★** |
| HH-RLHF | 55.7% | 50.6% | 50.9% |
| BeaverTails | — | — | 76.9% |
| Composite | — | — | **0.8743** |

## Learned precision Π_l (Module E)
All 9 layers converge to Π_l ≈ 1.003–1.007 — confirming that prediction error
is being selectively weighted across the Evaluation and Decision zones.

## Learned decay rates (Module M)
```
γ_pain     = 0.0803   (init 0.08) — pain memory most persistent
γ_pleasure = 0.1280   (init 0.13)
γ_epistemic = 0.2195  (init 0.22) — epistemic decays fastest
Ordering: γ_pain < γ_pleasure < γ_epistemic  ✓  (matches PBT theory)
```

## Files
| File | Description |
|---|---|
| `PBT_LLM_v6_1_Llama31_8B.ipynb` | Main experiment (renamed from `Test_6_1_Edit2.ipynb`) |
| `pbt_hybrid_v61_llama31_8b.pth` | Trained adapter weights (~full precision) |
| `PBT_Summary_v61.md` | Auto-generated experiment summary |
| `pbt_temporal_0.png` | Temporal analysis plot |
| `pbt_temporal_1.png` | Temporal analysis plot |
| `ผลการทดลอง LLM` | Thai-language result reports (earlier versions) |
