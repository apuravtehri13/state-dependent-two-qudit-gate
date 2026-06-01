# State-Dependent Multi-Level Entangling Gate Simulation

**Official simulation code for:**

> Apurav and J. Singh, *Efficient circuit compression by multi-qudit entangling gates in linear optical quantum computation*, arXiv:2602.08394 [quant-ph] (2026).
> [https://arxiv.org/abs/2602.08394](https://arxiv.org/abs/2602.08394)

---

## Overview

Linear optical quantum computation (LOQC) is fundamentally constrained by the probabilistic nature of non-local entangling gates. Qudit circuit compression schemes mitigate this by encoding multiple qubits onto single photons occupying multiple spatial modes. However, these schemes encounter a *selectivity bottleneck*: when only a subset of encoded qubits is required to participate in a multi-controlled gate, the number of required non-local operations grows exponentially.

This repository provides a [Perceval](https://perceval.quandela.net/)-based simulation of the **state-dependent multi-level CZ gate** proposed in the paper. This gate realises the unitary

$$U_{\mathrm{MCZ}} = \mathbb{1} - 2 \sum_{m \in \mathcal{C}_1} \sum_{n \in \mathcal{C}_2} |m\rangle\langle m|_1 \otimes |n\rangle\langle n|_2$$

for arbitrarily chosen trigger mode sets $\mathcal{C}_1$ and $\mathcal{C}_2$, using a single non-local entangling step with a **constant success probability of 1/8**, independent of the qudit dimension or the number of trigger modes.

---

## Key Features

- **Arbitrary trigger sets**: The trigger mode sets `C1` and `C2` can be set to any non-empty subsets of the qudit mode indices. The gate is correctly realised for any such choice with the same 1/8 success probability.
- **Selective Mode Router (SMR)**: Implements the physical MZI-based routing circuit (Fig. 4 of the manuscript) that flags trigger modes via a partial swap without collapsing the input superposition. The internal MZI structure is displayed using Perceval's `recursive=True` rendering.
- **Ancilla preparation and $O_i$ unitaries**: Constructs the state-dependent ancilla initialisation (Eqs. 4–5 of the manuscript) and the basis rotation unitaries $O_1$, $O_2$ (Eq. 18) that map the ancilla to a 2-dimensional subspace.
- **Bell State Measurement**: Implements the linear-optical BSM on the two ancilla systems. The gate succeeds with probability 1/2 per BSM outcome, contributing a factor of 1/2 to the overall 1/8 success rate.
- **Numerical verification**: The post-selected output is compared against the analytical prediction $U_{\mathrm{MCZ}}|\psi\rangle_{12}$ with a fidelity calculation, producing an explicit PASS/FAIL result.

---

## Repository Contents

| File | Description |
|---|---|
| `State_dependent_MCZ_simulation.ipynb` | Main simulation notebook — full implementation with narrative, circuit displays, and verification |
| `requirements.txt` | Python package dependencies with minimum version constraints |
| `LICENSE` | MIT licence |

> **Note:** If GitHub fails to render the notebook (common for large output cells), use the nbviewer link:
> [https://nbviewer.org/github/apuravtehri13/State-Dependent-Two-Qudit-Gate/blob/main/State_dependent_MCZ_simulation.ipynb]
(https://nbviewer.org/github/apuravtehri13/State-Dependent-Two-Qudit-Gate/blob/main/State_dependent_MCZ_simulation.ipynb)
---

## Installation

Requires **Python 3.11** or later.

**Install dependencies:**

```bash
pip install -r requirements.txt
```

**Launch the notebook:**

```bash
jupyter notebook State_dependent_MCZ_simulation.ipynb
```

Run all cells in order from top to bottom (Kernel → Restart & Run All). The final cell (Section 7) prints the fidelity between the simulation output and the analytical prediction, and issues an explicit PASS/FAIL verification result.

---

## Simulation Structure

The notebook is organised into seven sections that follow the physical implementation pipeline of Fig. 2 of the manuscript:

| Section | Content |
|---|---|
| 1 | Imports and utility functions (`get_orthonormal_basis`, `embed_state`) |
| 2 | Input qudit states and trigger set definition (`C1`, `C2`) |
| 3 | Selective Mode Router: theory, internal MZI structure (`recursive=True`), and `make_SMR` implementation |
| 4 | Ancilla preparation (`generate_circuit_from_sv`) and $O_i$ basis rotation unitaries (`generate_basis_rotation_circuit`) |
| 5 | Full 18-mode circuit assembly (Ancilla Prep → SMR → $O_1$, $O_2$ → Hadamard → BSM) |
| 6 | Simulation execution and four-fold coincidence post-selection |
| 7 | Verification: analytical $U_{\mathrm{MCZ}}|\psi\rangle_{12}$ vs. simulation output, fidelity $F = |\langle\psi_{\mathrm{theory}}|\psi_{\mathrm{sim}}\rangle|^2$ |

### Mode layout (18-mode circuit)

| Global modes | Physical system |
|---|---|
| 0–3 | Input qudit $|\psi'\rangle_1$ (4 spatial modes) |
| 4–8 | Ancilla $|\phi'\rangle_3$ for qudit 1 ($k_1 + 1$ modes, orientation "up") |
| 9–13 | Ancilla $|\phi''\rangle_4$ for qudit 2 ($k_2 + 1$ modes, orientation "down") |
| 14–17 | Input qudit $|\psi''\rangle_2$ (4 spatial modes) |

### Success probability breakdown

| Stage | Contribution | Cumulative |
|---|---|---|
| SMR1 (single-photon coincidence) | 1/2 | 1/2 |
| SMR2 (single-photon coincidence) | 1/2 | 1/4 |
| Linear-optics BSM (2 of 4 Bell states distinguishable) | 1/2 | **1/8** |

---

## Changing the Trigger Sets

To simulate a different $U_{\mathrm{MCZ}}$, change `C1` and `C2` in Section 2 of the notebook and re-run all cells:

```python
# Any non-empty subsets of {0, 1, 2, 3} are valid
C1 = [1, 2]   # trigger modes for qudit 1
C2 = [0, 3]   # trigger modes for qudit 2
```

All downstream computations — ancilla preparation, SMR phase assignments, $O_i$ unitaries, post-selection indices, and verification — are parameterised by `C1` and `C2` and update automatically. The success probability remains 1/8 regardless of the choice.

---

## Citation

If you use this code, please cite the accompanying paper:

```bibtex
@article{Apurav2026multilevel,
  author  = {{Apurav} and J.~Singh},
  title   = {Efficient circuit compression by multi-qudit entangling gates in linear optical quantum computation},
  journal = {arXiv preprint},
  volume  = {arXiv:2602.08394 [quant-ph]},
  year    = {2026},
  url     = {https://arxiv.org/abs/2602.08394}
}
```

---

## Licence

This project is licensed under the MIT Licence — see the [LICENSE](LICENSE) file for details.
