# Task VIII: Quantum Vision Transformer (QViT) Overview

This document outlines the transition from a classical **Vision Transformer (ViT)** to a **Quantum Vision Transformer (QViT)**. While the classical ViT uses self-attention to find global dependencies in image patches, the QViT leverages the high-dimensional Hilbert space of quantum mechanics to perform these computations.

---

## 1. Classical vs. Quantum: Structural Comparison

The table below summarizes the fundamental differences in how image data is processed between the two architectures.

| Feature | Classical Vision Transformer (ViT) | Quantum Vision Transformer (QViT) |
| :--- | :--- | :--- |
| **Data Representation** | 1D vectors of pixel intensities. | Quantum states (Superposition of basis states). |
| **Patch Embedding** | Linear Projection ($XW + b$). | **Quantum Feature Map** (Encoding into qubit rotations). |
| **Core Mechanism** | Scaled Dot-Product Self-Attention. | **Quantum Kernel Attention** (State Overlap/Fidelity). |
| **Non-linearity** | MLP with ReLU/GELU activations. | **Variational Quantum Circuits (VQC)** with entanglement. |
| **Scaling** | Memory scales with $N^2$ (number of patches). | Potential for exponential state representation. |
| **Output** | Softmax over class logits. | Measurement of qubits in the Z-basis. |

---

## 2. Quantum Architecture Breakdown

The QViT replaces the standard Transformer blocks with **Hybrid Quantum-Classical Layers**. The structure follows these four primary stages:

### I. Data Encoding (The Quantum Feature Map)
Since quantum computers require quantum states as input, we map the 2D MNIST patches into qubit parameters. 
- **Method:** Angle Encoding.
- **Process:** Each normalized pixel value is used as a rotation angle ($\theta$) for a $R_y$ or $R_z$ gate, mapping classical data into the Bloch Sphere.

### II. Quantum Multi-Head Attention (QMHA)
In the classical version, attention is calculated by $Attention = Softmax(QK^T/\sqrt{d})V$. In the QViT:
- **Quantum Queries & Keys:** We apply Parameterized Quantum Circuits (PQCs) to the encoded states to generate Query ($Q$) and Key ($K$) states.
- **Fidelity Measurement:** Instead of a dot product, we use a **Swap Test** or **Hadamard Test** to measure the similarity (overlap) between the $Q$ and $K$ quantum states.

### III. Entangled MLP (Feed-Forward Network)
After the attention scores are calculated, the data passes through a "Quantum MLP":
- **Entanglement:** CNOT gates are used to create correlations between different patches that classical linear layers struggle to mimic.
- **Variational Layers:** Trainable rotation gates act as the "weights" that are optimized during training.

### IV. Measurement and Classification
The final "Class Token" state is measured. The resulting expectation values (probabilities) are sent to a classical Softmax layer to determine if the MNIST digit is 0-9.

---

## 3. Implementation Workflow (Sketch)

1. **Preprocessing:** Resize MNIST images to $28 \times 28$ and divide into $4 \times 4$ patches.
2. **Quantum Circuit Construction:** Define a PQC using `PennyLane` or `Qiskit`.
3. **Hybrid Training:** - Use **PyTorch** for the classical wrapper.
   - Use **Parameter Shift Rules** to calculate gradients through the quantum circuit.
   - Optimize using the **Adam** optimizer.

---

## 4. Why QViT?
By utilizing **Quantum Entanglement**, the QViT can theoretically capture complex, non-local spatial relationships in images more efficiently than classical self-attention, which relies purely on mathematical matrix multiplication.
