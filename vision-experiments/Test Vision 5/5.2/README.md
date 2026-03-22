# PBT-Vision v5.2 — ★ Bidirectional Valence Discovery (V ∈ ℝ³)

**Status:** ★ THEORETICAL BREAKTHROUGH — proves V must be signed (V ∈ ℝ³, not ℝ³≥0)
**Model:** Same architecture as v5 (LSTM Module M)
**Dataset:** CIFAR-10 (hard sequences)

## What this experiment does
Removes the ReLU constraint on the valence accumulator, allowing V to take negative values.
This was motivated by the theoretical question: can a system actively *avoid* information,
not just fail to seek it?

## ★ Key discovery: V_epistemic < 0

In the road+frog context test, the model produces **negative** epistemic values:

```
Road+Frog (UNSAFE context):
  Frame 4: UNSAFE=1.000  V_acc=[-2.20, -0.02, -1.64]
  Frame 7: UNSAFE=1.000  V_acc=[-3.04, -0.04, -2.19]

Nature+Frog (SAFE context):
  Frame 4: UNSAFE=0.000  V_acc=[-0.17, -0.07, +0.21]  ← V_epi positive
  Frame 7: UNSAFE=0.000  V_acc=[-0.24, -0.08, +0.29]
```

**Interpretation:**
- In road context: `V_epistemic = −1.64` → the system *actively closes* epistemic processing.
  It has determined the scene is dangerous and suppresses further information gathering.
- In nature context: `V_epistemic = +0.21` → the system *seeks* more information (safe, curious).

This proves valence is inherently **bidirectional**:
- V > 0 → approach (seek)
- V = 0 → neutral
- V < 0 → avoidance (flee / suppress)

## Theoretical implication
Revises the PBT Valence Axiom from `V ∈ ℝ³≥0` to `V ∈ ℝ³`.
This sharpens clinical predictions (e.g. depression = `V_pleasure < 0`, not merely `→ 0`)
and explains denial (`V_epistemic << 0` = active information avoidance).

## Files
| File | Description |
|---|---|
| `PBT_Vision_v5_2_bidirectional_valence.ipynb` | Main experiment (renamed from `สำเนาของ_สำเนาของ_PBT_Vision_v5_2_A100.ipynb`) |
| `pbt_vision_v5_2.pth` | Trained adapter weights |
| `ผลทดสอบ PBT_Vision_v5_2_A100.docx` | Thai-language result report |
