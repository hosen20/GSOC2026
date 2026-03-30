# Task VI: Quantum Representation Learning

This project implements a **Quantum Representation Learning** scheme using a contrastive loss approach to embed MNIST images into a quantum Hilbert space. The goal is to learn a parametric quantum embedding where images of the same class exhibit high fidelity, while images of different classes are pushed toward orthogonality.

## Concept: Quantum Contrastive Learning

Quantum Representation Learning shifts the focus from direct classification to the geometry of the state space. By training a quantum circuit to act as a "feature extractor," we map classical data into quantum states $|\psi(x)\rangle$. 

The success of the representation is measured by **Fidelity**:
* **Positive Pairs:** Two different images of the same digit should result in $|\langle \psi_{same} | \psi_{same} \rangle|^2 \approx 1$.
* **Negative Pairs:** Images of different digits should result in $|\langle \psi_{diff} | \psi_{diff} \rangle|^2 \approx 0$ (or the physical limit of the SWAP test).

## Our Solution

### 1. PCA Preprocessing
To handle the 784-pixel dimensionality of MNIST with a limited number of qubits, we utilize **Principal Component Analysis (PCA)**. We reduce each image to its top 6 principal components. This ensures the quantum circuit receives the most statistically significant features (the core shapes of the digits) rather than redundant background pixels.



### 2. Parametric Quantum Embedding
The model uses a hardware-efficient ansatz consisting of:
* **Angle Embedding:** Initial $RY$ and $RZ$ rotations to encode PCA components into qubit phases.
* **Trainable Layers:** Multiple layers of parameterized $RY$ gates and CNOT entanglers. These parameters are optimized to "rotate" the representations of similar classes into the same region of the Hilbert space.

### 3. The SWAP Test
We implement a **SWAP Test** circuit to calculate the fidelity between two representations without performing full state tomography. By using an ancilla qubit and a series of Controlled-SWAP (Fredkin) gates, the overlap of the two states is directly mapped to the expectation value of the ancilla qubit.



### 4. Balanced Sampling & Margin Loss
To ensure stable convergence across all 10 MNIST classes, we implement:
* **Balanced Pair Sampling:** Each training batch is manually constructed to contain a 50/50 split of "same-class" and "different-class" pairs.
* **Contrastive Margin Loss:** We use a hinge-loss style margin. This forces the model to ignore pairs that are already "far enough" apart in the quantum space, preventing the gradient from being dominated by easy-to-separate examples and focusing the learning on harder digit distinctions.
