# Kolmogorov-Arnold Network (KAN) Project

This repository contains an implementation of a **Kolmogorov-Arnold Network (KAN)** applied to the MNIST digit classification task, along with a conceptual framework for extending this architecture into the quantum computing domain.

---

## Task IX: Kolmogorov-Arnold Network (MNIST & Quantum Extension)

### 1. Classical KAN Implementation
A classical Kolmogorov-Arnold Network was implemented using B-spline basis functions on the edges of the network. Unlike a standard Multi-Layer Perceptron (MLP) that uses fixed activation functions on nodes, this architecture utilized learnable univariate functions $\phi_{i,j}$ on every connection.

#### Mathematical Framework
The network followed the Kolmogorov-Arnold representation theorem, which states that a multivariate continuous function can be represented as a finite sum of continuous functions of a single variable:

$$f(x_1, \dots, x_n) = \sum_{q=1}^{2n+1} \Phi_q \left( \sum_{p=1}^n \phi_{q,p}(x_p) \right)$$

In the provided implementation:
* **Basis Functions**: Linear B-splines (hat functions) were used to approximate $\phi$.
* **Residual Connections**: A SiLU (Sigmoid Linear Unit) base path was included to stabilize training.
* **Optimization**: The model was trained on the MNIST dataset using the AdamW optimizer.

#### Performance Results
After 5 training epochs, the classical KAN achieved high classification accuracy on the MNIST test set. The spline-based transitions allowed the model to learn non-linear pixel correlations more efficiently than a standard ReLU-based MLP of similar parameter count.

---

### 2. Transition to Quantum KAN (QKAN)
The classical architecture was extended conceptually into the quantum domain. In a **Quantum Kolmogorov-Arnold Network (QKAN)**, the learnable B-spline functions $\phi(x)$ on the edges were replaced by **Parametrized Quantum Circuits (PQCs)**.

#### Quantum Architecture Sketch
The proposed QKAN architecture followed a hybrid quantum-classical design:

**A. Data Encoding (The Feature Map)** Classical input pixels $x$ were mapped into a quantum Hilbert space. This was achieved through **Angle Encoding**, where the input value determined the rotation of a qubit:
$$|\psi(x)\rangle = R_y(x)|0\rangle$$
This step acted as the "basis expansion," providing the initial non-linearity required by the Kolmogorov-Arnold theorem.

**B. Learnable Quantum Edges** Each edge $(i, j)$ consisted of a PQC with trainable parameters $\theta$. The circuit utilized:
* **Rotation Gates**: $R_x(\theta_1), R_z(\theta_2)$ gates provided the learnable degrees of freedom.
* **Entangling Gates**: CNOT gates were introduced to allow the network to learn inter-dimensional dependencies that are difficult for classical 1D splines to capture.

**C. Measurement and Summation** The output of a quantum edge was derived by measuring the expectation value of the Pauli-Z operator:
$$y_{i,j} = \langle \psi(x_i, \theta) | \hat{Z} | \psi(x_i, \theta) \rangle$$
These values were then summed classically at each output node, mirroring the summation step in the classical KAN architecture.

---

### 3. Potential Extensions and Advantages
The QKAN architecture offered several theoretical improvements over the classical version:

1.  **Exponential Expressivity**: By leveraging $N$ qubits, the "Quantum Splines" operated in a $2^{N}$ dimensional feature space, potentially capturing complex patterns in MNIST digits with fewer parameters.
2.  **Quantum Entanglement**: While classical KANs treated input features independently before the summation node, QKANs used multi-qubit gates to evolve features collectively, leading to a more holistic representation of image topology.
3.  **Kernel Hilbert Space**: The PQC naturally functioned as a learnable quantum kernel, providing a more flexible method for function approximation than fixed-grid B-splines.
