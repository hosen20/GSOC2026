# Semi-Supervised Symmetry Discovery (MNIST Rotations)

This project implements a simple pipeline for discovering rotational symmetries in MNIST using latent representations. The main idea is to first learn a compact latent space with a Variational Autoencoder (VAE), and then study how rotations act inside that space.

We restrict the dataset to digits **1 and 2**, and generate rotated versions in steps of **30 degrees** to explicitly introduce a known symmetry group.

---

## 1. Dataset Construction

We start from MNIST and filter only two classes:

\[
\mathcal{D} = \{(x_i, y_i)\;|\; y_i \in \{1,2\}\}
\]

Each image is then rotated by angles:

\[
\theta \in \{0^\circ, 30^\circ, 60^\circ, \dots, 330^\circ\}
\]

This creates a structured dataset:

\[
x_i^\theta = R_\theta(x_i)
\]

where \(R_\theta\) is a 2D rotation operator.

---

## 2. Latent Space Learning (VAE)

A Variational Autoencoder is used to encode images into a low-dimensional latent space:

\[
z \sim q_\phi(z|x), \quad z \in \mathbb{R}^d
\]

with encoder:

\[
(\mu, \log \sigma^2) = f_\phi(x)
\]

and sampling:

\[
z = \mu + \epsilon \cdot \sigma, \quad \epsilon \sim \mathcal{N}(0, I)
\]

The decoder reconstructs the image:

\[
\hat{x} = g_\theta(z)
\]

The loss function is:

\[
\mathcal{L}_{VAE} = \|x - \hat{x}\|^2 + \beta \cdot KL(q_\phi(z|x) \| \mathcal{N}(0,I))
\]

This step ensures that rotations are encoded into a smooth latent representation.

---

## 3. Supervised Symmetry Learning

We model rotation as a transformation in latent space:

\[
z_{\theta+30^\circ} \approx T_\psi(z_\theta, \theta)
\]

where \(T_\psi\) is a small MLP that takes the latent vector and the rotation angle.

Training objective:

\[
\mathcal{L}_{sym} = \|T_\psi(z_\theta, \theta) - z_{\theta+30^\circ}\|^2
\]

This learns how rotation acts as a continuous transformation in latent space.

---

## 4. Unsupervised Symmetry Discovery

To discover symmetries without explicit pairing, we learn a transformation:

\[
z' = S_\psi(z)
\]

A valid symmetry should preserve the semantics of the input. We enforce this using two constraints:

### (a) Reconstruction consistency
\[
\|g(z) - g(S_\psi(z))\|^2
\]

### (b) Logit invariance (classifier constraint)

A pretrained classifier \(C(\cdot)\) is used:

\[
C(g(z)) \approx C(g(S_\psi(z)))
\]

Final loss:

\[
\mathcal{L} = \mathcal{L}_{recon} + \lambda \cdot \mathcal{L}_{logit}
\]

This encourages the learned transformation to behave like a true symmetry of the data distribution.

---

## 5. Key Idea

The whole pipeline is based on the assumption:

\[
\text{Symmetry in input space} \;\Rightarrow\; \text{Linear/structured transformation in latent space}
\]

By learning this structure, we can approximate rotation operators and study invariances in a data-driven way.

---

## 6. Outputs and Visualizations

The code produces:

- reconstructed MNIST digits from VAE
- latent space visualization (PCA projection)
- learned rotation results in image space
- logit consistency check under transformations

These help verify whether the learned transformations behave consistently with true rotational symmetry.

---

## 7. Summary

This project demonstrates:

- how VAEs learn structured latent spaces
- how rotations become smooth transformations in latent space
- how supervised and unsupervised methods can recover symmetry operators
- how invariance constraints (reconstruction + logits) enforce meaningful transformations

The approach is simple but captures the core idea of symmetry discovery in generative latent models.
