# Task VII: Equivariant Quantum Neural Networks ($Z_2 \times Z_2$)

This project implements and compares a standard Quantum Neural Network (QNN) against a **$Z_2 \times Z_2$ Equivariant Quantum Neural Network (EQNN)**. The goal is to solve a classification problem where the dataset respects a specific permutation symmetry: mirroring along the line $y=x$.

## Implementation Overview

Our solution focuses on enforcing **Permutation Symmetry** ($S_2$) directly within the quantum circuit architecture. The dataset consists of two features, $x_1$ and $x_2$, where the label is invariant if the features are swapped. 

To achieve equivariance, the EQNN is constructed using two primary constraints:
1. **Parameter Sharing:** We apply the same trainable rotation angles to both qubits simultaneously. This ensures the model cannot "favor" one input feature over the other.
2. **Symmetric Entanglers:** We utilize gates like the **CZ gate** which are inherently symmetric (the gate matrix commutes with the SWAP operator), maintaining the $Z_2$ symmetry throughout the state evolution.



## Results and Analysis

The comparison between the two models highlights the power of **Geometric Deep Learning** in quantum computing:

### Equivariant QNN (Symmetric)
The decision boundary for the EQNN is a clean, mirrored "checkerboard." Because the symmetry is "hard-coded" into the circuit, the model is mathematically incapable of producing an asymmetric result. This leads to:
* **High Confidence:** Indicated by the deep, solid Red and Blue regions.
* **Better Generalization:** The model understands the global rule ($x_1 \cdot x_2 > 0$) even in areas with sparse training data.

### Standard QNN
The Standard QNN lacks these geometric constraints, resulting in a "messy" decision boundary. Since it treats $x_1$ and $x_2$ as independent variables, it must attempt to learn the symmetry from scratch. This leads to:
* **Asymmetric Over-fitting:** The red and blue regions are jagged and do not mirror each other across the $y=x$ axis.
* **Lower Reliability:** The faded colors indicate the model is "unsure" in regions where it hasn't seen enough specific data points.



**Conclusion:** By using an Equivariant architecture, we significantly reduce the parameter search space and prevent the model from considering "impossible" (asymmetric) solutions. This makes the EQNN more data-efficient and robust than its standard counterpart.
