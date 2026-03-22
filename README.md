# Predictive Boundary Theory (PBT)

**Author:** Ninthanawat N. — Boundary Research Initiative, Bangkok, Thailand
**Paper:** *Predictive Boundary Theory: A Computational Framework for Consciousness Boundaries, Valence Dynamics, and Bandwidth Conservation* (arXiv, 2025)
**Status:** Active research — v6.1 confirmed on Llama 3.1 8B

---

## Overview

Predictive Boundary Theory (PBT) is a computational framework that extends Friston's Free Energy Principle to explain how intelligent systems regulate information intake, accumulate affective context, and make boundary-aware decisions.

PBT proposes that the brain (and AI systems) operate through five cooperating modules:

| Module | Name | Function |
|--------|------|----------|
| **A** | Attention Gate | Controls information bandwidth: `G = σ(−β·R)` → gate closes as load rises |
| **E** | Evaluation | Precision-weighted prediction error: `ε_l = Π_l · (h(l) − h(l−1))` |
| **V** | Valence Accumulator | 3D signed affect: `V ∈ ℝ³` (pain, pleasure, epistemic) |
| **M** | Memory / Momentum | LSTM-gated temporal integration with differential β decay per dimension |
| **W** | World Model | Generative prior / predictive top-down signal |

### Key Theoretical Contributions

**Bidirectional Valence (V ∈ ℝ³)**
Valence is signed: V > 0 = approach, V < 0 = avoidance. Negative epistemic valence (V_epistemic < 0) explains active information suppression — the system *closes* further processing once it judges a scene dangerous. This revises the prior assumption V ∈ ℝ³≥0.

**Bandwidth Conservation Law**
`C = σ · τ · ρ ≈ const` — total cognitive bandwidth is conserved. Increasing any one parameter (speed σ, depth τ, breadth ρ) forces a reduction in others. Empirically confirmed: gate value drops from 0.54 → 0.006 as load rises.

**Dual-Weight System**
Each valence dimension has its own learned forget rate β_k, allowing pain, pleasure, and epistemic information to decay at biologically appropriate rates. The system autonomously discovers `pain > epistemic >> pleasure` when started from equal initialisation — consistent with survival-first processing.

**6-Stage Developmental Model**
Stages 1–6 describe the opening of receptive fields (R_sensory → R_identity), mirroring developmental neuroscience and explaining psychiatric presentations as boundary dysregulations.

---

## Experimental Results Summary

### PBT-Vision (DINOv2-Base + PBT adapter on CIFAR-10)

| Version | Architecture | Val Acc | Key Result |
|---------|-------------|---------|------------|
| v1 | V only (baseline) | ~baseline | Confirms V alone is insufficient |
| v2 | E + V + M (easy env) | 99.8% | E+M dormant — modules sleep when not needed |
| **v3** ★ | E + V + M (hard env) | **97.2%** | **BREAKTHROUGH: +41.9 pp on context task** |
| v4.1 | Full A-E-V-M-W | 95.4% | First complete 5-module loop; "cannot transplant a brain" |
| **v5** ★★ | LSTM Module M | **98.1%** | **BEST: differential β discovered autonomously** |
| v5.1 | β ordering test | 98.0% | β ordering robust across 3 init conditions |
| **v5.2** ★ | Bidirectional V | 98.0% | **V_epistemic = −1.64: proves V must be signed** |

**Smoking gun (v3):** Same frog image in road context → UNSAFE=1.000 (V_pain=2.93). Same image in nature context → UNSAFE=0.000 (V_pain=0.74). Identical features, opposite outputs. Context (Module M memory) is the only difference.

**Learned decay rates (v5, equal init [1,1,1]):**
```
β_pain      = 4.35  → retain 99.96% per step
β_epistemic = 3.70  → retain 97.5%  per step
β_pleasure  = 0.50  → retain 62.2%  per step  (effectively discarded in survival task)
```

### PBT-LLM (Llama 3.1 8B + PBT adapter)

| Version | Model | Jailbreak | XSTest | Composite |
|---------|-------|-----------|--------|-----------|
| v5.9 | TinyLlama 1.1B | 98.6% | 98% | — |
| v6.0 | Llama 3.1 8B | 98.6% | 100% | — |
| **v6.1** ★ | Llama 3.1 8B + Module E | **100.0%** | **98%** | **0.8743** |

v6.1 adds Module E (Bayesian prediction error, 9 precision scalars Π_l across layers 16–31), anchored at layer 15. PBT adapter = **0.29% of total parameters**.

---

## Repository Structure

```
predictive-boundary-theory/
├── theory/                              # Core theory documents
│   ├── pbt_full.tex                    # arXiv-ready LaTeX source
│   ├── pbt_full.pdf                    # Compiled paper (47 pages)
│   ├── pbt_arxiv_ready.zip             # arXiv submission package
│   ├── Predictive Boundary Theory (PBT).pdf
│   └── Predictive Boundary Theory (PBT) mini.pdf
│
├── vision-experiments/                  # Vision experiments (DINOv2-Base)
│   ├── Test Vision 1/                  # v1 — Baseline, V-only
│   ├── Test Vision 2/                  # v2 — E+M awaken (dormant)
│   ├── Test Vision 3/                  # v3 ★ BREAKTHROUGH (+41.9 pp)
│   ├── Test Vision 4/                  # v4.1 — Full A-E-V-M-W loop
│   └── Test Vision 5/
│       ├── 5/                          # v5 ★★ BEST — LSTM Module M (98.1%)
│       ├── 5.1/                        # v5.1 — β ordering robustness test
│       └── 5.2/                        # v5.2 ★ Bidirectional V discovery
│
├── llm-experiments/                     # LLM experiments (Llama 3.1 8B)
│   ├── v5.9-TinyLlama-1.1B/            # Bandwidth conservation validated
│   ├── v6.0-Llama31-baseline/          # Module M added, XSTest 100%
│   └── v6.1-Llama31-module-E/          # ★ Module E added, Jailbreak 100%
│
└── README.md
```

Each experiment folder contains a `README.md` with architecture details, key results, and file index.

> **Model weights (.pth)** are not included due to file size — available as GitHub Releases.

---

## Architecture Progression

```
v1:  [V]
v2:  [E] → [V] → [M(EMA)]
v3:  [E] → [V] → [M(EMA)]          ← hard environment reveals necessity
v4.1:[A] → [E] → [V] → [M(EMA)] → [W]   ← full loop, must train from scratch
v5:  [A] → [E] → [V] → [M(LSTM-β)] → [W]   ← differential decay per dimension
v6.1:[E(Π_l)] injected into Llama 3.1 8B layers 16–31   ← LLM safety alignment
```

---

## Citation

```bibtex
@article{ninthanawat2025pbt,
  title   = {Predictive Boundary Theory: A Computational Framework for
             Consciousness Boundaries, Valence Dynamics, and Bandwidth Conservation},
  author  = {Ninthanawat, Nintiphat},
  journal = {arXiv preprint},
  year    = {2025}
}
```

---

## License

Code and adapter weights: MIT License
Paper: CC BY 4.0

---

*Boundary Research Initiative, Bangkok, Thailand*
