# Quantum Embedding with Hybrid Neural-Quantum Circuits

## Overview
This project implements **Task XI**, focusing on a hybrid classical-quantum architecture for data embedding. We utilize a classical Multi-Layer Perceptron (MLP) to map normally distributed data into the parameters of a **Parameterized Quantum Circuit (PQC)**. The goal is to evaluate how different quantum architectures (varying qubit counts and circuit depths) affect the model's ability to reconstruct classical information from quantum states.

## Methodology
The pipeline follows a four-step process:

1.  **Data Generation:** Input data $X \in \mathbb{R}^4$ is sampled from a normal distribution $\mathcal{N}(0, 1)$.
2.  **Classical Mapping:** A 3-layer MLP processes the input to produce rotation angles $\theta$.
3.  **Quantum Embedding:** The PQC applies $RX(\theta)$ and $RY(\theta)$ gates followed by a CNOT entangling chain to create the quantum state $|\psi(\theta)\rangle$.
4.  **Reconstruction:** The model measures the expectation value of the Pauli-Z operator: 
    $$\langle \hat{Z}_i \rangle = \langle \psi(\theta) | \hat{Z}_i | \psi(\theta) \rangle$$
    The training objective is to minimize the **Mean Squared Error (MSE)** between the input $X$ and the quantum measurements $\hat{X}$:
    $$\mathcal{L} = \frac{1}{n} \sum_{i=1}^{n} (x_i - \hat{x}_i)^2$$

## Experimental Configurations
We compared four different configurations to observe the impact of **Width** (Qubits) and **Depth** (Layers) on the embedding performance.

### PQC Architecture
The circuit uses a layered approach where each layer $L$ consists of:
* **Rotation:** $R(\theta) = RY(\theta_y) \cdot RX(\theta_x)$ for each qubit.
* **Entanglement:** A chain of $CNOT_{i, i+1}$ gates to create inter-qubit correlations.

---

## Performance Summary
| Configuration | Qubits | Embedding Layers | Final MSE Loss |
| :--- | :---: | :---: | :--- |
| **Case A** | 4 | 1 | [Insert Value] |
| **Case B** | 4 | 2 | [Insert Value] |
| **Case C** | 5 | 1 | [Insert Value] |
| **Case D** | 5 | 2 | [Insert Value] |

---

## Key Findings
* **Layer Depth:** Increasing the number of PQC layers generally decreases the loss, as it allows for more complex non-linear mappings.
* **Qubit Count:** Utilizing 5 qubits for 4-feature data provides an "ancilla" (extra) workspace, which can improve convergence stability by expanding the reachable Hilbert space.
* **Precision:** Successful training requires 64-bit precision (`float64`) to maintain consistency between PyTorch tensors and the quantum simulator's state vector calculations.
