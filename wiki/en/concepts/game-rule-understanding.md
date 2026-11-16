---
title: Game Rule Understanding
aliases: [rule understanding, game progression, next-state prediction]
updated: 2026-11-16
sources: [2025-llm-game-rule-understanding, 2018-world-models]
---

## One-line definition

Whether a model correctly grasps a set of game rules and can use them to judge, predict, play, or even evaluate and modify the rules themselves.

## Explanation

Being able to play a game is not the same as understanding its rules. A model may have memorized good moves under one ruleset and fail as soon as the rules change. Rule understanding asks a different question: given a text description of the rules, can the model reason by following them?

The most basic test is game progression: given a state and an action, is the action legal, and what does the state become afterwards? Such questions need no search or strategy, so a wrong answer means the rules were not applied correctly. This separates "understanding the rules" from "being good at planning".

Two kinds of errors are common: picking the wrong rule or misreading the board, and picking the right rule but executing it wrongly, for example resolving a condition to the opposite truth value.

## What each source says

### LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning (2025)

[This paper](../sources/2025-llm-game-rule-understanding.md) divides rule understanding into three types (p.3):

- **Game Progression**: given a state and an action, predict how the action changes the state. The rules are applied to one specific scenario.
- **Game Playing**: given a state, suggest an action. All possible actions must be considered.
- **Rule Space**: generate, modify, or evaluate rules. All possible states and actions must be considered.

The last two both require some form of search, so the authors study only game progression in order to measure rule understanding by itself (p.3). In their experiments, GPT-4o-mini without fine-tuning averages only 0.67 legal accuracy on these questions, slightly above the 0.5 of random guessing (p.8).

### World Models (2018)

[This paper](../sources/2018-world-models.md) does not use the term "rule understanding"; including it on this page is this wiki's own judgment. The reason is that its [world model](world-model.md) does the same thing as game progression: given the current state and an action, predict the next state.

The difference is that nobody tells the model the rules. Seeing only frames from random play, the world model learns to simulate the game logic, enemy behaviour, and physics. The paper's examples include: the view shifts accordingly when the agent chooses to move left, the agent is blocked from moving beyond the walls on both sides, several fireballs are tracked along their directions at once, and the model detects whether the agent has been hit (p.8).

This understanding is incomplete. The world model does not even reproduce the number of monsters exactly, and it occasionally generates trajectories that do not follow the laws of the actual environment (p.8). The controller also found a way to make monsters never shoot fireballs, which cannot happen in the real game (p.8).

### Comparing the sources

| | 2025 rule understanding | 2018 World Models |
|---|---|---|
| Where the rules come from | Given explicitly as text | Not given; learned from frame data |
| Model | LLM | VAE plus MDN-RNN |
| Form of the state | A game state written as text | A latent vector compressed from a frame |
| How it is checked | Each answer is compared for legality and next state | Not checked directly; judged by whether an agent trained inside it can solve the real environment |
| Do the rules change | Yes; unseen rulesets are tested | No; each model corresponds to one game |

"Understanding the rules" means different things in the two papers. In the 2025 paper it means reasoning correctly from explicit rules, verifiable question by question. In the 2018 paper the model can produce subsequent frames that roughly follow the rules, but it cannot state what the rules are and is not checked step by step. Calling both rule understanding is a broad usage. The definition at the top of this page is this wiki's working definition, not a standard definition in the field. There is no citation between the two papers; this comparison is this wiki's own synthesis.

## Related concepts

- [Out-of-Distribution Generalization](out-of-distribution-generalization.md): whether rule understanding extends to unseen rulesets.
- [Fine-Tuning](fine-tuning.md): one way to improve rule understanding.
- [Game Description Language](game-description-language.md): used to define rules precisely and generate questions.
- [World Model](world-model.md): learns to predict the next state from data, without explicit rules.
- [Evaluation Benchmark](evaluation-benchmark.md): how rule understanding is measured.
- [Game Generation](game-generation.md): a further goal that depends on rule understanding.
