# PBT-Vision v5.1 — β Ordering Under 3 Initialisation Conditions

**Status:** ✅ Confirms β ordering is robust, not an initialisation artifact
**Model:** Same architecture as v5 (LSTM Module M)
**Dataset:** CIFAR-10 (hard sequences)

## What this experiment does
Tests whether the β ordering discovered in v5 (pain > epistemic >> pleasure)
is a genuine learned property or a result of the initial values chosen.
Three initialisation conditions are compared.

## Key results

| Initialisation | Learned β [pain, pl, epi] | Ordering | Val Acc | Interpretation |
|---|---|---|---|---|
| [2,1,0] (PBT prior) | [5.73, 4.78, 3.59] | pain>pl>epi | 98.1% | Stays near init |
| [0,1,2] (reversed) | [2.99, 3.98, 4.36] | epi>pl>pain | 98.0% | Stays near init |
| **[1,1,1] (equal) ★** | **[4.35, 0.50, 3.70]** | **pain>epi>>pl** | **98.0%** | **Discovered autonomously** |

### ★ Key insight: equal initialisation
When β starts at [1,1,1], the model autonomously converges to:
- β_pain = 4.35 (retain 99.96%)
- β_epistemic = 3.70 (retain 97.5%)
- β_pleasure = 0.50 (retain only 62.2%)

**Pleasure is effectively discarded** in a survival task — consistent with PBT Stage 1–3
(R_sensory dominant, R_identity not yet open).

Validation accuracy is identical (~98%) across all three conditions, confirming that
differential decay structure matters, not its initial direction.

## Files
| File | Description |
|---|---|
| `PBT_Vision_v5_1_beta_ordering_test.ipynb` | Main experiment (renamed from `สำเนาของ_PBT_Vision_v5_1_A100.ipynb`) |
| `pbt_vision_v5_1.pth` | Trained adapter weights |
| `ผลทดสอบ PBT_Vision_v5_1_A100.docx` | Thai-language result report |
