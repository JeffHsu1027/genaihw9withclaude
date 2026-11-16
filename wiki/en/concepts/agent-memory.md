---
title: Agent Memory
aliases: [agent memory, memory system, episode memory, skill memory, world memory]
updated: 2026-11-16
sources: [2026-gui-agents-continual-game-generation]
---

## One-line definition

A mechanism that lets an AI agent keep its past experience and use it in later steps or tasks.

## Explanation

A language model does not remember what happened in its previous task. Without a memory mechanism, an agent starts from zero every time and the same mistakes recur.

Agent memory stores experience as text, such as what was tried, what worked, and what general lessons were drawn, and reads it back into the model's input when needed. This requires no retraining, new experience is usable immediately, and the contents can be inspected and edited directly.

Two common design considerations are how long experience is kept (only within the current task, or across tasks in the long term) and who can see it (private to one agent, or shared among several).

## What each source says

### GUI Agents for Continual Game Generation (2026)

[This paper](../sources/2026-gui-agents-continual-game-generation.md) observes that the same kinds of failures recur, such as incorrect jump arcs in platformers, yet are often rediscovered from scratch each time (p.6). It designs three memory layers along two dimensions, lifecycle and scope (p.6):

| Layer | Lifecycle | Scope | Contents |
|---|---|---|---|
| Episode Memory | In-task | Shared by the two agents | Summaries, fixes, and attempts from each round |
| Skill Memory | Cross-task | Separate for each agent | Recurring code patterns; interaction strategies for playing |
| World Memory | Cross-task | Shared by all agents | General game rules, common archetypes, and design principles |

Ablations with a GPT-5.4 backbone show that every layer contributes (p.18):

| Setting | Rubric pass rate |
|---|---|
| No memory | 64.1 |
| Episode only | 69.3 |
| Episode + Skill | 71.8 |
| All three layers | 72.3 |

Episode Memory contributes the most. The authors explain that without it, the coding agent often re-introduces fixes it had already tried and discarded (p.18).

The authors also list the single memory schema shared by all games as a limitation (p.9).

## Related concepts

- [GUI Agent](gui-agent.md): one of the roles that uses this memory.
- [Fine-Tuning](fine-tuning.md): another way for a system to improve from data. The difference is that fine-tuning changes model parameters and memory does not.
