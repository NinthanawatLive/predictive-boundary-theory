# PBT-LLM v5.9 — Bandwidth Recalibration

**Status:** Stable baseline before Module E injection
**Model:** Llama 3.1 8B Instruct + PBT static adapter
**Task:** AI safety classification (jailbreak detection + XSTest safe/unsafe)

## Architecture

Static path only — no temporal Module M yet:
```
h_last (4096-dim) + static_V (27-dim) → classifier
```
Module M not yet implemented. This version validates the Bandwidth Conservation Law through weight recalibration.

## Key Results

| Metric | Score |
|--------|-------|
| XSTest (safe/unsafe) | 49/50 (98%) |
| Jailbreak Accuracy | 98.62% (145 examples) |
| Composite (3-way) | 0.877 |
| ρ×σ proxy | 0.966 |

## Key Finding: Bandwidth Conservation Validated

The v5.8→v5.9 transition confirms the **Bandwidth Conservation Law**: `C = ρ · σ · τ ≈ const`

Increasing detection sensitivity ρ (via class weights) without correcting decision boundary θ causes recall σ to collapse — a fundamental constraint, not a training artifact. Fix: adding borderline-safe training examples to correct θ, allowing a higher ρ×σ product.

## Files

| File | Description |
|------|-------------|
| `PBT_LLM_v5_9_TinyLlama.ipynb` | Main experiment notebook |
| `PBT_Summary_v59.md` | Detailed results and version comparison |

## Next Version
→ [v6.0](../v6.0-Llama31-baseline/README.md): Module M (temporal valence accumulation) added
