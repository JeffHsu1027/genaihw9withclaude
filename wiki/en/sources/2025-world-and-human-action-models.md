---
title: World and Human Action Models towards gameplay ideation
authors: Anssi Kanervisto et al.
year: 2025
venue: Nature (2025), doi:10.1038/s41586-025-08600-3
url: https://doi.org/10.1038/s41586-025-08600-3
raw: raw/World and Human Action Models towards gameplay ideation.pdf
added: 2026-11-17
concepts: [creativity-support, world-model, vector-quantization, autoregressive-transformer, evaluation-benchmark, game-rule-understanding, game-generation]
---

## One-line summary

The authors first interview 27 game creatives and conclude that generative AI needs three capabilities to support creative ideation: consistency, diversity, and persistency. They then train WHAM on about 7 years of human gameplay; it is a world model that turns both frames and controller actions into discrete tokens and predicts them one by one with a transformer. The three capabilities are evaluated with FVD, the Wasserstein distance, and human annotation respectively.

## Research question

Creative industries already use generative AI to help with ideation, but studies show that its capabilities often fall short of what creatives expect (p.1). The authors argue that two creative practices are poorly supported by current models: iterative tweaking and divergent thinking (p.1).

Using game development as the example, the paper asks: can we start from user needs to decide which capabilities a generative model should have and how to evaluate them, and then build a model that meets those needs (p.1)?

The authors give three reasons for choosing games (p.1). 3D game development needs many kinds of creative skill. Gameplay data is temporally correlated and multimodal, which suits increasingly complex tasks. Gaming is also the largest entertainment sector worldwide, reaching more than 3 billion people.

The paper also states that it does not aim to provide a tool that can be dropped into a development process. Its goal is to point out gaps in model capabilities and to show how to make progress on them (p.9).

## Method

The paper has three parts: a user study, a model, and an evaluation methodology.

**User study (p.2–3, p.9).** The authors interviewed 27 game creatives from 8 teams: 4 indie studios, 1 AAA studio, and 3 teams of game accessibility developers. Each session lasted 90 minutes with three to four people from the same studio. Participants first walked through a design probe. It was built in Unity and simulated creating a new game level with AI, but its outputs were scripted and did not rely on a real model. The transcripts were analysed with thematic analysis, which gave two themes relevant to model development:

- **Divergent thinking needs consistency.** Creatives want diverse ideas, but a new idea is only meaningful inside a consistent game world, for example one that keeps to its physics, style, and narrative (p.2).
- **Iterative practice needs control.** Creatives usually work by directly manipulating what they see, in many small steps, and they go back and forth between versions and fuse them. A model therefore cannot accept only text prompts; it must also accept direct modifications and keep them (p.2–3).

From these the authors derive three model capabilities needed for [creativity support](../concepts/creativity-support.md) (p.3):

| Capability | Meaning |
|---|---|
| Consistency | Generated frames are coherent over time and follow the game mechanics; for example, solid objects do not pass through walls |
| Diversity | The same starting point can lead to several continuations that differ in meaningful ways |
| Persistency | A user's modifications to frames or actions are kept and built into later generations, and do not disappear after a few frames |

**The WHAM model (p.3–4, p.10).** WHAM stands for World and Human Action Model. It predicts both game frames and the player's controller actions. The authors state that it builds on the line of work on [world models](../concepts/world-model.md) (p.3).

- **Turning data into discrete tokens.** A VQGAN encoder (see [vector quantization](../concepts/vector-quantization.md)) turns each frame into a sequence of tokens. Controller buttons are already discrete; the x and y coordinates of the left and right joysticks are each split into 11 buckets (p.3).
- **Predicting the next token.** Frame tokens and action tokens are interleaved into one sequence, and a decoder-only [transformer](../concepts/autoregressive-transformer.md) is trained to predict the next token. There is no delimiter in the sequence; the model has to infer from position embeddings whether a frame token or an action token comes next (Fig. 2, p.4).
- **Generation and prompting.** At generation time the next token is sampled one at a time. Frame or action tokens can be replaced during this process. This is how generation is controlled through controller actions or by editing frames directly, and it is the prerequisite for persistency (p.3).

The model configurations are as follows (p.3–4, p.10):

| Item | 1.6B WHAM | 15M–894M WHAMs |
|---|---|---|
| Training data | 7 Maps | Skygarden |
| Frame resolution | 300 × 180 | 128 × 128 |
| Tokens per frame | 540 | 256 |
| Image vocabulary size | 16,384 | 4,096 |
| Context length | 5,560 tokens (1 s, 10 frames) | 2,720 tokens for the 894M model (1 s, 10 frames) |
| Image encoder | A ViT-VQGAN of about 300M parameters | A VQGAN of about 60M parameters |

Each action takes 16 tokens (Fig. 2, p.4).

**Training data (p.3, p.9–10).** The data comes from Bleeding Edge, a 3D 4v4 multiplayer combat game by Ninja Theory. It was recorded between September 2020 and October 2022, and personally identifiable information was removed.

- **7 Maps**: 60,986 matches, about 500,000 player trajectories, more than 7 years of gameplay; about 1.4 billion frames after downsampling to 10 Hz.
- **Skygarden**: a single map only, 66,709 trajectories, about 310 million frames, about 1 year of gameplay.

Both are split 80:10:10 into training, validation, and test sets (p.10).

**WHAM Demonstrator (p.6–7).** A concept prototype interface. The user picks some frames as the prompt, and the model generates several branches of possible continuations. The user can continue from any branch or frame, go back and change an earlier choice, or directly edit a frame (for example, add an opponent) and supply controller actions.

## Key results

**Consistency: measured with FVD (p.4, p.10).** The authors use 1,024 human gameplay videos that were not used in training as ground truth, each 10 seconds long. The model receives the first 10 frames and the player's full action sequence and generates the frames. The Fréchet Video Distance (FVD) to the real videos is then computed; lower is better.

- FVD improves with larger models and more compute (Fig. 3a, p.5).
- The 1.6B model has a better FVD. The authors' explanation is that it uses higher-resolution frames, so the ceiling on reconstruction quality is higher (p.4).
- The 1.6B model can generate gameplay that stays consistent for up to 2 minutes (p.4–5).
- The authors also check that FVD is a suitable metric. Two annotators marked each frame generated by the 894M model as consistent or not in structure, response to actions, and interactions; the result matches the trend in FVD (p.10; Extended Data Fig. 3, p.15).
- The correlation between FVD and training loss is r = 0.77 (p.10).

**Diversity: measured with the Wasserstein distance (p.5–6, p.10–11).** The authors compare the marginal distribution of actions generated by the model with that of human actions. A smaller distance means the model is closer to the range of human player behavior. The distance is computed by sub-sampling 10,000 actions from 102,400 (1,024 trajectories of 100 steps each), repeated 10 times (p.6).

- The distance falls during training for all models and approaches the human-to-human baseline; uniform random actions have a distance of 5.3 (Fig. 4a, p.6).
- The 1.6B model is slightly worse than the 894M model. The authors hypothesize that more image tokens and a larger vocabulary put less weight on the action tokens in the loss. Increasing the weight of the action loss tenfold improves the distance (p.5).
- Qualitatively, the same starting point can produce clips that differ in behavior (for example, circling the spawn location or heading straight for a Jumppad) and in appearance (for example, different hoverboard skins) (Fig. 4b, p.6).

**Persistency: human annotation (p.5–7, p.11–12).** The authors manually insert three kinds of element into frames: a Powercell, another character, and a Vertical Jumppad, each at a plausible location where it was not present. The 1.6B model generates a 1-second video, and annotators judge whether the element is kept. The proportion judged as persisting is as follows (Table 1, p.7):

| Edited frames used as conditioning | Powercell | Character | Vertical Jumppad |
|---|---|---|---|
| 1 frame | 58% | 45% | 58% |
| 5 frames | 86% | 85% | 98% |

- Going from 1 to 5 frames gives a significant increase; going from 5 to 10 frames does not (p.11).
- In total 600 videos were generated and annotated by 7 of the authors, who did not know which condition each video came from. 300 of them were annotated by two people independently, with 90% agreement (p.11).
- An inserted opponent does not just stay on screen; it starts attacking the player character and inflicts damage. The Vertical Jumppad was placed in an area where it appears neither in the real game nor in the training data, and it is still kept (Fig. 5, p.7).

**Model scale (p.10).** Training loss improves reliably with model size, data, and compute. By extrapolating from the 15M to 206M models, the authors accurately predicted the final loss of the 894M model, and they used this analysis to configure the 1.6B model for a compute budget of about 1 × 10²² FLOPS.

**What is released (p.12).** Weights for two models (the 1.6B model and a smaller 200M one), the WHAM Demonstrator, and 1,024 Skygarden test trajectories.

## Limitations and open questions

The paper has no separate limitations section. The points below are collected from across the text:

- **Only one game.** The model is trained only on Bleeding Edge. The authors expect the results to be replicable in other games and ultimately to generalize to new games and genres, but this is an expectation; the paper has no experiment on it (p.7).
- **The context is only 1 second.** The model sees only the most recent 10 frames (p.3, p.10).
- **Persistency is weak with a single edited frame.** All three elements persist less than 60% of the time (p.7). Some locations are much lower; the authors suggest the edit was too small, lacked contrast with the background, or was in an unusual place (p.12; Extended Data Fig. 7, p.20).
- **The persistency test is narrow.** It covers only common in-game elements at plausible locations, uses frames deliberately chosen to have little camera and character movement, sets all actions to no-ops, and looks at only 1 second (p.11). The authors leave more imaginative elements to future versions (p.6).
- **Diversity is measured only on the marginal distribution of actions.** The authors explain that the space of possibilities is vast, so they focus on player actions (p.4–5). The largest model, 1.6B, is in fact slightly worse on this metric (p.5).
- **Not a ready-to-use tool.** The WHAM Demonstrator is a concept prototype, and the paper does not evaluate how well creatives actually work with it (p.6, p.9).
- **The training data is not public.** The full dataset is owned by Ninja Theory; only a test sample is released (p.12).
- **The user study sample.** Studios were sampled opportunistically from the Microsoft Founders Hub, most participants came from the USA and the UK, and only 3 of the 27 were women (p.9).

## Related concepts

- [Creativity Support](../concepts/creativity-support.md)
- [World Model](../concepts/world-model.md)
- [Vector Quantization](../concepts/vector-quantization.md)
- [Autoregressive Transformer](../concepts/autoregressive-transformer.md)
- [Evaluation Benchmark](../concepts/evaluation-benchmark.md)
- [Game Rule Understanding](../concepts/game-rule-understanding.md)
- [Game Generation](../concepts/game-generation.md)

## Relation to other sources

- [World Models (2018)](2018-world-models.md): this paper cites it directly and states that it builds on the line of work on world models (p.3, references 39–40). Both papers first compress frames into a compact representation and then use a sequence model to predict what comes next. The differences: that paper uses a variational autoencoder plus an MDN-RNN, is very small, and aims to train an agent. This paper uses a VQGAN plus a 1.6-billion-parameter transformer, trains on human gameplay, predicts both frames and player actions, and aims to give human creatives something to ideate with.

## My notes

