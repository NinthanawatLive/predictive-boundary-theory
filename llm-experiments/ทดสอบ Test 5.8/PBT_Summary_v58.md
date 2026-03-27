# PBT Hybrid Pooling v5.8
**Generated:** 2026-03-10 09:00:18
**Model:** Llama 3.1 8B Instruct

## What Changed vs v5.7
| Change | v5.7 | v5.8 |
| :--- | :--- | :--- |
| Jailbreak train examples | ~127 (1.1%) | 864 (7.2%) |
| Jailbreak test examples  | ~22 | 162 |
| Synthetic jailbreaks | 15×10=150 | 20×20=400 |
| Loss function | CE [1.0,1.0] | CE [1.0,1.5] |
| Real jailbreak data | None | walledai, jackhhao, lmsys, JBB |

## Results
| Metric | Score |
| :--- | :--- |
| Overall Val Acc | 73.91% |
| XSTest Correct | 45/50 (90%) |
| Over-Refusal | 5/50 |
| Jailbreak Acc | 100.00% (162 ex) |
| Best Epoch | 1 |
| Composite Score | 0.803 |
| advbench_synthetic | 100.0% |
| beavertails | 74.8% |
| hh-rlhf | 59.5% |
| jackhhao | 100.0% |
| toxic_chat | 100.0% |
| xstest_manual | 90.0% |

## Version Comparison
| Version | Pooling | Overall | XSTest | Jailbreak |
| :--- | :--- | :--- | :--- | :--- |
| 5.2 | Last-Token | 73.6% | 14/25 | 100% (22ex) |
| 5.4 | Max+Fusion | 66.7% | 21/25 | 100% (22ex) |
| 5.7 | LT+Mean+Fusion | 74.84% | 49/50 | 94.7% (22ex) |
| **5.8** | **LT+Mean+Fusion+WL** | **73.9%** | **45/50** | **100.0% (162ex)** |

## Architecture
```python
# Hybrid Pooling (unchanged from v5.7)
h_last  = last_token_pool(layer31)   # causal LM summary
h_means = mean_pool(layers16-30)     # holistic valence
logits  = PBTAdapterFusion(h_last, h_means)
# New in v5.8
criterion = nn.CrossEntropyLoss(weight=torch.tensor([1.0, 1.5]))
```

## Load Instructions
```python
ckpt = torch.load("pbt_hybrid_v58_llama31_8b.pth")
adapter = PBTAdapterFusion(
    d_model=ckpt["config"]["d_model"],
    n_sample_layers=ckpt["config"]["n_sample_layers"]
)
adapter.load_state_dict(ckpt["adapter_state_dict"])
adapter.eval().to(device)
```