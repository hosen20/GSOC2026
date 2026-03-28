# Semi-Supervised Symmetry Discovery (MNIST Rotations)

This project implements a simple pipeline for discovering rotational symmetries in MNIST using latent representations. The main idea is to first learn a compact latent space with a Variational Autoencoder (VAE), and then study how rotations act inside that space.

We restrict the dataset to digits **1 and 2**, and generate rotated versions in steps of **30 degrees** to explicitly introduce a known symmetry group.

---

## 1. Dataset Construction

We start from MNIST and filter only two classes:

$$
\mathcal{D} = \{(x_i, y_i) \mid y_i \in \{1,2\}\}
$$

Each image is then rotated by angles:

$$
\theta \in \{0^\circ, 30^\circ, 60^\circ, \dots, 330^\circ\}
$$

This creates a structured dataset where $x_i^\theta = R_\theta(x_i)$ and $R_\theta$ is a 2D rotation operator.

---

## 2. Latent Space Learning (VAE)

A Variational Autoencoder (VAE) encodes images into a low-dimensional latent space:

$$z \sim q_{\phi}(z|x), \quad z \in \mathbb{R}^d$$

- **Encoder:** $(\mu, \log \sigma^2) = f_{\phi}(x)$
- **Sampling:** $z = \mu + \sigma \odot \epsilon, \quad \epsilon \sim \mathcal{N}(0, I)$
- **Decoder:** $\hat{x} = g_{\omega}(z)$

The loss function ensures reconstructions are accurate and the latent space is regularized:

$$\mathcal{L}_{VAE} = \Vert x - \hat{x} \Vert^2 + \beta \cdot D_{KL}(q_{\phi}(z|x) \Vert \mathcal{N}(0,I))$$

---

## 3. Supervised Symmetry Learning

We model rotation as a transformation $T_{\psi}$ (the "Rotator") in latent space:

$$
z_{\theta+30^\circ} \approx T_{\psi}(z_{\theta}, \theta)
$$

**Training objective:**

$$
\mathcal{L}_{\text{sym}} =
\left\|T_{\psi}(z_{\theta}, \theta) - z_{\theta+30^\circ}\right\|^2
$$

This learns how rotation acts as a continuous transformation within the latent space.

---

## 4. Unsupervised Symmetry Discovery

To discover symmetries without explicit pairing, we learn a transformation:

$$
z' = S_{\psi}(z)
$$

A valid symmetry must preserve semantics via two constraints:

### (1) Reconstruction consistency

$$
\mathcal{L}_{\text{recon}} =
\left\|g_{\omega}(z) - g_{\omega}(S_{\psi}(z))\right\|^2
$$

### (2) Logit invariance

Let \(C(\cdot)\) be a pretrained classifier:

$$
C(g_{\omega}(z)) \approx C(g_{\omega}(S_{\psi}(z)))
$$

We enforce this as:

$$
\mathcal{L}_{\text{logit}} =
\left\|C(g_{\omega}(z)) - C(g_{\omega}(S_{\psi}(z)))\right\|^2
$$

**Final loss:**

$$
\mathcal{L} =
\mathcal{L}_{\text{recon}} +
\lambda \cdot \mathcal{L}_{\text{logit}}
$$

---

## 5. Visualizing the Latent Space (PCA)

Using PCA to reduce the latent space to 2D reveals how the model encodes rotation:

- **Clustering:** Points with similar rotation angles form structured regions in latent space.  
- **Continuity:** Smooth transitions between colors show that rotations are learned as continuous transformations.  
- **Topology:** The circular pattern indicates the model has implicitly learned the periodic nature of rotation (mod 360°).

---

## 6. Summary

This project demonstrates:

- How VAEs learn structured latent representations from images  
- How rotations become smooth transformations in latent space  
- How supervised learning can recover symmetry operators  
- How unsupervised constraints (reconstruction + logit stability) can discover invariant transformations  
