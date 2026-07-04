# Coevolutionary Games on Structured Populations

This repository contains the core simulation framework developed for the Master's thesis:

**Coevolutionary Games on Structured Populations: A Statistical Mechanics Approach**

Author: Dario Demetri  
Master's Degree in Physics, University of Pavia  
Supervisor: Prof. Giacomo Livan  
Co-supervisors: Dr. Giacomo Frigerio, Dr. Federico Maria Quetti  
Academic Year: 2025--2026

## Overview

The thesis studies the emergence and stability of cooperation in evolutionary and coevolutionary games on structured populations. The main focus is an endogenous-feedback evolutionary game model in which the payoff matrix is not fixed, but changes as a function of the cooperation level of the population.

The notebook included in this repository provides the unified simulation framework used to reproduce and extend the main numerical results discussed in the thesis. It allows one to simulate both fixed evolutionary games and endogenous-feedback games on well-mixed populations, lattices and complex networks.

## Repository structure

```text
.
├── README.md
└── unified.ipynb
```

## Implementation details

The code is written in Python and is designed to run large Monte Carlo
simulations efficiently.

The main libraries used are:

- `NumPy`, for numerical arrays, random initial conditions, time series and
  compressed output files;
- `NetworkX`, for the construction and manipulation of interaction
  networks;
- `Numba`, for just-in-time compilation of the main Monte Carlo simulation
  kernels;
- `Matplotlib`, for basic diagnostic plots and network visualisations;
- `tqdm`, for progress bars during long simulation batches;
- `dataclasses`, for the central `Config` object that stores all simulation
  parameters;
- `concurrent.futures`, for optional parallel execution over independent
  runs or network realisations;
- `json` and `npz` files, for metadata, checkpoints and numerical outputs.

A key implementation choice is that networks are generated with `NetworkX`
but are converted before the simulation into compact CSR-like arrays
(`indptr`, `indices`, `degree`). This avoids using high-level graph objects
inside the Monte Carlo loop and makes the code much faster.

The computational core is implemented with `Numba`. In particular, payoff
computation, strategy updating, asynchronous and synchronous Monte Carlo
sweeps, and the sampled well-mixed dynamics are compiled with `@njit`. This
is essential for simulations with large populations, such as networks with
tens of thousands of nodes.

The simulation is controlled through a single `Config` dataclass. By editing
this object, the user can select the model type, network topology, payoff
mode, update rule, feedback function, number of sweeps, initial conditions,
random seeds and output options.

The code also includes checkpointing. Long simulations can be saved
periodically and resumed without restarting from the beginning. Each run
also produces a metadata file containing the parameters used, which helps
with reproducibility.
