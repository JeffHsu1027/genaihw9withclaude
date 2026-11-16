---
title: Game Generation
aliases: [AI game generation, game code generation, rule generation, gameplay generation, procedural content generation, PCG]
updated: 2026-11-17
sources: [2026-gui-agents-continual-game-generation, 2025-llm-game-rule-understanding, 2025-world-and-human-action-models]
---

## One-line definition

Having AI automatically produce a game, or part of one such as code, rules, levels, or gameplay clips, from a prompt given by a person, such as a natural-language description or reference frames.

## Explanation

Game generation can happen at different levels. The most complete form produces a runnable game program directly. More partial forms produce rules, levels, or assets that an existing engine then runs. Another approach produces no program at all and has a model generate the frames of gameplay directly.

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

### World and Human Action Models towards gameplay ideation (2025)

What [this paper](../sources/2025-world-and-human-action-models.md) generates is not a runnable game but gameplay clips: sequences of game frames and the player's controller actions (p.3). Including it on this page is a broad usage. The paper positions itself as building on the research tradition of computational creativity and procedural content generation (p.1).

It differs from the two papers above in several ways:

- **Output**: the frames are generated directly by WHAM, a [world model](world-model.md), with no game program or explicit rules behind them (p.3).
- **Prompting**: there is no text description. The user picks some frames as the starting point and can also edit frames directly or supply controller actions to steer the generation (p.7).
- **Purpose**: not to produce a finished product, but to let creatives quickly see several possible developments during ideation (p.6–7). The authors state that this is not a tool that can be readily integrated into a development process (p.9).
- **Scope**: the model is trained on a single existing game, and everything it generates stays within that game (p.3). A user can place an element where it did not exist before, but the paper does not demonstrate generating a new game.

The authors see the value of such models in their ability to learn the structure of a complex domain from data, removing the need to handcraft it for each domain (p.1, p.7).

### Comparing the sources

| | 2026 GUI Agents | 2025 rule understanding | 2025 WHAM |
|---|---|---|---|
| What is generated | Runnable web games | Nothing; rule generation is a long-term goal | Gameplay clips (frames and actions) |
| Prompt | A text description | (Not applicable) | Starting frames, edited frames, controller actions |
| How quality is judged | An agent plays and judges against a rubric | Each answer is checked for correct application of the rules | Consistency, diversity, and persistency |
| The human's role | Provides prompts and rubrics | Designs rules and questions | Repeatedly selects and edits during generation |

The three papers work at different levels. GUI Agents generates an entire runnable game and judges quality from the behavior of the finished product. The rule-understanding paper is concerned with the rules themselves and regards understanding rules as a prerequisite for generating them. WHAM does not produce the game itself; it generates clips of "what playing it might look like" for people to ideate with before they start building. None of the three papers cites another; this comparison is this wiki's own synthesis.

## Related concepts

- [Playtesting](playtesting.md): a way to tell whether a generated result can be played.
- [Game Rule Understanding](game-rule-understanding.md): the ability that generating and modifying rules depends on.
- [Evaluation Benchmark](evaluation-benchmark.md): how generation quality is measured.
- [World Model](world-model.md): a model that generates gameplay frames directly, without writing a program.
- [Creativity Support](creativity-support.md): generation used to help people ideate, not to produce a finished product.
