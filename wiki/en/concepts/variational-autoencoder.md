---
title: Variational Autoencoder
aliases: [VAE, autoencoder, latent vector, latent space]
updated: 2026-11-16
sources: [2018-world-models]
---

## One-line definition

A neural network that learns to compress an input into a small latent vector and to reconstruct an approximation of the original input from that vector.

## Explanation

An autoencoder has two parts. The encoder compresses an input, such as an image, into a low-dimensional vector, and the decoder restores the input from that vector. The training objective is to make the reconstruction as close as possible to the original. Because the vector in the middle is small, the network is forced to keep only the most important information.

A variational autoencoder (VAE) adds one requirement: the latent vector should follow a simple probability distribution. This makes the latent space smoother and more regular, so nearby vectors decode to similar results, and new content can be produced by sampling from the distribution.

Training a VAE needs no labels, so it is unsupervised learning. It is often used to turn high-dimensional data such as images into a compact representation that later models can handle more easily. The compression is lossy, and details are lost.

## What each source says

### World Models (2018)

[This paper](../sources/2018-world-models.md) uses a VAE as the agent's vision component V, compressing every game frame into a latent vector z (p.2–3). z has 32 dimensions in the car racing experiment and 64 in the Doom experiment (p.5, p.7).

- The training data comes from 10,000 rollouts collected with a random policy, and the objective is to make the reconstructed frame close to the original (p.4).
- The authors note that although details are lost in compression, z still captures the essence of each frame (p.5).
- V is trained separately from the predictive model M. The authors say that in principle both could be trained together end to end, but training them separately is more practical and gives satisfactory results (p.4).

The paper also points out a limitation of this approach: the VAE is trained as a standalone model and does not know which information is useful for the task. It reproduced unimportant brick tile patterns on the walls in Doom but failed to reproduce task-relevant tiles on the road in car racing. Training it together with a model that predicts rewards could help, but the trade-off is that it may need retraining for new tasks (p.12).

## Related concepts

- [World Model](world-model.md): here, the VAE is the vision part of the world model.
