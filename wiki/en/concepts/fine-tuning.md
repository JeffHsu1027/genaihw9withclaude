---
title: Fine-Tuning
aliases: [fine-tune, finetuning, supervised fine-tuning]
updated: 2026-11-17
sources: [2025-llm-game-rule-understanding]
---

## One-line definition

Taking an already pre-trained model and continuing to train it on task-specific data so that it performs better on that task.

## Explanation

A pre-trained model already has broad general knowledge. Fine-tuning adjusts its parameters with a small amount of task data on top of that foundation, at far lower cost than training from scratch.

The main risk is over-specialization: the model improves on the kinds of data it was trained on but may lose some of its generality. So when evaluating fine-tuning, looking only at the trained task is not enough. One should also check whether nearby, untrained tasks improve or get worse.

The design of the training data affects the outcome, for example the number of samples, how broad the covered types are, and whether answers come with a reasoning process.

## What each source says

### LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning (2025)

[This paper](../sources/2025-llm-game-rule-understanding.md) fine-tunes GPT-4o-mini on Solitaire game progression questions and compares four training sets of different diversity, ft0 to ft3 (p.7). Its findings:

- **A few samples are enough.** 200 samples from a single game raise next-state accuracy on Blind Alley's from 0.14 to 0.82 (p.9).
- **More samples are not always better.** Among 100, 200, and 300 samples, 200 works best (p.8).
- **The diversity trade-off.** With more variants in training, each variant gets fewer samples, so in-distribution performance may drop, but out-of-distribution performance usually rises (p.9).
- **Including reasoning helps.** The training data contains step-by-step legality explanations, which the authors believe help legality judgments generalize. The next state has no such explanation, and its generalization is less consistent (p.8).
- **A small model can catch up with large ones.** Fine-tuned GPT-4o-mini matches or surpasses DeepSeek-v3 and Llama 3.3 70B without fine-tuning in most cases (p.8).

The authors also note that performance after fine-tuning is still not satisfactory (p.9).

## Related concepts

- [Out-of-Distribution Generalization](out-of-distribution-generalization.md): a way to check whether fine-tuning has over-specialized the model.
- [Game Rule Understanding](game-rule-understanding.md): the ability that fine-tuning aims to improve here.
- [Agent Memory](agent-memory.md): another way for a system to improve from experience. The difference is that it does not change model parameters.
- [Reinforcement Learning](reinforcement-learning.md): another training method, which this paper contrasts with fine-tuning.
