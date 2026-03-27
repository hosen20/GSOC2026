# Kolmogorov-Arnold Network (KAN) Project

This repository contains an implementation of a **Kolmogorov-Arnold Network (KAN)** applied to the MNIST digit classification task, along with a conceptual framework for extending this architecture into the quantum computing domain.

---

## Task IX: Kolmogorov-Arnold Network (MNIST & Quantum Extension)

### 1. Classical KAN Implementation
A classical Kolmogorov-Arnold Network was implemented using B-spline basis functions on the edges of the network. Unlike a standard Multi-Layer Perceptron (MLP) that uses fixed activation functions on nodes, this architecture utilized learnable univariate functions on every connection.

#### Mathematical Framework
The network followed the Kolmogorov-Arnold representation theorem:

$$f(x_1, \dots, x_n) = \sum_{q=1}^{2n+1} \Phi_q \left( \sum_{p=1}^n \phi_{q,p}(x_p) \right)$$

**Implementation Details:**
* **Basis Functions:** Linear B-splines (hat functions) were used to approximate the functions.
* **Residual Connections:** A SiLU (Sigmoid Linear Unit) base path was included to stabilize training.
* **Optimization:** The model was trained on the MNIST dataset using the AdamW optimizer.

---

### 2. Transition to Quantum KAN (QKAN)
The classical architecture was extended conceptually into the quantum domain. In a **Quantum Kolmogorov-Arnold Network (QKAN)**, the learnable B-spline functions on the edges were replaced by **Parametrized Quantum Circuits (PQCs)**.

#### Quantum Architecture Sketch
The proposed QKAN architecture followed a hybrid quantum-classical design:

1. **Data Encoding:** Classical input pixels were mapped into a quantum Hilbert space using **Angle Encoding**:
   $$|\psi(x)\rangle = R_y(x)|0\rangle$$
2. **Learnable Quantum Edges:** Each edge $(i, j)$ consisted of a PQC with trainable rotation gates ($R_x, R_z$) and CNOT entangling gates.
3. **Measurement:** The output was derived by measuring the expectation value of the Pauli-Z operator:
   $$y_{i,j} = \langle \psi(x_i, \theta) | \hat{Z} | \psi(x_i, \theta) \rangle$$

---

### 3. Potential Extensions and Advantages
* **Exponential Expressivity:** Leveraging $N$ qubits allows the network to operate in a $2^N$ dimensional feature space.
* **Quantum Entanglement:** Multi-qubit gates allow the network to evolve features collectively.
* **Kernel Hilbert Space:** The PQC functions as a learnable quantum kernel.
