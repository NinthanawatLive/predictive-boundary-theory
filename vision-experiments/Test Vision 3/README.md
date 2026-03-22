# PBT-Vision v3 — ★ BREAKTHROUGH: E and M Become Necessary

**Status:** ★ BREAKTHROUGH — first proof that E+M are structurally necessary
**Model:** DINOv2-Base + Module E (Π_max=59.99) + Module M (EMA, γ_pain=0.0003)
**Dataset:** CIFAR-10 (hard sequences: sudden anomaly, gradual drift, context-dependent)

## What this experiment does
Switches to a **hard, dynamic environment** where context matters.
Introduces gradual drift and context-dependent sequences (same object, different meaning).

## Key results

### Ablation by task type
| Task type | V only | Full (E+V+M) | E+M gain |
|---|---|---|---|
| Normal sequences | 100% | 99.7% | -0.3 pp |
| Sudden anomaly | 94.0% | 99.5% | +5.5 pp |
| Gradual drift | 78.5% | 94.9% | +16.3 pp |
| **Context road+frog** | **54.0%** | **96.0%** | **+41.9 pp** ★ |
| **Overall** | **89.6%** | **97.2%** | **+7.6 pp** |

### ★ Smoking gun: same frog, opposite meaning
```
Road context  → frog at Frame 4: UNSAFE=1.000, V_acc_pain=2.93
Nature context → frog at Frame 4: UNSAFE=0.000, V_acc_pain=0.74
```
Identical feature vector. Opposite output. Context (Module M memory) is the only difference.

### Module parameters
- `Π_max = 59.99` (×20 relative to v2) — Module E locks on to prediction error
- `γ_pain = 0.0003` — pain memory decays at only 0.03% per step (near-permanent)

## Files
| File | Description |
|---|---|
| `PBT_Vision_v3_breakthrough.ipynb` | Main experiment notebook (renamed from `PBT_Vision_v3_A100.ipynb`) |
| `PBT_Vision_v3_A100.py` | Python script version |
| `pbt_vision_v3.pth` | Trained adapter weights |
| `ผลทดสอบ PBT_Vision_v3_A100.docx` | Thai-language result report |

## Next version
→ [Vision v4.1](../Test%20Vision%204/README.md): Full A-E-V-M-W loop (Module W added)
