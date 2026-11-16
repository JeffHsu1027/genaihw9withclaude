---
title: Out-of-Distribution Generalization
aliases: [OOD, OOD generalization, out-of-distribution, in-distribution]
updated: 2026-11-16
sources: [2025-llm-game-rule-understanding, 2018-world-models]
---

## One-line definition

A model's ability to keep performing well on data that differs in nature from its training data.

## Explanation

Data of a kind seen during training is called in-distribution (ID); data of an unseen kind is called out-of-distribution (OOD). Good in-distribution performance may only mean the model memorized features of its training data. Good out-of-distribution performance is stronger evidence that it learned something transferable.

"Out-of-distribution" is a matter of degree, not a yes-or-no label. New data can differ from the training data a little or a lot, and larger differences are usually harder to generalize to. So besides splitting data into inside and outside, it is useful to measure how far outside it is.

There is often a trade-off between the two. Making the training data more diverse usually helps out-of-distribution performance, but each kind of data then gets fewer samples, so in-distribution performance may drop.

## What each source says

### LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning (2025)

[This paper](../sources/2025-llm-game-rule-understanding.md) defines the "distribution" at the level of rulesets: a Solitaire variant not seen in training is out-of-distribution. It also adds a category in between (p.7):

- **In-distribution (ID)**: a separate set of samples from the tested variant is in the training set.
- **In-family (IF)**: the variant is not in the training set, but other variants from its family are.
- **Out-of-distribution (OOD)**: the variant is not in the training set.

The authors also define a distance between variants, based on the number of rule differences, to measure how far outside a variant is (p.6).

Main findings (p.8–9):

- Fine-tuning helps on both in-distribution and out-of-distribution rulesets in almost all cases.
- The more diverse the training variants, the better the out-of-distribution performance, while in-distribution performance may drop.
- Generalization has a limit: on Thirteen, whose rules differ too much, fine-tuning brings no meaningful improvement.
- Recombining similar rules generalizes more easily than handling entirely new rules.

The paper also mentions a different result: Chu et al. found that, on the game General Points, fine-tuning lowers out-of-distribution performance while reinforcement learning raises it (p.3). The authors point out that the settings differ. Rule variations in General Points only change how cards are interpreted, whereas rules in Solitaire variants directly change how the game is played.

### World Models (2018)

[This paper](../sources/2018-world-models.md) does not study out-of-distribution generalization as a topic, but it meets the problem at a key point: a learned [world model](world-model.md) is accurate only near the states covered by its training data.

The world model's training data comes from rollouts of a random policy (p.5). When a controller is trained inside the world model, it finds policies that look good under the model but fail in the actual environment. The authors explain that this is usually because the policy visits states away from the training distribution, where the model is wrong (p.9).

The paper's remedy is not to enlarge the training data but to raise the sampling temperature, which makes the virtual environment more random and makes it harder for the controller to exploit the model's errors (p.9–10). For more complex environments, the authors propose iterative training: let the agent keep exploring and collecting new observations to improve the world model (p.10).

### Comparing the sources

| | 2025 rule understanding | 2018 World Models |
|---|---|---|
| What the "distribution" is | Rulesets seen in training | States reached during random play |
| What faces out-of-distribution data | A fine-tuned LLM | A learned world model |
| Consequence out of distribution | Accuracy drops; fine-tuning stops helping when the difference is too large | The model predicts wrongly and the controller exploits it |
| Remedy | More diverse training data | More randomness; iteratively collecting new data |

The shared conclusion is that a learned model is reliable only within a range close to its training data. The difference is that the 2025 paper measures how large this range is, while the 2018 paper is affected by the problem when using its model. There is no citation between the two papers; this comparison is this wiki's own synthesis.

## Related concepts

- [Fine-Tuning](fine-tuning.md): here, out-of-distribution generalization is used to examine the effect of fine-tuning.
- [Game Rule Understanding](game-rule-understanding.md): the ability being tested for generalization.
- [World Model](world-model.md): predicts wrongly in states its training data did not cover.
- [Evaluation Benchmark](evaluation-benchmark.md): in-distribution versus out-of-distribution is one way of splitting test sets.
