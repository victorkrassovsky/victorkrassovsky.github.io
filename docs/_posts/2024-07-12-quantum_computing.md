---
title: Quantum Computing
author: Victor Krassovsky
date: 2024-07-12
category: Jekyll
layout: post
mermaid: true
---

## Quantum Systems
Quantum information systems are built up of four components:
1. State
2. Dynamics
3. Measurement
4. Observables

States are made up of qubits which are complex vectors in a hilbert space $\mathcal{H}$ which is just a vector space equiped with an inner product. We use Dirac notation to denote the state of these qubits, e.g. $\|\psi \rangle $ with $\|\psi\|^2=1$, and the inner product of two states is $\langle \psi \| \varphi\rangle$. In two dimensions, a basis can be formed with the states $\| 0 \rangle$ and $\| 1\rangle$ and the other states can be written as a complex linear combination of this basis. Hence, for any $\| \psi \rangle \in \mathcal{H}$,

$$| \psi \rangle = \alpha |0\rangle + \beta |1\rangle$$

with the condition that $\|\alpha\|^2 + \|\beta\|^2 = 1$. 


