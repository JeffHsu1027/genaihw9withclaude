---
title: Evolution Strategies
aliases: [ES, CMA-ES, evolutionary algorithm, black-box optimization]
updated: 2026-11-17
sources: [2018-world-models]
---

## One-line definition

A family of optimization methods that imitate evolution: repeatedly generate many candidate parameter sets, evaluate each one, and produce the next generation from the candidates that did well.

## Explanation

Most neural networks are trained with backpropagation, which requires a loss function whose gradient can be computed. Evolution strategies need no gradient. They only require a score for each parameter set, which is why they are also called black-box optimization.

The basic loop is: sample a batch of parameters from a probability distribution, score each one, adjust the distribution so that it moves toward the high-scoring ones, and repeat. CMA-ES is one widely used algorithm that also adapts the shape of the sampling distribution.

The advantages are simplicity, easy parallelization, and needing only the score of the final outcome, not the contribution of each step. The drawback is that efficiency drops as the number of parameters grows, so the method suits small models better.

## What each source says

### World Models (2018)

[This paper](../sources/2018-world-models.md) trains the agent's controller C with CMA-ES (p.4).

The architecture and this choice fit together: the complexity goes into the [world model](world-model.md), which is trained with backpropagation, and the controller is deliberately a linear model with only 867 parameters (p.5). The authors note that CMA-ES works well for problems with up to a few thousand parameters (p.4).

The benefits listed by the authors (p.12):

- Only the cumulative reward of a whole rollout is needed, not the entire history. This sidesteps the difficult credit assignment problem in reinforcement learning.
- It is easy to parallelize: many candidate parameter sets can each play a rollout at the same time, and the scores are then gathered.

In the experiments, multiple rollouts run in parallel on the CPU cores of a single machine (p.4). The authors also cite other work showing that evolution strategies are a viable alternative to traditional deep reinforcement learning methods (p.12).

## Related concepts

- [World Model](world-model.md): provides the representation the controller needs, which is what allows the controller to be small.
- [Reinforcement Learning](reinforcement-learning.md): here, evolution strategies take the place of a usual reinforcement learning algorithm for training the controller.
