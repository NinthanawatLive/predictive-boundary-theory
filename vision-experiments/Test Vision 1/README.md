# PBT-Vision v1 — Baseline

**Status:** Baseline (proof-of-concept)
**Model:** DINOv2-Base + simple valence probe
**Dataset:** CIFAR-10 (basic sequences)

## What this experiment does
First attempt at attaching a PBT valence probe (Module V only) to DINOv2 frozen features.
No Module E, M, or W. Tests whether a raw valence signal can distinguish safe vs. anomalous sequences.

## Key result
- Overall accuracy: ~baseline level
- Modules E and M not yet implemented → system cannot accumulate context
- Confirms: Module V alone is insufficient for dynamic, context-dependent tasks

## Files
| File | Description |
|---|---|
| `PBT_Vision_v1_baseline.ipynb` | Main experiment notebook (renamed from `PBT_Vision_1.ipynb`) |
| `ผลการทดสอบ PBT Test vision 1.docx` | Thai-language result report |

## Next version
→ [Vision v2](../Test%20Vision%202/README.md): Module E added, E+M begin to awaken
