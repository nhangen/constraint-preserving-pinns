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
