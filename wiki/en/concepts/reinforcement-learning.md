---
title: Reinforcement Learning
aliases: [RL, deep reinforcement learning, deep RL, model-free RL, credit assignment, policy, reward]
updated: 2026-11-17
sources: [2018-world-models, 2025-llm-game-rule-understanding]
---

## One-line definition

Having an agent interact with an environment and adjust its behavior according to the rewards it receives, so that it gradually learns how to collect the most cumulative reward.

## Explanation

Reinforcement learning is set up as a loop: the agent observes the state of the environment and picks an action, and the environment returns a new state and a reward. What the agent has to learn is a policy, that is, what to do in each state.

The difference from supervised learning is that nobody provides the right answer. The agent only finds out whether the outcome was good and has to try things for itself. Rewards also often arrive late; for example, a win or loss is only known when the game ends, and it is hard to tell which earlier step caused it. This is called the credit assignment problem.

There are two families of methods. Model-free methods learn a policy directly from trial and error. Model-based methods first learn a model of the environment and then use it for planning or training.

Reinforcement learning is good at problems that require searching among many possible actions; the price is that it usually needs a great deal of interaction.

## What each source says

### World Models (2018)

[This paper](../sources/2018-world-models.md) takes reinforcement learning as its problem setting but has reservations about using it directly.

- **Starting point**: a reinforcement learning agent benefits from a good representation of states and a predictive model of the future (p.2).
- **Criticism of existing practice**: model-free reinforcement learning methods mostly use small networks with few parameters. The authors attribute this to the fact that the reinforcement learning algorithm is often bottlenecked by the credit assignment problem, which makes it hard to learn the millions of weights of a large model (p.2).
- **The paper's response**: split the agent. The large [world model](world-model.md) is trained without supervision by backpropagation and never touches the reward. Only a very small controller sees the reward, and it is trained with [evolution strategies](evolution-strategies.md) instead of a usual reinforcement learning algorithm, needing only the cumulative reward of a whole rollout (p.4, p.12).
- **Baselines**: in car racing, DQN scores 343 ± 18 and A3C scores 591 ± 45 (continuous actions) and 652 ± 10 (discrete actions), while the full world model agent scores 906 ± 21 (Table 1, p.6).
- **The model-based route**: the authors note that many earlier works learned a dynamics model of the environment but did not actually use it to fully replace the actual environment, possibly because an imperfect model is easily exploited by the agent (p.9).

The authors also cite other work showing that evolution strategies are a viable alternative to deep reinforcement learning methods (p.12).

### LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning (2025)

[This paper](../sources/2025-llm-game-rule-understanding.md) does not use reinforcement learning. In the paper it is the other route that the authors compare themselves with.

- The authors mention that an LLM's performance at playing games or predicting the next state can be improved when it is paired with search algorithms such as Monte Carlo Tree Search (MCTS) or other reinforcement learning methods (p.1).
- A related study, Chu et al., compares two kinds of post-training on the game General Points. Its results suggest that [fine-tuning](fine-tuning.md) degrades performance on [out-of-distribution](out-of-distribution-generalization.md) rulesets, while reinforcement learning improves it (p.3).
- The authors explain why they do not adopt it: this paper uses only game progression questions, which need no search over the action space, and search is exactly what reinforcement learning algorithms excel at. Their goal is purely to measure whether rules are understood and applied correctly (p.3).
- Among future directions, the authors list using reinforcement learning strategies to improve reasoning steps as one possible improvement (p.9).

### Comparing the sources

| | World Models (2018) | 2025 rule understanding |
|---|---|---|
| Role of reinforcement learning | The problem setting and the baseline | Not used; another training method the paper compares itself with |
| Property pointed out | Limited by the credit assignment problem, so hard to train a large model directly | Good at searching the action space |
| What the paper uses instead | A world model trained without supervision, plus a small controller trained with evolution strategies | Supervised fine-tuning |
| Reason | Moves most parameters into a part that needs no reward signal | The questions need no search; the paper only wants to measure rule understanding |

Both papers treat reinforcement learning as background and choose a different training method, but for different reasons. World Models goes around it because it is hard to train a large model with it. The rule-understanding paper does so because its research question has no use for what reinforcement learning is good at. There is no citation between the two papers; this comparison is this wiki's own synthesis.

## Related concepts

- [World Model](world-model.md): the model of the environment that model-based reinforcement learning learns.
- [Evolution Strategies](evolution-strategies.md): the method World Models (2018) uses to train the controller in place of a usual reinforcement learning algorithm.
- [Fine-Tuning](fine-tuning.md): another training method, which the 2025 rule-understanding paper contrasts with reinforcement learning.
- [Out-of-Distribution Generalization](out-of-distribution-generalization.md): one study reports that reinforcement learning and fine-tuning affect out-of-distribution performance in opposite directions.
