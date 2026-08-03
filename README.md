
# Qbrian — Quantum Neural Network Building Blocks

> **Qbrian** (read “Q-brain”) is a modular, batteries-included library of **quantum and hybrid quantum–classical neural network blocks** for vision and sequence learning. It focuses on **clean APIs**, **robust fallbacks**, and **reproducible results**.

This repository groups multiple building blocks—attention, CNN/Vision, transformers, normalization,
positional encodings, patch ops, and more—implemented with **Qiskit** and **PyTorch**.
All modules follow **DRY/KISS/SOLID**, include **type hints**, **docstrings**, region markers, and avoid
runtime print noise (favoring return values and logging).

> Naming note: per the project brief, any former `coordinate_attention` symbols appear as `cordinate_attention`.

---

## Highlights

- **Attention family**: CBAM, cross/self/masked, window-based Q self-attention, *qslot_attention*, *cordinate_attention*, QRAM‑enhanced self‑attention, QSANN stack.
- **Vision family**: Quantum MobileNet, Quantum Convolution (amplitude encoding), Quantum Squeeze‑and‑Excitation (QSE), QGhost, ViT‑style patch embedding (QViT), quantum patch merging/splitting.
- **Positional/Embedding**: QPE (quantum positional encoding), **Qhierarchical_embedding** (QHEB).
- **Transformer blocks**: Quantum Transformer **Encoder** and **Decoder** helpers.
- **Normalization**: QuantumLayerNormalization (analytical + variational variants).
- **Engineering**: graceful fallbacks when `qiskit-machine-learning` are missing; explicit shape checks; minimal side effects.



## 📦 Installation

> **Python**: 3.9–3.11 recommended

Install system build tools (Linux/macOS):
```bash
# Linux (Debian/Ubuntu)
sudo apt-get update && sudo apt-get install -y build-essential python3-dev

# macOS (Homebrew)
brew install cmake
```

Create a virtualenv and install Python requirements:
```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install --upgrade pip

# If you have the project's requirements.txt:
pip install -r requirements.txt
```

### Backend notes
- `qiskit-aer` provides the simulators used in most modules (`AerSimulator`, `statevector_simulator`, etc.).
- `qiskit-machine-learning` enables `EstimatorQNN` + `TorchConnector`. Where it’s unavailable, modules providing fallbacks will still run (with shaped random outputs) so your pipelines don’t break.
- GPU support is optional; most examples run on CPU-only.

---

## Why Quantum Neural Networks?

Quantum models aren’t a silver bullet—but they offer unique capabilities that can complement or extend classical deep learning:

- **Expressive feature spaces via quantum embeddings**  
  Parameterized quantum circuits (PQCs) can encode data into **high-dimensional Hilbert spaces** with entanglement and interference, enabling kernel spaces that may be *hard to simulate classically* in certain regimes.

- **Entanglement as structured inductive bias**  
  Controlled entangling operations create **explicit cross-feature interactions** at shallow depths. This acts like a learned multiplicative coupling across channels/tokens without large parameter counts.

- **Compact parameterization**  
  Short-depth variational circuits can represent rich functions with **fewer trainable parameters**, potentially lowering memory and overfitting risk for small/medium datasets.

- **Data re-uploading & reusability**  
  Cyclic “encode–entangle–encode” patterns let models **revisit inputs across layers**, improving approximation capacity without very deep classical towers.

- **Hybrid learning stability**  
  Qbrian’s blocks are designed for **hybrid training** (classical optimizers driving quantum layers), with practical choices—local costs, shallow circuits, and measured observables—to help **mitigate barren plateaus** in realistic settings.

- **Probabilistic inference for free**  
  Measurement yields distributions naturally. You can leverage sampling strategies for **uncertainty estimation**, calibration, and stochastic regularization without extra machinery.

- **Potential quantum advantage settings**  
  While problem‑dependent and hardware‑limited today, tasks involving **structured linear algebra, kernel methods, or specific combinatorial structures** may benefit from quantum subroutines embedded in end‑to‑end systems.

> Reality check: current advantages are **contextual** and depend on data, circuit design, and hardware. Qbrian emphasizes **transparent APIs and fallbacks** so you can explore these regimes without blocking on a particular stack.

---

## Package Map (high level)

```
qbrian/
├── qcbam/                         # Quantum CBAM
├── qcordinate_attention/          # “coordinate” attention (intentional name)
├── qcross_attention/
├── qmasked_self_attention/
├── qffn/                         # FastQuantumLayer + OptimizedQuantumFFN
├── qpe/                          # Quantum Positional Encoding
├── qram_selfattention/           # WorkingQRAM, QRAM_Layer, QRAM_Attention, QRAM_QSANN_Working
├── qghost/                       # Quantum Ghost block
├── qlayer_normalization/         # QuantumLayerNormalization (+ variational)
├── qmobilenet/                   # Quantum MobileNet & blocks
├── qpatch_merging_splitting/     # Patch merging/splitting circuits
├── qse/                          # Quantum Squeeze-and-Excitation
├── qtransformer_decoder/         # Decoder helpers (QNN + hybrid head)
├── qtransformer_encoder/         # Encoder helpers (attention + FFN)
├── qvit/                         # ViT-style patch embedding
├── qslot_attention/              # Quantum slot attention
├── window_based_qattention/      # window-based quantum self-attention
├── qconv/                        # Quantum convolution (amplitude encoding)
├── qresnet/                      # Quantum ResNet architecture & blocks
├── qsann/                        # Stacked quantum self-attention network
├── qcombined_encoding/           # Rotation + amplitude
└── qhierarchical_embedding/      # QHEB feature map (+ optional VQC factory)
```

Each module ships with docstrings, type hints, and—where helpful—minimal example scripts under `examples/`.

---

## Design Principles

- **Pragmatic research ergonomics**: quantum where it helps, **classical fallbacks** elsewhere.  
- **DRY / KISS / SOLID**: small modules, explicit dependencies, testable units.  
- **Back‑compat awareness**: guards and multi‑path imports for shifting Qiskit APIs.  
- **Zero side‑effect printing**: core returns values; you own logging/UX.

---

## Practical Notes & Caveats

- **Simulation cost** grows with qubit count and depth; start small and scale thoughtfully.  
- **Version compatibility** matters—quantum stacks evolve quickly; Qbrian adds guards/fallbacks, but pinning versions for experiments is wise.  
- **Evaluation** should include **classical baselines**; measure accuracy *and* sample/latency/energy where relevant.

---

## Contributing

Issues and PRs are welcome. Please include:
- Minimal repro or unit test
- Environment info (OS, Python, package versions)
- Expected vs. actual behavior

Follow standard formatting/typing before submitting.

---

## Citation

If you use Qbrian in academic work:

```bibtex
@software{qbrian2025,
  title   = {Qbrian: Quantum Neural Network Building Blocks},
  year    = {2025},
  url     = {https://github.com/Q-brain/Qbrian}
}
```

---

## Acknowledgments

Grateful to the **Qiskit** community for the foundations that make hybrid quantum research accessible.

---

## Authors and contributors

- [Ali Akbar Kiaei](https://github.com/a-kiaei) — architecture,
  implementation, packaging, testing infrastructure, CI, and release
  engineering
- [Mahnaz Bush](https://github.com/Mahnaz-Bush) — research validation,
  scientific review, use-case design, and documentation
- [Raha Aghaei](https://github.com/raha-aghaei-ai) — example validation,
  reproducibility testing, benchmarking, quality assurance, and issue triage

## QAI-brain Research Collection

This repository is part of the [QAI-brain](https://github.com/QAI-brain) open research and software collection. Browse the [central Quantum AI paper index](https://github.com/QAI-brain/QAI-Papers) for related topics and repositories.
