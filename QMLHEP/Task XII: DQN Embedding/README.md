# Hybrid Quantum-Classical Reinforcement Learning (QRL)

This project implements a **Hybrid Action-Controlled Quantum Model** trained via **Deep Q-Learning (DQN)**. The architecture explores how a classical reinforcement learning agent can learn to manipulate the internal parameters of a Variational Quantum Circuit (VQC) to minimize reconstruction error.

---

## 1. Architecture Overview

The system consists of three primary components working in a closed loop:

1.  **The Hybrid Model:** A classical MLP generates base parameters for a quantum circuit. These parameters are modified by an external `shift` before being passed to the quantum gates ($RX$, $RY$, and $CNOT$ entanglers).
2.  **The Environment:** A custom OpenAI Gym-style environment (`QuantumEnv`) that provides data observations, executes the model, and calculates rewards based on the Mean Squared Error (MSE).
3.  **The Q-Network:** A Deep Q-Network (DQN) that observes the quantum state outputs and decides whether to increase, decrease, or maintain the `shift` value to optimize performance.

---

## 2. Mathematical Foundation

### Quantum Circuit Evolution
The quantum state $|\psi\rangle$ is transformed by a series of unitary rotations $U(\theta)$ defined by the MLP output and the RL agent's action:

$$\theta_{final} = \text{MLP}(x) + \text{shift}_{action}$$

The expectation value for each qubit $i$ is measured using the Pauli-Z operator:
$$\langle \hat{Z}_i \rangle = \langle \psi | U^\dagger(\theta) \hat{Z}_i U(\theta) | \psi \rangle$$

### Reinforcement Learning Objective
The agent aims to maximize the discounted cumulative reward. The **Bellman Equation** used for updating the Q-values is:

$$Q(s, a) \approx r + \gamma \max_{a'} Q_{target}(s', a')$$

Where:
* $r$ is the reward (defined as $-MSE$).
* $\gamma$ is the discount factor ($0.95$).
* $s'$ is the next state.

---

## 3. Experimental Results

The model was tested across different circuit depths and qubit counts. The following table summarizes the average loss (MSE) achieved at the end of training for four specific configurations:

| Configuration | Qubits ($n$) | Layers ($L$) | Final Avg Loss (MSE) | (MSE) previous neural network task |
| :--- | :---: | :---: | :---: | :---: }
| **Case A** | 4 | 1 | 0.005236 | 0.586 |
| **Case B** | 4 | 2 | 0.038882 | 0.336 |
| **Case C** | 8 | 1 | 0.031425 | 0.557 |
| **Case D** | 8 | 2 | 0.036163 | 0.361 |

> **Pro-Tip:** Increasing the number of layers ($L$) generally improves the "expressivity" of the quantum circuit "but not always or in all runs", allowing the RL agent to find more precise shifts to match the input data $X$.

---

## 4. Implementation Details

### Action Space
The agent chooses from three discrete actions that modify the `shift` parameter:
* **Action 0:** `shift = shift - 0.05`
* **Action 1:** No change
* **Action 2:** `shift = shift + 0.05`

### Training Hyperparameters
* **Optimizer:** Adam (Learning Rate = $10^{-3}$)
* **Exploration:** Epsilon-greedy ($\epsilon$ starts at 0.5, decays by 0.98 per episode)
* **Memory:** Replay buffer size of 2000 transitions.
* **Batch Size:** 32.

---

## 5. Usage

To run the experiment and generate the loss history, call the `run_experiment` function:

```python
# Example execution
history = run_experiment(n_qubits=4, n_layers=2)
