---
title: LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning
authors: Bahar Bateni, Benjamin Pratt, Jim Whitehead
year: 2025
venue: AIIDE 2025 (21st AAAI Conference on Artificial Intelligence and Interactive Digital Entertainment)
url: https://doi.org/10.1609/aiide.v21i1.36804
raw: raw/LLM game rule understanding through out-of-distribution fine-tuning.pdf
added: 2026-11-16
concepts: [game-rule-understanding, out-of-distribution-generalization, fine-tuning, game-description-language, evaluation-benchmark, game-generation, reinforcement-learning]
---

## One-line summary

The authors use a custom Solitaire game description language to generate questions of the form "is this move legal, and what is the next state?", and find that a small amount of fine-tuning improves GPT-4o-mini's rule understanding, even on Solitaire variants it was not trained on. When the rules differ too much, however, fine-tuning barely helps.

## Research question

Large language models (LLMs) are natively poor at understanding rules. Fine-tuning on a specific ruleset helps, but it gives up the main advantage of a pre-trained model: generalizing to rulesets it has never seen. That ability is necessary if LLMs are to assist game design by evaluating, modifying, or generating rules (p.1).

The paper asks three questions (p.1–2):

1. How well does an out-of-the-box LLM understand game rules?
2. How much does fine-tuning on specific rules help? (in-distribution)
3. Does this fine-tuning generalize to other games with unseen rules? (out-of-distribution)

## Method

**Testbed: Solitaire.** Solitaire rules are simple, since every game moves cards between piles by rank and suit. Yet there are many variants that play very differently, which makes Solitaire suitable for testing generalization across rulesets (p.2).

**SGDL and the simulation framework.** The authors design SGDL, a [game description language](../concepts/game-description-language.md) for Solitaire. One file defines a variant in five sections: `$cards`, `$initial`, `$actions`, `$auto`, and `$win` (p.4–5). From such a file the framework provides a playable game, general game-playing agents, and a template-based text generator (p.4). The repository contains more than 30 variants from four families: Spider, Klondike, Pairing, and Free Cell (p.5). Code and datasets are public at <https://github.com/iambb5445/SolitaireGDL> (p.4).

**Task: game progression.** Each question gives a game state and an action. The model must decide whether the action is legal and, if it is, output the next state (p.2). The authors choose this task on purpose: it needs no search, so it measures only whether the rules are understood and applied correctly (p.3). Rules are given to the model in natural language, not in SGDL (p.4).

**Dataset generation.** For each variant, 100 random initial states are generated and played by a depth-first search agent until a win or 2000 moves. States are sampled from those visited, and each is paired with one random action that is legal or illegal with equal probability (p.6–7). The framework also generates a step-by-step explanation of legality, but not of how the next state is produced (p.5).

**Distance metric.** The distance between two variants is based on the number of rule differences, normalized between 0 and 1. Variants in the same family are closer. Thirteen, whose rule is that ranks must add up to 13, is especially far from the others (p.6).

**Fine-tuning setup.** All fine-tuned models are based on GPT-4o-mini, with training sets of increasing diversity (p.7):

| Model | Variants in training | Total samples |
|---|---|---|
| ft0 | 1 (Blind Alley's) | 100, 200, or 300 |
| ft1 | 5 from one family | 200 |
| ft2 | 5 from 2 families | 200 |
| ft3 | 10 from 3 families | 200 |

**Test sets.** Six variants with 500 questions each, chosen by their relation to the training sets: in-distribution (ID), in-family but out-of-distribution (IF), and out-of-distribution (OOD). For example, Blind Alley's is in-distribution for every model, and Baker's Game is out-of-distribution for every model (p.7).

**Prompt and metrics.** The model outputs JSON containing the state, the action, step-by-step thinking, legality, and the next state, with 4 few-shot examples. The metrics are legal accuracy and next-state accuracy; the latter is counted only for legal actions (p.8).

## Key results

**Without fine-tuning, performance is poor.** GPT-4o-mini averages 0.67 legal accuracy, while random guessing would give about 0.5 (p.8). Errors fall into two groups: citing the wrong rules or board information, and executing rules incorrectly. These match the triggering and execution errors described in RuleBench (p.8).

**Fine-tuning almost always helps.** The table shows GPT-4o-mini results as "legal accuracy / next-state accuracy" (Table 5, p.9):

| Test variant | No fine-tuning | ft0 (200) | ft3 |
|---|---|---|---|
| Blind Alley's (ID for all) | 0.63 / 0.14 | 0.86 / 0.82 | 0.78 / 0.59 |
| Legion (in-family, OOD) | 0.62 / 0.14 | 0.69 / 0.67 | 0.79 / 0.67 |
| Baker's Game (OOD for all) | 0.75 / 0.29 | 0.73 / 0.79 | 0.88 / 0.86 |
| Thirteen (ID for ft3 only) | 0.81 / 0.00 | 0.83 / 0.00 | 0.80 / 0.02 |

- Fine-tuning on a single game with 200 samples raises next-state accuracy from 0.14 to 0.82 (Blind Alley's). Among 100, 200, and 300 samples, 200 works best (p.8).
- More diverse training data can lower in-distribution performance but raises out-of-distribution performance: Baker's Game reaches 0.88 / 0.86 with ft3 (p.9).
- Generalization has a limit: Thirteen's rules differ too much from the other variants, and no fine-tuned model improves on it meaningfully (p.8–9).
- Recombining similar rules generalizes more easily than handling entirely new rules. The Spider and Klondike families are different, but they are different combinations of similar rules (p.8).
- Step-by-step legality explanations help generalization: on fully out-of-distribution datasets, legal accuracy generally rises with training diversity. Next-state accuracy depends more on whether the game is in-distribution; the authors suggest this is because no step-by-step explanation is provided for it (p.8).

**Comparison with larger models.** Without fine-tuning, DeepSeek-v3 and Llama 3.3 70B perform clearly better (for example, 0.93 / 0.87 and 0.87 / 0.67 on Baker's Game), but fine-tuned GPT-4o-mini matches or surpasses them in most cases (p.8–9).

## Limitations and open questions

- Only one model, GPT-4o-mini, is fine-tuned. It was chosen for speed and relaxed rate limits; the goal is to analyze the effect of fine-tuning, not to reach the best performance (p.7, p.9).
- Performance after fine-tuning is still not satisfactory (p.9).
- Step-by-step explanations are generated only for whether an action is legal, not for how the next state is produced (p.9).
- Only the game progression task is tested; game playing and rule space are not covered (p.3).
- ft3's training set includes Thirteen, which is very different from the rest and may have hurt its performance on other variants (p.8).
- Future directions proposed by the authors: tool use for retrieving board information, an intermediate logical formalization of rules, and reinforcement learning to improve reasoning steps (p.9).

## Related concepts

- [Game Rule Understanding](../concepts/game-rule-understanding.md)
- [Out-of-Distribution Generalization](../concepts/out-of-distribution-generalization.md)
- [Fine-Tuning](../concepts/fine-tuning.md)
- [Game Description Language](../concepts/game-description-language.md)
- [Evaluation Benchmark](../concepts/evaluation-benchmark.md)
- [Game Generation](../concepts/game-generation.md)
- [Reinforcement Learning](../concepts/reinforcement-learning.md)

## Relation to other sources

- [GUI Agents for Continual Game Generation (2026)](2026-gui-agents-continual-game-generation.md): both papers build evaluation methods related to games, but they evaluate different things. This paper evaluates a model, asking whether an LLM can work out game states from text rules, with ground truth computed by a simulator. That paper evaluates generated games, judged against human-written criteria by an agent that actually plays them. Their motivations are also close: both want AI to give feedback during game development. That paper does not cite this one; the comparison is this wiki's own synthesis.
- [World Models (2018)](2018-world-models.md): both papers ask a model to predict the next state given the current state and an action. This paper hands the rules to an LLM explicitly as text and checks each answer. That paper provides no rules at all and lets a neural network learn how the game works from frames alone. Both also run into an out-of-distribution problem: here, fine-tuning stops helping when the rules differ too much; there, the world model is wrong when the controller reaches states the training data did not cover. There is no citation between the two papers; the comparison is this wiki's own synthesis.

## My notes

