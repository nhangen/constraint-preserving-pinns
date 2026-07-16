# Structural Unitarity and Time-Marching Close the Local-vs-Global Gap in Constraint-Preserving Quantum PINNs

Manuscript and supporting materials for the arxiv-v3 follow-up to
[Constraint-Preserving PINNs for Lindblad Quantum Dynamics][cpp-repo].

**Nathan Hangen** · Independent Researcher

## Overview

The prior constraint-preserving PINN paper~\[1\] established a
trace-normalized Gram-matrix parameterization that guarantees a
density-matrix output is Hermitian, positive semi-definite, and unit-trace
at every time `t`, on a 1-qubit Lindblad pure-dephasing problem with
short time horizon (`T = 5`).

This paper extends the methodology to:

- closed-system (`γ = 0`) dynamics with sustained oscillation rather than
  damped decay,
- longer time intervals (`T = 25`), and
- multi-qubit systems (1q / 2q / 3q closed-system; 1q open-system Kraus).

We report a previously-undocumented failure mode: **physics residual
converges to near zero while the model trajectory drifts from the
analytic solution by an order-one Frobenius distance**. We call this the
*local-vs-global gap*. It is dimension-independent and is not closed by
any of six loss-formulation modifications we tested.

Two changes *together* close it:

1. **Structural unitary parameterization** of `ρ(t) = U(t) ρ₀ U(t)†` with
   `U(t) = exp(-i A(t))` and a boundary factor on `A(t)` that pins
   `ρ(t_min) = ρ₀` exactly.
2. **Time-marching curriculum**: train sequentially on `[0, 1] → [0, 2]
   → … → [0, T]`, warm-starting each stage from the previous.

The combination reduces median trajectory error by **~180× at 1 qubit**
(n = 10 seeds; 9 of 10 in the 145–210× range, one seed still converging
at the 30,000-epoch budget) and **~20× at 2 qubits** (single seed). The
original constraint-preserving result holds on its original problem; the
gap is a scaling artifact at longer intervals and sustained-oscillation
dynamics.

## What's in this directory

| File | Purpose |
|---|---|
| `main.tex` | Manuscript source |
| `references.bib` | Bibliography |
| `figures/fig1_*..fig5_*.pdf` | Paper figures (regenerable; see below) |
| `main.pdf` | Compiled manuscript (gitignored; build locally) |

## Build the PDF

From this directory:

```bash
pdflatex main.tex && bibtex main && pdflatex main.tex && pdflatex main.tex
```

## Regenerate the figures

The committed PDFs under `figures/` are the canonical artifacts. Regeneration
from source requires experiment checkpoints (manifests under
`experiments/DeCoN-PINN/oneq_long_*`, `experiments/DeCoN-PINN/foundation_1q_legacy_*`)
and the curriculum log `logs/oneq_curriculum_*.log` produced by the trainer
runs — none of which ship in this repo. To regenerate after reproducing the
underlying experiments (see *Reproduce the experiments* below):

```bash
python scripts/paper/generate_arxiv_v3_figures.py
```

Each figure references the most recent matching manifest; replace the
glob in the script (`first_match`) if you need a specific run.

## Reproduce the experiments

The trainer is `scripts/train/safe_batch_trainer.py` in the parent repo.
Every result in this paper was produced by one invocation of that script
with different CLI flags. The protocol is documented in Section 2.3 of
the manuscript; the relevant flags map to that section as follows:

| Manuscript field | CLI flag |
|---|---|
| Architecture | `--architecture {cholesky,unitary,kraus,chebyshev}` |
| Matrix size | `--matrix-size {2x2,4x4,8x8,16x16}` |
| Hamiltonian config | `--loss-config-yaml configs/closed_1q_omega1.yaml` (1q only; 2q/3q/4q runs use the trainer's built-in `DEFAULT_*X*_CONFIG` from `matrix_aware_loss`) |
| Initial condition | `--initial-state {ground,plus,plus_zero,bell,ghz}` |
| Time interval | `--t-max 25` |
| Collocation points | `--n-time-points 200` |
| Curriculum stages | `--curriculum-stages "1.0,2.0,5.0,10.0,25.0"` |
| Loss modifications (Tbl. 1) | `--semigroup-weight`, `--terminal-pin-weight`, `--supervised-weight`, `--alm-initial-mu`, `--n-basis` |

Reproducing the headline 1q closed-system + curriculum result:

```bash
python scripts/train/safe_batch_trainer.py \
    --matrix-size 2x2 --architecture unitary \
    --loss-config-yaml configs/closed_1q_omega1.yaml \
    --initial-state plus --t-max 25 \
    --hidden-dim 64 --n-layers 3 --n-time-points 200 \
    --learning-rate 1e-3 --epochs 30000 \
    --curriculum-stages "1.0,2.0,5.0,10.0,25.0" \
    --experiment-name oneq_repro
```

Approximate wall-clock on an RTX 5070: 30 minutes.

The trajectory error analyzer:

```bash
python scripts/analysis/closed_system_trajectory_error_2x2.py \
    --manifest experiments/DeCoN-PINN/oneq_repro_*/runs/run_001/run_manifest.json
```

## Citing this work

```
@misc{hangen2026localglobal,
  author = {Hangen, Nathan},
  title  = {Structural Unitarity and Time-Marching Close the Local-vs-Global Gap in Constraint-Preserving Quantum PINNs},
  year   = {2026},
  note   = {Manuscript, arxiv-v3 draft}
}
```

## References

[1] N. Hangen, *Constraint-Preserving PINNs for Lindblad Quantum
Dynamics*, [github.com/nhangen/constraint-preserving-pinns][cpp-repo].

[cpp-repo]: https://github.com/nhangen/constraint-preserving-pinns

## Prior version (v1)

The earlier paper this work follows up on — *Constraint-Preserving PINNs for
Lindblad Quantum Dynamics* — is archived in [`v1/`](v1/) (`v1/paper.pdf`,
`v1/paper.tex`). It is the "companion paper" referenced in the bibliography.
