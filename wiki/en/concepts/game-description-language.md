---
title: Game Description Language
aliases: [GDL, SGDL, Solitaire GDL]
updated: 2026-11-16
sources: [2025-llm-game-rule-understanding]
---

## One-line definition

A formal language for describing game rules precisely, so that a program can read the rules directly and simulate the game.

## Explanation

Rules written in natural language are easily ambiguous, and a program cannot execute them directly. A game description language (GDL) writes rules in a fixed format: which objects exist, the initial state, the available actions, the conditions for each action, and the win condition.

With a GDL, one program can simulate many different games just by swapping the description file. This is useful for research: rule variants can be produced in bulk, the legality of an action can be checked automatically, the correct next state can be computed, and so questions with ground-truth answers can be generated automatically.

The expressive range of a GDL is usually a design trade-off: a simpler language is easier to use but can describe fewer kinds of games.

## What each source says

### LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning (2025)

[This paper](../sources/2025-llm-game-rule-understanding.md) proposes SGDL (Solitaire GDL), a language specific to Solitaire that aims to support a large set of variants while staying fairly simple (p.4). An SGDL file has five sections (p.4–5):

- `$cards`: how many decks are used and which suits and cards are present.
- `$initial`: which piles exist, how cards are dealt at the start, and whether they are face-up or face-down.
- `$actions`: the possible actions (move one card, move a stack, draw) and the conditions that make each one legal.
- `$auto`: actions performed automatically when their conditions are met (optional).
- `$win`: the win condition.

Conditions can be combined into trees with And and Or (p.5).

Notably, SGDL is used only to define games and generate data; it is not shown to the LLM. The framework turns SGDL into a natural-language description of the rules before giving it to the model. The reasons are that natural language generalizes more robustly to other games, and that earlier work shows LLMs understand rules better in natural language (p.4).

## Related concepts

- [Game Rule Understanding](game-rule-understanding.md): a GDL makes it possible to test this ability automatically and at scale.
- [Evaluation Benchmark](evaluation-benchmark.md): a GDL lets questions and ground-truth answers be generated automatically.
