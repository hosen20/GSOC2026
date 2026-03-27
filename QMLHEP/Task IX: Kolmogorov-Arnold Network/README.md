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
To extend the classical architecture conceptually into the quantum domain. In a **Quantum Kolmogorov-Arnold Network (QKAN)**, the learnable B-spline functions on the edges should be replaced by **Parametrized Quantum Circuits (PQCs)**.

#### Quantum Architecture Sketch
The proposed QKAN architecture follows a hybrid quantum-classical design:

1. **Data Encoding:** Classical input image pixels will mapped into a quantum Hilbert space using **Angle Encoding**:
   $$|\psi(x)\rangle = R_y(x)|0\rangle$$
2. **Learnable Quantum Edges:** Each edge $(i, j)$ consists of a PQC with trainable rotation gates ($R_x, R_z$) and CNOT entangling gates.
3. **Measurement:** The output will derived by measuring the expectation value of the Pauli-Z operator:
   $$y_{i,j} = \langle \psi(x_i, \theta) | \hat{Z} | \psi(x_i, \theta) \rangle$$

---

### 3. Potential Extensions and Advantages
* **Exponential Expressivity:** Leveraging $N$ qubits allows the network to operate in a $2^N$ dimensional feature space.
* **Quantum Entanglement:** Multi-qubit gates allow the network to evolve features collectively.
* **Kernel Hilbert Space:** The PQC functions as a learnable quantum kernel.

---

### 4. Detailed Hybrid QKAN Architecture

The table below provides a granular breakdown of the architecture, highlighting the interaction between Classical (CPU/GPU) and Quantum (QPU/Simulator) resources:

| Pipeline Stage | Implementation Detail | Execution Environment | Role in KAN |
| :--- | :--- | :--- | :--- |
| **Data Loading** | MNIST Pixel Normalization $\mu, \sigma$ | **Classical** | Pre-processing |
| **Input Encoding** | Angle Embedding $\vert \psi(x) \rangle = R_y(x) \vert 0 \rangle$ | **Hybrid** | Basis Expansion |
| **Edge Function** | Parametrized Unitary Evolution $U(\theta)$ | **Quantum** | Learnable $\phi_{i,j}$ |
| **Entanglement** | CNOT Gates between Feature Qubits | **Quantum** | Multivariate Correlation |
| **Measurement** | Pauli-Z Expectation $\langle \hat{Z} \rangle$ | **Hybrid** | Quantum-to-Classical Sink |
| **Node Summation** | $y_j = \sum_{i} \text{Measurement}_i$ | **Classical** | Kolmogorov-Arnold Sum |
| **Loss Function** | Cross-Entropy Calculation | **Classical** | Objective Evaluation |
| **Backpropagation** | Parameter Update $\theta_{new} = \theta - \eta \nabla L$ | **Classical** | Hybrid Optimization |

---

### 5. Hybrid Quantum-Classical Flow

The architecture functions as a **Hybrid Quantum-Classical Loop**. While the non-linear transformations will be offloaded to the quantum Hilbert space, the structural integrity of the Kolmogorov-Arnold Network will be maintained by a classical controller.

#### The Hybrid Function Mapping
Each output neuron $y_j$ in the QKAN layer will be calculated as:

$$y_j = \text{Aggregate}_{i=1}^{n} \left( \langle 0 \vert U^\dagger(x_i, \theta_{i,j}) \hat{Z} U(x_i, \theta_{i,j}) \vert 0 \rangle \right)$$

This approach allows the model to utilize the **exponential expressivity** of quantum states for feature extraction while leveraging **standard gradient descent** (AdamW) on a classical processor to update the circuit parameters.
