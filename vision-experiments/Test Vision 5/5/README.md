# PBT-Vision v5 — ★ BEST: LSTM Module M (BRT-Inspired)

**Status:** ★ BEST overall result — 98.1% validation accuracy
**Model:** DINOv2-Base + A-E-V-M(LSTM)-W
**Inspiration:** Block-Recurrent Transformers (Hutchins et al., NeurIPS 2022)
**Dataset:** CIFAR-10 (hard sequences)

## What this experiment does
Upgrades Module M from simple EMA to an **LSTM-style gated accumulator**
with **differential β bias per valence dimension**:

```
V_acc_k = V_acc_k · f_k + z_k · i_k
f_k = σ(W_f · v + b_f + β_k)   ← β_k is learnable per valence
```

This allows each valence dimension (pain/pleasure/epistemic) to have its own
forget rate, rather than a shared exponential decay.

## Key results
| Metric | Score |
|---|---|
| **Overall Val Acc** | **98.1%** ★ BEST |
| Context (road+frog) | 100% |
| V_epistemic at Frame 7 | −1.01 (first signed observation) |

## Ablation
| Config | Val Acc | vs V-only (+pp) |
|---|---|---|
| V only | 81.7% | — |
| +E+M(LSTM) | 97.2% | +15.5 pp |
| **Full A-E-V-M-W** | **98.1%** | **+16.4 pp** |

## Learned β (equal initialisation [1,1,1])
```
β_pain     = 4.35   → retain 99.96% per step
β_epistemic = 3.70  → retain 97.5% per step
β_pleasure  = 0.50  → retain 62.2% per step (effectively discarded)
```
The model **spontaneously discovers** pain > epistemic >> pleasure — confirming the
PBT Dual-Weight ordering without it being hard-coded.

## Files
| File | Description |
|---|---|
| `PBT_Vision_v5_LSTM_module_M.ipynb` | Main experiment (renamed from `สำเนาของ_PBT_Vision_v5_A100.ipynb`) |
| `PBT_Vision_v5_LSTM_A100.py` | Python script version |
| `pbt_vision_v5.pth` | Trained adapter weights |
| `pbtv5_training.png` | Training curve |
| `ผลทดสอบ PBT_Vision_v5_A100.docx` | Thai-language result report |

## Sub-versions
- [v5.1](../5.1/README.md) — β ordering under 3 init conditions
- [v5.2](../5.2/README.md) — ★ Bidirectional Valence (V ∈ ℝ³) discovery
