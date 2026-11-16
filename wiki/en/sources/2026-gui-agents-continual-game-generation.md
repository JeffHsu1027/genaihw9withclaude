---
title: GUI Agents for Continual Game Generation
authors: Yixu Huang et al.
year: 2026
venue: arXiv 2605.28258v2
url: https://arxiv.org/abs/2605.28258
raw: raw/GUI Agents for Continual Game Generationv2.pdf
added: 2026-11-16
concepts: [gui-agent, playtesting, game-generation, agent-memory, evaluation-benchmark]
---

## One-line summary

The authors argue that game generation needs a "player", and let GUI agents actually play generated web games: they use one to build the PlaytestArena benchmark, and put another inside the generation loop as Play2Code, which reaches an average rubric pass rate of 66.8%, above 29.7% for single-pass generation and 52.2% for an agentic pipeline that does not play.

## Research question

Generating a game is not the same as making a playable one. A game can compile, run, and pass every test while still being broken in ways static analysis cannot reveal, such as a key press with no response, a sprite that does not render, or a win condition that never triggers. The game industry has therefore long relied on human playtesters, but LLMs now produce games faster than humans can play them (p.1).

The paper asks three questions (p.2):

1. How does the nature of games change what counts as "better"?
2. What should a "player" look like, and where should it sit in the process?
3. How does such a "player" drive game generation, and does it drive it where a human would?

## Method

**First, check whether GUI agents can playtest.** The authors test three models on 20 web games (split into about 120 levels) and recruit three human players under matched conditions as a reference (p.3).

**PlaytestArena: the benchmark.** It contains 200 web game generation tasks across 8 genres, with games delivered as self-contained HTML/CSS/JS bundles (p.4). Each task has two parts:

- A generation prompt written by human experts, 131 tokens on average, specifying gameplay, win and loss conditions, player input, and feedback behavior.
- A rubric written by human experts, where each criterion states one observable expected behavior, for example "the spawned enemies move toward the player's position". There are 1,548 criteria in total, 5 to 17 per game with a mean of 7.7, covering five dimensions: mechanics, controls, progression, interface, and visual feedback (p.4, p.14).

Scoring is done by an independent GPT-5.5 GUI judge. It loads the game in a browser, plays it through clicks and keys, and gives each criterion a pass or fail based on what it observes. The score is the fraction of criteria passed (p.4).

**Play2Code: putting playtesting inside generation.** Two agents work around a shared runtime (p.5):

- The **Game Agent** writes, debugs, and patches the code.
- The **GUI playtester** plays the current build in a browser, with no access to the code or the rubric. Afterwards it produces two artifacts: a play summary, and a fix list that maps observed failures to code-level changes (p.5–6).

The Game Agent treats this report as advice, not instruction, and decides for itself which fixes to act on. The loop ends when the playtester judges that the build meets the intent of the prompt, or when the maximum number of rounds is reached (p.6). The maximum is 5 rounds; the average is 3.24, and 93.5% of tasks end early (p.18).

**Three-layer memory.** Memory is organized by lifecycle (in-task or cross-task) and scope into episode, skill, and world memory, so that experience accumulates across rounds and tasks (p.6). See [Agent Memory](../concepts/agent-memory.md).

**Baselines.** Direct LLM emits the whole game in a single pass. OpenGame is an agentic pipeline that iteratively builds, runs, and inspects the game at the syntactic and compiling level, but does not play it. Each method is run with GPT-5.4, Claude Sonnet 4.6, and Kimi K2.5 as backbones (p.6).

## Key results

**GUI agents can playtest, but still fall short of humans.** Level pass rate within 20 attempts: 0.82 for GPT-5.4, 0.79 for Sonnet 4.6, 0.72 for Kimi K2.5, and 0.92 for humans (Table 1, p.3).

**Playtesting brings a clear improvement.** Average rubric pass rate (%) on PlaytestArena (Table 3, p.6):

| Backbone | Direct LLM | OpenGame | Play2Code |
|---|---|---|---|
| GPT-5.4 | 31.6 | 55.7 | 72.3 |
| Sonnet 4.6 | 29.7 | 55.3 | 71.1 |
| Kimi K2.5 | 27.8 | 45.5 | 56.9 |
| Average of the three | 29.7 | 52.2 | 66.8 |

- The comparison with OpenGame is the most telling: it already builds and inspects iteratively and lacks only playtesting. Failures such as unresponsive controls, missing state transitions, or win conditions that never trigger are invisible at the code and compilation level and surface only during play (p.6).
- Scores rise monotonically across rounds. Early rounds mostly expose basic interaction failures, while later rounds address finer issues such as visual effects (p.7).

**Ablations (GPT-5.4 backbone, p.18–19):**

| Setting | Rubric pass rate |
|---|---|
| Full Play2Code | 72.3 |
| No memory at all | 64.1 |
| GUI playtester removed; the Game Agent verifies its own code | 58.7 |

Removing the GUI playtester causes a larger drop than removing any single memory layer, so the authors regard feedback from actual play as the most impactful component (p.19).

**Is the judge reliable?** At the criterion level, the GPT-5.5 judge agrees with humans 87.9% of the time (κ = 0.66), while humans agree with each other 91.4% of the time (κ = 0.68). With Claude Opus 4.8 and Gemini 3.5 Flash as judges, the ordering of the three methods stays the same (p.4–5).

**Comparison with human playtesters.**

- Every action, observation, and diagnosis of the GUI agent is logged, so a failure can be traced back to a concrete sequence of frames; a human report is a summary written afterwards (p.7).
- On 50 moderate-gain games, the GUI agent approaches human playtesters in driving refinement (p.7).
- Different models notice different things: GPT-5.4 concentrates on functionality bugs and control responsiveness, Sonnet 4.6 is more even and also attends to pacing and difficulty balance, and nearly half of Kimi K2.5's feedback concerns visual and aesthetic aspects (p.7–8).

**Cost.** The average inference cost per game is about 5 USD for Direct LLM, 12 USD for OpenGame, and 17 USD for Play2Code (Table 8, p.18).

## Limitations and open questions

- Only HTML web games are handled; native-engine and 3D games are left to future work (p.9).
- The benchmark is a curated set of games selected for quality control and does not cover the full diversity of real-world games (p.9).
- All games share one memory schema; game-specific or genre-aware memory structures may work better (p.9).
- The authors acknowledge that human playtesters remain irreplaceable: a felt sense of difficulty, frustration, boredom, and surprise, as well as aesthetic judgment, is something no agent currently approximates (p.7).
- Games are grouped by the refinement gain actually observed. The authors caution that this does not measure intrinsic difficulty and does not show that complexity determines the gain (p.7).
- Play2Code costs more than both baselines (p.18).

## Related concepts

- [GUI Agent](../concepts/gui-agent.md)
- [Playtesting](../concepts/playtesting.md)
- [Game Generation](../concepts/game-generation.md)
- [Agent Memory](../concepts/agent-memory.md)
- [Evaluation Benchmark](../concepts/evaluation-benchmark.md)

## Relation to other sources

- [LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning (2025)](2025-llm-game-rule-understanding.md): both papers build evaluation methods related to games, but they evaluate different things. That paper evaluates a model, asking whether an LLM can work out game states from text rules, with ground truth computed by a simulator. This paper evaluates generated games, judged against human-written criteria by an agent that actually plays them. Their motivations are also close: both want AI to give feedback during game development. This paper does not cite that one; the comparison is this wiki's own synthesis.

## My notes

