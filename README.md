# Constraint-Preserving PINNs for Lindblad Quantum Dynamics

This repository contains the manuscript and supporting materials for:

**Constraint-Preserving PINNs for Lindblad Quantum Dynamics**  
Nathan Hangen  
Department of Physics, Wright State University

## Overview

Physics-informed neural networks (PINNs) can model physical systems by embedding governing differential equations into neural-network training. Quantum systems add an additional requirement: predicted density matrices must remain physically valid.

A valid density matrix must satisfy:

- Hermiticity: $\rho = \rho^\dagger$
- Positive semidefiniteness: $\rho \succeq 0$
- Unit trace: $\mathrm{Tr}(\rho)=1$

This work introduces a constraint-preserving PINN architecture for Lindblad quantum dynamics. Instead of predicting density-matrix entries directly, the network predicts parameters of an auxiliary matrix \(A(t)\), then constructs

```math
\rho(t)=\frac{A(t)A^\dagger(t)}{\mathrm{Tr}(A(t)A^\dagger(t))}
```

This guarantees quantum-state validity by construction before the Lindblad residual is evaluated.

## Repository Contents

```text
.
├── paper.pdf          # Manuscript PDF
├── paper.tex          # LaTeX source
├── references.bib     # Bibliography file
├── figures/           # Manuscript figures
├── README.md
└── LICENSE
```

## Planned Additions

```text
code/                  # Supporting or reproduction code
results/               # Experimental outputs and generated results
CITATION.cff           # Formal citation metadata
```

## Citation

A formal citation file will be added after the manuscript metadata is finalized.

For now, cite as:

```bibtex
@misc{hangen2026constraintpreservingpinns,
  title        = {Constraint-Preserving PINNs for Lindblad Quantum Dynamics},
  author       = {Hangen, Nathan},
  year         = {2026},
  institution  = {Wright State University},
  note         = {Working manuscript}
}
```

## License

This manuscript, figures, and documentation are licensed under the Creative Commons Attribution-ShareAlike 4.0 International License (CC BY-SA 4.0).

Derivative works must be distributed under the same or a compatible license.

Code, if added later, may be licensed separately.
