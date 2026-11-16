---
title: Vector Quantization
aliases: [VQ, VQ-VAE, VQGAN, ViT-VQGAN, discrete token, image tokenizer, codebook]
updated: 2026-11-17
sources: [2025-world-and-human-action-models]
---

## One-line definition

Replacing a continuous vector with the closest entry in a finite codebook, so that data can be represented as a sequence of discrete indices.

## Explanation

Language models work on discrete tokens, but an image is made of continuous pixels. Vector quantization is a common way to turn images into tokens as well.

The process is as follows. An encoder first compresses the image into a small grid of vectors. Each vector is then replaced by the closest entry in a codebook, and only the index of that entry is kept. An image thus becomes a sequence of integers. A decoder reconstructs an approximate image from these indices. The size of the codebook is the "vocabulary size".

VQ-VAE is the standard example of this approach. VQGAN adds a perceptual loss and an adversarial loss during training, so that reconstructed images still look sharp at high compression.

The main trade-off is the number of tokens. More tokens per image give better reconstruction, but the sequence gets longer, the downstream model gets slower, and fewer images fit in the same context length.

## What each source says

### World and Human Action Models towards gameplay ideation (2025)

[This paper](../sources/2025-world-and-human-action-models.md) uses a VQGAN to turn each game frame into discrete tokens. The authors describe framing the data as a sequence of discrete tokens as critical to their approach (p.3).

The paper trains two encoders (p.10):

- Smaller models (15M–894M): a VQGAN of about 60M parameters that encodes a 128 × 128 frame into 256 tokens, with a vocabulary of 4,096.
- The 1.6B model: a ViT-VQGAN of about 300M parameters that encodes a frame at the native 300 × 180 resolution into 540 tokens, with a vocabulary of 16,384.

Both are first trained with a reconstruction loss and a perceptual loss, and then with a GAN loss as well (p.10).

The authors point out that the number of tokens per frame is a key hyperparameter that trades off image quality against generation speed and context length (p.3). Both sides of the trade-off are visible in the paper:

- The 1.6B model uses a higher resolution, so the ceiling on reconstruction quality is higher, and its consistency metric, FVD, is better (p.4).
- The same model is slightly worse on action diversity. The authors hypothesize that more image tokens and a larger vocabulary reduce the weight of the action tokens in the loss (p.5).

Controller actions are discretized too: buttons are already discrete, and joystick coordinates are split into 11 buckets (p.3). This step is direct bucketing and does not use a codebook.

## Related concepts

- [Variational Autoencoder](variational-autoencoder.md): also compresses an image into a compact representation, but produces a continuous vector.
- [Autoregressive Transformer](autoregressive-transformer.md): takes these discrete tokens and predicts the next one.
- [World Model](world-model.md): here, vector quantization handles the part of the world model that compresses frames.
