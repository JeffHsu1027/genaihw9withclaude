---
title: GUI Agent
aliases: [GUI agent, graphical user interface agent, computer-use agent]
updated: 2026-11-16
sources: [2026-gui-agents-continual-game-generation]
---

## One-line definition

An AI agent that operates a graphical interface by looking at the screen and using the mouse and keyboard, the same way a person uses software.

## Explanation

Most automation between programs relies on APIs or code. A GUI agent takes another route: what it sees is the screen, and what it does is click, press keys, and scroll. Such agents are usually built on models that handle both images and text.

Its operation can be split into three repeated steps: observe the current screen, reason about what to do next, perform one input action, and then observe how the screen changed.

The advantage is generality: as long as software has an interface humans can use, the agent can operate it, with no special hook required. The cost is that it is slower than calling a program directly, and it can misread the screen or click the wrong place.

## What each source says

### GUI Agents for Continual Game Generation (2026)

[This paper](../sources/2026-gui-agents-continual-game-generation.md) argues that, among the possible substitutes for human playtesters, only GUI agents meet the game on the same visual and input surface as humans, which makes them the natural surrogate (p.3).

The paper first verifies that they can actually play. On about 120 levels, the three models pass 0.72 to 0.82 of levels within 20 attempts, against 0.92 for humans (p.3). The authors note that the agents show the three capabilities a playtester needs: observing the screen, reasoning about the next step, and acting (p.3).

The paper gives GUI agents two separate roles:

- **Judge**: plays the game with the rubric in hand and decides whether each criterion passes (p.4).
- **Playtester**: sees neither the rubric nor the code, and reports only what happened during play together with suggested fixes (p.5).

Different models acting as playtester emphasize very different things; for example, nearly half of Kimi K2.5's feedback concerns visual and aesthetic aspects (p.7–8).

## Related concepts

- [Playtesting](playtesting.md): what GUI agents are used for here.
- [Agent Memory](agent-memory.md): the mechanism that lets agents accumulate testing experience.
- [Evaluation Benchmark](evaluation-benchmark.md): the setting in which a GUI agent serves as judge.
