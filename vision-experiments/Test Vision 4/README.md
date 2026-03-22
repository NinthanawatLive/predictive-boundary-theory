# PBT-Vision v4.1 — Full A-E-V-M-W Loop

**Status:** ✅ SUCCESS — first complete 5-module loop proven
**Model:** DINOv2-Base + full A-E-V-M-W loop (from scratch)
**Dataset:** CIFAR-10 (hard sequences, same as v3)

## What this experiment does
Adds **Module W** (World Model / predictive prior) to complete the full A-E-V-M-W cycle.
v4 (warm-start) FAILED — modules must co-evolve from scratch, cannot be transplanted.
v4.1 trains the full loop from scratch and succeeds.

## Key lesson: "Cannot transplant a brain — must co-evolve"
v4 attempted to add Module W to a pre-trained v3 adapter (warm-start).
Result: catastrophic interference — the pre-trained representations were incompatible
with a new generative prior. v4.1 restarts from scratch and all 5 modules learn together.

## Key results (v4.1 from-scratch)
| Config | Val Acc | vs V-only (+pp) |
|---|---|---|
| V only | 81.4% | — |
| +E | 87.3% | +5.9 pp |
| +M | 88.4% | +7.0 pp |
| +A | 94.5% | +13.1 pp |
| +W | 95.4% | +14.0 pp ★ |
| **Full A-E-V-M-W** | **95.4%** | **+14.0 pp** |

### Learned R weights (Module A)
```
R_weights = [2.15 (pain), 1.25 (pleasure), 2.10 (epistemic)]
pain ≈ epistemic >> pleasure
```
→ survival task: pleasure is not required, confirmed Stage 1–3 system

### Gate dynamics
- Gate 0.54 → 0.006 as R rises: bandwidth conservation in action

## Files
| File | Description |
|---|---|
| `PBT_Vision_v4_1_full_AEVMW_loop.ipynb` | Main experiment notebook (renamed from `PBT_Vision_v4_1_A100.ipynb`) |
| `PBT_Vision_v4_AEVMW_A100.py` | Python script version |
| `pbt_vision_v4_1.pth` | Trained adapter weights |
| `pbtv4_training.png` | Training curve |
| `ผลทดสอบ PBT_Vision_v4_1_A100.docx` | Thai-language result report |

## Next version
→ [Vision v5](../Test%20Vision%205/5/README.md): LSTM Module M (BRT-inspired)
