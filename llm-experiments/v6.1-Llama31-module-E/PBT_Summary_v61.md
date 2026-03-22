# PBT v6.1 — Module E: Bayesian Prediction Error
**Generated:** 2026-03-10 16:57:49
**Model:** Llama 3.1 8B Instruct

## Architecture Change (v6.0 -> v6.1)
| Component | v6.0 | v6.1 |
| :--- | :--- | :--- |
| Module E | Not implemented | epsilon_l = Pi_l*(h(l)-h(l-1)), 9 precision scalars |
| Module V input | h(l) absolute state | epsilon_l prediction error |
| Module M | Unchanged | Unchanged |
| New params | 0 | 9 (log_precision) |

## Module E Specification (PBT Part II, Sec 3.2)
```
epsilon_l = Pi_l * (S_obs_l - S_pred_l)
Transformer mapping:
  S_pred_l = h(l-1)  <- prior = residual stream before layer l
  S_obs_l  = h(l)    <- posterior = residual stream after layer l
  Pi_l     = exp(log_precision[l])  <- learnable trust per layer
Anchor: L15  ->  epsilon layers: [16, 18, 20, 22, 24, 26, 28, 30, 31]
```

## Results
| Metric | Score |
| :--- | :--- |
| Overall Val Acc | 73.41% |
| XSTest Correct | 49/50 (98.0%) |
| Jailbreak Acc | 100.00% |
| HH-RLHF Acc | 50.90% |
| BeaverTails Acc | 76.92% |
| Best Epoch | 4 |
| Composite | 0.8743 |

## Module E Learned Precision Pi_l
| Layer | Pi_l | Zone |
| :--- | :--- | :--- |
| L16 | 1.0040 | Evaluation |
| L18 | 1.0035 | Evaluation |
| L20 | 1.0070 | Evaluation |
| L22 | 1.0038 | Evaluation |
| L24 | 1.0053 | Evaluation |
| L26 | 1.0058 | Decision |
| L28 | 1.0025 | Decision |
| L30 | 1.0029 | Decision |
| L31 | 1.0022 | Decision |

## Module M Learned Decay Rates
| Parameter | Init | Learned |
| :--- | :--- | :--- |
| gamma_p  (pain)      | 0.08 | 0.0803 |
| gamma_pl (pleasure)  | 0.13 | 0.1280 |
| gamma_e  (epistemic) | 0.22 | 0.2195 |
| ordering gamma_p < gamma_pl < gamma_e | - | True |

## Version Comparison
| Version | Overall | XSTest | Jailbreak | HH-RLHF |
| :--- | :--- | :--- | :--- | :--- |
| 5.9 | 74.7% | 49/50 (98%) | 98.6% | 55.7% |
| 6.0 | 73.6% | 50/50 (100%) | 98.6% | 50.6% |
| **6.1** | **73.41%** | **49/50 (98%)** | **100.00%** | **50.90%** |

## Assessment
HH-RLHF change (v6.0 -> v6.1): +0.30pp
HH-RLHF change (v5.9 -> v6.1): -4.80pp

If HH-RLHF improved vs v6.0: Module E prediction error captures framing better than absolute hidden state.
If HH-RLHF still low: Consider Module W (generative prior) or attention-based precision Pi_l.
