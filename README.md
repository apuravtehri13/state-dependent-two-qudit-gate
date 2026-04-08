This repository contains the official implementation of the state-dependent multi-level entangling gate scheme proposed in the paper: "Efficient circuit compression by multi-qudit entangling gates in linear optical quantum computation."
Read on ArXiv: https://arxiv.org/abs/2602.08394

## Overview
In Linear Optical Quantum Computation (LOQC), standard qudit circuit compression schemes encounter a "selectivity bottleneck" when only a subset of encoded qubits is required to participate in a multi-controlled operation, leading to an exponential overhead in the required number of non-local entangling gates. 

This repository provides the code to simulate our state-dependent scheme, which overcomes this bottleneck. The provided notebook demonstrates the realization of a multi-level control-Z (CZ) gate that applies a conditional phase flip for an arbitrarily chosen subset of spatial modes across two encoded qudits. 

### Key Features
Constant Success Probability: Achieves the multi-level entangling operation using a single operation with a success probability of 1/8 (incorporating feedforward corrections).
Selective Mode Routing (SMR): Simulates the physical optical routing that flags the specified trigger modes without collapsing the superposition.
Bell State Measurement (BSM): Implements the projective measurement and local unitary feedforward corrections required to finalize the gate (feedforward corrections are omitted for the current code).

## Repository Contents
`(2026-03-16) Multi-level entangling scheme.ipynb`: The main Jupyter Notebook containing the theoretical simulation and circuit implementation of the state-dependent multi-level CZ gate.
