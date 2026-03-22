# PBT-Vision v2 — Modules E and M Awaken

**Status:** Modules E+M present but not yet necessary (static environment)
**Model:** DINOv2-Base + Module E (precision-weighted) + Module M (EMA)
**Dataset:** CIFAR-10 (easy sequences)

## What this experiment does
First implementation of the full E+M pipeline on top of Module V.
Module E computes precision-weighted prediction error (ΔS).
Module M accumulates valence via exponential moving average (EMA).

## Key result
| Config | Val Acc |
|---|---|
| V only | 99.8% |
| Full (E+V+M) | 99.8% |
| **E+M impact** | **+0.0 pp** |

- E and M are dormant in a static/easy environment — they add no value when context is trivial
- This is a **predicted result** under PBT: modules sleep when not needed
- Confirms the Bandwidth Conservation Law: `C = σ·τ·ρ ≈ const`

## Files
| File | Description |
|---|---|
| `PBT_Vision_v2_E_M_awaken.ipynb` | Main experiment notebook (renamed from `สำเนาของ_PBT_Vision_2.ipynb`) |
| `PBT_Vision_v2_A100.py` | Python script version (for Colab A100) |
| `pbt_vision_v2.pth` | Trained adapter weights |
| `ผลการทดสอบ PBT Vision 2.docx` | Thai-language result report |

## Next version
→ [Vision v3](../Test%20Vision%203/README.md): ★ BREAKTHROUGH — hard environment, E+M become necessary
