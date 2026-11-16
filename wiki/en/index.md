# Index

A catalog of every page in the wiki. Updated after each ingest.

## Sources

- [LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning](sources/2025-llm-game-rule-understanding.md) (2025) — Tests LLM rule understanding on Solitaire variants; a little fine-tuning generalizes to unseen variants, but not when the rules differ too much.
- [GUI Agents for Continual Game Generation](sources/2026-gui-agents-continual-game-generation.md) (2026) — Lets GUI agents actually play generated web games, for evaluation (PlaytestArena) and for improving generation (Play2Code).
- [World Models](sources/2018-world-models.md) (2018) — Splits an agent into vision, memory, and controller; learns a world model and uses it to train a tiny controller, even entirely inside the model's "dream".

## Concepts

- [Game Rule Understanding](concepts/game-rule-understanding.md) (2 sources) — Whether a model correctly grasps a set of game rules and can use them to judge, predict, play, or even evaluate and modify the rules themselves.
- [Out-of-Distribution Generalization](concepts/out-of-distribution-generalization.md) (2 sources) — A model's ability to keep performing well on data that differs in nature from its training data.
- [Fine-Tuning](concepts/fine-tuning.md) (1 source) — Taking an already pre-trained model and continuing to train it on task-specific data so that it performs better on that task.
- [Game Description Language](concepts/game-description-language.md) (1 source) — A formal language for describing game rules precisely, so that a program can read the rules directly and simulate the game.
- [GUI Agent](concepts/gui-agent.md) (1 source) — An AI agent that operates a graphical interface by looking at the screen and using the mouse and keyboard, the same way a person uses software.
- [Playtesting](concepts/playtesting.md) (1 source) — Finding a game's problems by actually playing it; what is checked is the experience of play, not the code.
- [Game Generation](concepts/game-generation.md) (2 sources) — Having AI automatically produce a game, or part of one such as code, rules, or levels, from a natural-language description.
- [Agent Memory](concepts/agent-memory.md) (1 source) — A mechanism that lets an AI agent keep its past experience and use it in later steps or tasks.
- [Evaluation Benchmark](concepts/evaluation-benchmark.md) (2 sources) — A fixed set of tasks plus a scoring method, used to measure an ability or the quality of an output in a repeatable way.
- [World Model](concepts/world-model.md) (1 source) — An agent's internal model of its environment, which predicts what will happen next given the current state and an action.
- [Variational Autoencoder](concepts/variational-autoencoder.md) (1 source) — A neural network that learns to compress an input into a small latent vector and to reconstruct an approximation of the original input from that vector.
- [Evolution Strategies](concepts/evolution-strategies.md) (1 source) — A family of optimization methods that imitate evolution: repeatedly generate many candidate parameter sets, evaluate each one, and produce the next generation from the candidates that did well.
