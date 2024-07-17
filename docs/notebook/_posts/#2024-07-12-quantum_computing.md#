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
1. States
2. Dynamics
3. Measurements
4. Observables

### States
States are made up of qubits which are complex vectors in a Hilbert space $\mathcal{H}$ which is just a vector space equiped with an inner product. We use Dirac notation to denote the state of these qubits, e.g. $\|\psi \rangle $ with $\|\psi\|^2=1$, and the inner product of two states is $\langle \psi \| \varphi\rangle$. In two dimensions, a basis can be formed with the states $\| 0 \rangle$ and $\| 1\rangle$ and the other states can be written as a complex linear combination of this basis. Hence, for any $\| \psi \rangle \in \mathcal{H}$,

$$| \psi \rangle = \alpha |0\rangle + \beta |1\rangle$$

with the condition that $\|\alpha\|^2 + \|\beta\|^2 = 1$.

### Dynamics
Dynamics describe how the quantum system changes over time. They are usually characterized by gates which are linear transformations of the states. Some common gates are the Pauli matrices:

$$X=\begin{bmatrix}0 & 1\\ 1 & 0\end{bmatrix}, Y=\begin{bmatrix}0 & -i\\ i & 0\end{bmatrix}, Z = \begin{bmatrix}1 & 0\\ 0 & -1\end{bmatrix}$$

Quantum gates are unitary and hermitian. Another important matrix is the Hadmard transformation

$$H=\frac{1}{\sqrt{2}}\begin{bmatrix}1 & 1\\ 1 & -1 \end{bmatrix}$$

which transforms the orthanormal standard basis to another orthanormal basis $H\|0\rangle =\|+\rangle$, $H\|1\rangle =\|-\rangle$.

### Measurements
Measurements are the way by which observations are made about a quantum system. They are described by a positive operator-valued measure (POVM) whose values are positive semi-definite operators. In other words, with our Hilbert space $\mathcal{H}$, we associate a measure $ \{ M_i \} $ with the property

$$\sum M_i = I.$$

When a state $\|\psi\rangle$ is measured, an outcome is determined with some probability. The probability is determined via Born's rule:

$$p_i = \langle \psi | M_i | \psi \rangle$$

We can verify that this is indeed a probability distribution as each probability is nonnegative and

$$
\begin{align*}
\sum p_i &= \sum \langle \psi | M_i | \psi \rangle\\
&= \langle \psi | \sum M_i |\psi \rangle\\
&= \langle \psi | I | \psi \rangle\\
&= |\psi|^2\\
&= 1
\end{align*}
$$

### Observables
Observables are physical properties that can be observed about a system. They are represented by self-adjoint operators on the system. We write 

$$A = A^{t} = \sum_i a_i M_i$$

for some coefficients $a_i\in \mathbb{R}$ and $M_i$ the set of POVMs. We write the expectation of $A$ with respect to sum state $\| \psi \rangle$ as

$$\langle A\rangle_\psi = \langle \psi | A |\psi\rangle = \langle \psi |\sum_i a_i M_i|\psi \rangle = \sum_i a_ip_i.$$

## Single Qubit

### Experiment

We write the standard normal basis vectors in two dimensions

$$|0 \rangle = \begin{pmatrix}1 \\ 0\end{pmatrix}, |1\rangle = \begin{pmatrix} 0 \\ 1 \end{pmatrix}.$$

For any state $\|\psi \rangle \in \mathcal{H}_2=\text{span} (\|0\rangle, \|1\rangle)$ we can write

$$|\psi \rangle = \alpha |0\rangle + \beta |1\rangle$$

for complex $\alpha, \beta$ with $\|\alpha\|^2 + \|\beta\|^2=1$. With this condition, we can parameterize $\alpha = e^{ia}\cos \frac{\theta}{2}$ and $\beta = e^{ib}\sin\frac{\theta}{2}$ for real $a,b,\theta$. Furthermore, we can observe that if $\|\tilde{\psi}\rangle = e^{i\gamma}\|\psi\rangle$, then for some POVM $M$, 

$$\langle \tilde{\psi} | M | \tilde{\psi}\rangle = \langle \psi | M | \psi \rangle$$

so $\|\psi \rangle$ and $\|\tilde{\psi}\rangle$ are indistinguishable as far as we can measure. In this sense, they are the same, so we can write $\phi=b-a$. Hence, 

$$|\psi \rangle = \cos \frac{\theta}{2} + e^{i\phi}\sin\frac{\theta}{2}.$$

### Transformations

As we said before, transformations are characterized by unitary linear operators called quantum logic gates. 



