---
title: Autoregressive Transformer
aliases: [causal transformer, decoder-only transformer, next-token prediction, GPT, autoregressive model]
updated: 2026-11-17
sources: [2025-world-and-human-action-models]
---

## One-line definition

A sequence model that predicts each next token from only the tokens before it, then appends the result to the sequence and keeps generating.

## Explanation

A transformer uses attention to let every position in a sequence refer to other positions. The autoregressive version adds a causal mask, so each position can see only what comes before it; it is also called a causal transformer. The GPT family uses this architecture.

In training, the model is given a sequence and asked to predict the next token at every position, with a cross-entropy loss measuring the error. In generation, one token is sampled from the probability distribution the model outputs, appended to the sequence, and the next one is predicted. Because of sampling, the same beginning can lead to different continuations.

This kind of model is not limited to text. As long as data can be turned into discrete tokens, images, audio, or actions can be handled the same way.

The main limitation is context length: the model sees only a fixed number of the most recent tokens, and anything earlier does not affect the prediction.

## What each source says

### World and Human Action Models towards gameplay ideation (2025)

[This paper](../sources/2025-world-and-human-action-models.md) uses an autoregressive transformer as its sequence prediction backbone (p.3).

- **Input sequence**: frame tokens (obtained through [vector quantization](vector-quantization.md)) interleaved with controller action tokens. There is no delimiter in the sequence; the model has to infer from position embeddings whether a frame or an action should be predicted next (Fig. 2, p.4).
- **Implementation**: a GPT-2 architecture based on a modified nanoGPT, trained for next-token prediction with a cross-entropy loss (p.10).
- **Scale**: the largest model has 1.6B parameters and a context length of 5,560 tokens, which equals 1 second or 10 frames, and is trained for 200k updates (p.10).
- **Prompting**: tokens for frames or actions can be replaced during generation. This is how generation is controlled through controller actions, and how the model continues from a frame the user has edited (p.3).

The authors relate the choice of architecture to the three capabilities (p.3). Consistency needs a sequential model that captures the dependencies between frames and actions. Diversity needs a model that preserves the conditional distribution in the data. Persistency needs a model that can be conditioned on modified frames or actions. A further reason is scalability: the model should benefit from more data and compute.

The paper also analyses scale in the way that is done for large language models. It trains models of different sizes, observes how the loss changes with compute, and extrapolates from the 15M to 206M models to predict the final loss of the 894M model accurately (p.10).

## Related concepts

- [Vector Quantization](vector-quantization.md): turns frames into discrete tokens that this model can process.
- [World Model](world-model.md): here, the transformer is the part of the world model that predicts the future.
- [Creativity Support](creativity-support.md): sampling gives diversity, and replacing tokens makes the output modifiable.
