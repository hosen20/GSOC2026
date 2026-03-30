# Task V: Quantum Graph Neural Network (QGNN)

## 1. Task Description
The objective is to classify **Quark jets** vs. **Gluon jets** using a Quantum Graph Neural Network. Jets are treated as graphs where:
* **Nodes:** Individual particles with physical features (momentum, coordinates).
* **Edges:** Connections between particles based on their spatial proximity (e.g., k-NN).

## 2. QGNN Circuit Architecture
The circuit is designed to take advantage of the graph representation of particle data:

* **Node Encoding:** Classical features (like $p_T$) are mapped into quantum states using **RY rotation gates** on individual qubits.
* **Edge Interaction:** For every edge in the graph, we apply a **CNOT-RZ-CNOT** sequence. This entangles the qubits, allowing information to "flow" between connected particles, mimicking the message-passing of a classical GNN.
* **Quantum Pooling:** Instead of measuring all qubits, we measure the **expectation value $\langle Z \rangle$ of a single qubit**. Through entanglement, this single qubit aggregates information from the entire graph.



## 3. Classification Process
The circuit outputs a **continuous float** (Expectation Value) between **-1.0 and 1.0**. 

### From Float to Class:
Because we are performing binary classification, we apply a **decision boundary**:
* **Result > 0:** Classified as **Gluon**.
* **Result ≤ 0:** Classified as **Quark**.

The "float" result represents the model's confidence; a value closer to the extremes (-1 or 1) indicates higher certainty.

## 4. Implementation Notes
The provided code demonstrates a **forward pass** (inference). It initializes the circuit with features and variational weights to produce a prediction. A full training phase was not executed here, as that would require an iterative classical optimizer (like Adam) to tune the weights over thousands of dataset samples until the output floats match the target labels.
