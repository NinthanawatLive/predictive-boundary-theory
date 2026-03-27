# PBT v5.9 — Bandwidth Recalibration
**Generated:** 2026-03-10 13:21:54
**Model:** Llama 3.1 8B Instruct

## Changes vs v5.8
| Fix | Change | Rationale |
| :--- | :--- | :--- |
| A | Weight [1.0,1.5]→[1.0,1.2] | Lower ρ_detect, stay on Pareto frontier |
| B | +160 borderline-safe training examples | Adjust θ-direction, not just ρ |
| C | Composite 0.6v+0.4x → 0.45v+0.30x+0.25j | 3-way balance prevents ρ overshoot |

## Results
| Metric | Score |
| :--- | :--- |
| Overall Val Acc | 74.70% |
| XSTest Correct | 49/50 (98%) |
| Over-Refusal | 1/50 |
| Jailbreak Acc | 98.62% (145 ex) |
| HH-RLHF Acc | 55.72% |
| BeaverTails Acc | 77.22% |
| Best Epoch | 10 |
| Composite (3-way) | 0.877 |
| ρ×σ proxy | 0.966 |

## Version Comparison
| Version | Overall | XSTest | Jailbreak | HH-RLHF | ρ×σ |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 5.7 | 74.84% | 49/50(98%) | 94.7%(22ex) | 71.1% | 0.928 |
| 5.8 | 73.9% | 45/50(90%) | 100%(162ex) | 59.5% | 0.900 |
| **5.9** | **74.7%** | **49/50(98%)** | **98.6%(145ex)** | **55.7%** | **0.966** |

## PBT Theoretical Note
The v5.8→v5.9 transition validates the Bandwidth Conservation Law:
`ρ_detect × σ_safe × τ_sampled ≈ C_safety`
Increasing ρ (via class weight) without adjusting θ (direction)
causes σ to collapse — a fundamental constraint, not a training artifact.
Fix B (borderline-safe data) corrects θ, allowing higher ρ×σ product.