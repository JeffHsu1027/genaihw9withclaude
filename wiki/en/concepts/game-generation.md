---
title: Game Generation
aliases: [AI game generation, game code generation, rule generation]
updated: 2026-11-16
sources: [2026-gui-agents-continual-game-generation, 2025-llm-game-rule-understanding]
---

## One-line definition

Having AI automatically produce a game, or part of one such as code, rules, or levels, from a natural-language description.

## Explanation

Game generation can happen at different levels. The most complete form produces a runnable game program directly. More partial forms produce rules, levels, or assets that an existing engine then runs.

It is harder than ordinary code generation because "correct" is hard to define. A program that runs is not necessarily a game that can be played, and a game that can be played does not necessarily match the intended design. Research on game generation therefore often has to deal with two problems at once: how to generate, and how to tell whether the result is good.

## What each source says

### GUI Agents for Continual Game Generation (2026)

[This paper](../sources/2026-gui-agents-continual-game-generation.md) generates complete web games (HTML/CSS/JS) and sorts existing approaches into three paradigms (p.2, p.6):

- **Static generation**: the whole game is produced from the prompt in one shot and never revised.
- **End-to-end agentic system**: code is produced in multiple steps and probed at runtime, but only at the syntactic and compiling level.
- **Continual evolution**: the paper's own approach, a sustained dialogue between coding and [playing](playtesting.md).

The average rubric pass rates of the three are 29.7%, 52.2%, and 66.8% respectively (p.6). The paper concludes that both generation and evaluation need a role that actually plays the game (p.8).

### LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning (2025)

[This paper](../sources/2025-llm-game-rule-understanding.md) does not generate games itself, but treats generation as a long-term goal. Its concern is the level of rules: it hopes LLMs can evaluate, modify, or generate rules during game development (p.1).

The paper calls such tasks "rule space" and notes that they require the model to consider all possible states and actions, which is harder than working out a single step (p.3). The authors hold that to get there, a model must first understand rules and generalize to unseen ones, so the paper starts with the more basic problem of [game rule understanding](game-rule-understanding.md) (p.1, p.3).

### Comparing the sources

The two papers work at different levels. The first generates an entire runnable game and judges quality from the behavior of the finished product. The second is concerned with the rules themselves and regards understanding rules as a prerequisite for generating them. Neither paper cites the other; this comparison is this wiki's own synthesis.

## Related concepts

- [Playtesting](playtesting.md): a way to tell whether a generated result can be played.
- [Game Rule Understanding](game-rule-understanding.md): the ability that generating and modifying rules depends on.
- [Evaluation Benchmark](evaluation-benchmark.md): how generation quality is measured.
