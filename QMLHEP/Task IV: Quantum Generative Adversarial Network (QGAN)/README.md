# QGAN with PCA Dimensionality Reduction

This implementation is a Quantum Generative Adversarial Network (QGAN) developed in **TensorFlow Quantum (TFQ)**, adapted from the [PennyLane QGAN Tutorial](https://pennylane.ai/qml/demos/tutorial_QGAN).

## Overview
The project processes a 5-parameter dataset and trains a quantum generator to mimic its distribution. Because the quantum "real" circuit is designed for 3 parameters, PCA is used for dimensionality reduction.

## Key Features
- **Data Loading:** Loads `QIS_EXAM_200Events.npz` and extracts the 5D training features.
- **PCA Transformation:** Strictly reduces the 5 input parameters to 3 principal components to fit the quantum circuit architecture.
- **Quantum Circuits:** - **Real Circuit:** Encodes PCA-reduced data into a quantum state.
  - **Generator:** A 9-parameter PQC that learns to "fake" the real data distribution.
  - **Discriminator:** A PQC that classifies inputs as Real (1) or Fake (0).
- **Minimax Optimization:** Uses the Adam optimizer to update weights for both the Generator and Discriminator.

## Evaluation Metrics
After training, the model calculates:
1. **Accuracy:** How often the discriminator correctly identifies the source.
2. **AUC Score:** The Area Under the ROC Curve, measuring the discriminator's diagnostic ability.
