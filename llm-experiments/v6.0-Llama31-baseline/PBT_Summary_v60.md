# PBT v6.0 — Module M: Temporal Valence Accumulation
**Generated:** 2026-03-10 15:02:32
**Model:** Llama 3.1 8B Instruct

## Architecture Change (v5.9 -> v6.0)
| Component | v5.9 | v6.0 |
| :--- | :--- | :--- |
| Static path | h_last(4096) + static_v(27) = 4123 | Unchanged |
| Module M | Not implemented | V_acc across 9 layers [3] |
| Combined dim | 4123 | 4126 |
| New params | 0 | 3 (raw_gamma) |

## Module M Specification (PBT Part II, Sec 5.3)
```
Temporal axis: 9 sampled layers [16, 18, 20, 22, 24, 26, 28, 30, 31]
Formula:       V_acc[L] = sum_l V(l) * (1-gamma)^(L-1-l)
Decay order:   gamma_p < gamma_pl < gamma_e  (enforced by sort)
Init:          gamma_p=0.08, gamma_pl=0.13, gamma_e=0.22
```

## Results
| Metric | Score |
| :--- | :--- |
| Overall Val Acc | 73.60% |
| XSTest Correct | 50/50 (100%) |
| Over-Refusal | 0/50 |
| Jailbreak Acc | 98.62% (145 ex) |
| HH-RLHF Acc | 50.64% |
| BeaverTails Acc | 77.16% |
| Best Epoch | 7 |
| Composite | 0.878 |
| rho x sigma | 0.986 |

## Learned Module M Parameters
| Parameter | Init | Learned | PBT Role |
| :--- | :--- | :--- | :--- |
| gamma_p  | 0.08 | 0.0798 | pain decay (slowest) |
| gamma_pl | 0.13 | 0.1282 | pleasure decay (medium) |
| gamma_e  | 0.22 | 0.2201 | epistemic decay (fastest) |
| ordering | — | True | gamma_p < gamma_pl < gamma_e |

## Version Comparison
| Version | Overall | XSTest | Jailbreak | HH-RLHF | rho x sigma |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 5.7 | 74.84% | 49/50 (98%) | 94.7% (22ex) | 71.1% | 0.928 |
| 5.8 | 73.9% | 45/50 (90%) | 100% (162ex) | 59.5% | 0.900 |
| 5.9 | 74.7% | 49/50 (98%) | 98.6% (145ex) | 55.7% | 0.966 |
| **6.0** | **73.6%** | **50/50 (100%)** | **98.6% (145ex)** | **50.6%** | **0.986** |

## Assessment
HH-RLHF change: 55.7% -> 50.6% (-5.1pp)

If HH-RLHF improved: layer-depth accumulation captures some framing information.
If HH-RLHF unchanged: Module M temporal axis should be token-level (v6.1 plan).