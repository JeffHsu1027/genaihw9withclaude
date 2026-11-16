---
title: Creativity Support
aliases: [creativity support tool, creative ideation, ideation, divergent thinking, iterative practice, consistency, diversity, persistency]
updated: 2026-11-17
sources: [2025-world-and-human-action-models]
---

## One-line definition

Using tools to help people produce and develop new ideas; the point is to assist the human creative process, not to finish the work in the person's place.

## Explanation

Creative work rarely gets it right in one pass. A common description is that two kinds of thinking alternate: divergent thinking produces as many different possibilities as it can, and convergent thinking then selects among them and refines. Along the way creators revise again and again, and often only know what is wrong once they see a result.

The design of creativity support tools therefore cares about a few things: whether the tool can offer several options quickly, whether a person can adjust things directly, whether the tool keeps those adjustments, and whether the person still feels the work is their own. The last point is often called creative agency.

This differs from the usual way of measuring AI. Automating a task is judged by accuracy and efficiency; creativity support is judged by whether the tool fits the way people work. A model with high-quality output that cannot be steered or modified may be of limited help to a creator.

## What each source says

### World and Human Action Models towards gameplay ideation (2025)

[This paper](../sources/2025-world-and-human-action-models.md) starts from a user study. The authors interviewed 27 game creatives and identified two themes (p.2–3):

- The diverse ideas that come from divergent thinking are only meaningful inside a consistent game world.
- Creatives need to control the iterative process, for example by editing frames directly and having the model adopt those edits.

The authors turn these into three model capabilities that can be evaluated (p.3):

- **Consistency**: generated content is coherent over time and follows the game mechanics.
- **Diversity**: the same starting point can produce several results that differ in meaningful ways.
- **Persistency**: a user's modifications are kept in later generations.

The paper then trains the WHAM model and evaluates each of the three. It also builds a concept prototype, the WHAM Demonstrator, which shows interaction patterns such as generating branches, going back to change a choice, and editing frames directly (p.6–7).

The paper's position is that model evaluation should be informed by the needs of creatives, not only by the effectiveness and efficiency of task completion, and that a model built for creativity will not be an end in itself but one part of a creative workflow (p.7). The authors also note that earlier creativity support tools required structure to be defined by hand for a specific domain, whereas generative models can learn this structure from data (p.1).

## Related concepts

- [World Model](world-model.md): the kind of model this paper uses to support ideation.
- [Game Generation](game-generation.md): what is generated here is gameplay clips for ideation.
- [Evaluation Benchmark](evaluation-benchmark.md): each of the three capabilities has its own way of being measured.
- [Game Rule Understanding](game-rule-understanding.md): consistency requires generated content to follow the game mechanics.
