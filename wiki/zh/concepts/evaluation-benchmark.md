---
title: 評測基準（Evaluation Benchmark）
aliases: [benchmark, evaluation, 評測, 評估, 基準測試, rubric, 評分準則]
updated: 2026-11-16
sources: [2025-llm-game-rule-understanding, 2026-gui-agents-continual-game-generation]
---

## 一句話定義

一組固定的任務加上評分方法，用來以可重複的方式衡量某種能力或某種產出的品質。

## 說明

設計評測時要回答三個問題：評的是什麼、標準答案從哪裡來、分數怎麼算。

標準答案的來源大致有三種。第一種是程式自動算出，精確且能大量產生，但只適用於有明確對錯的問題。第二種是人類標註，能處理較主觀的判斷，但成本高。第三種是讓另一個模型擔任評審，可以擴大規模，但需要另外驗證評審本身是否可靠。

好的評測還要避免一個常見問題：被評的系統若在訓練或開發過程中看過評測內容，分數就會失真。

## 各來源怎麼說

### LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning（2025）

[這篇論文](../sources/2025-llm-game-rule-understanding.md)評測的是**模型的能力**：LLM 是否理解遊戲規則。

- **任務**：給定接龍盤面與一個動作，判斷是否合法並輸出下一個盤面（p.2）。
- **標準答案**：由模擬框架自動算出。因為規則以[遊戲描述語言](game-description-language.md)定義，框架可以驗證任何動作並執行出下一個盤面（p.7）。
- **分數**：合法性準確率與下一盤面準確率（p.8）。
- **規模**：6 個測試變體各 500 題（p.7）。
- **特色**：測試集依與訓練集的關係分成分布內、同家族與分布外，用來衡量[泛化](out-of-distribution-generalization.md)（p.7）。

### GUI Agents for Continual Game Generation（2026）

[這篇論文](../sources/2026-gui-agents-continual-game-generation.md)評測的是**產出的品質**：生成出來的遊戲是否符合預期。它建立了 PlaytestArena。

- **任務**：200 個網頁遊戲生成提示，涵蓋 8 種類型（p.4）。
- **標準答案**：沒有唯一解。改用人類專家撰寫的評分準則，共 1,548 條可觀察的預期行為（p.4）。
- **分數**：由 GPT-5.5 [GUI 代理人](gui-agent.md)實際遊玩後逐條判定，取通過的比例（p.4）。
- **評審的驗證**：與人類標註的一致率為 87.9%（κ = 0.66），接近人類彼此之間的 91.4%（κ = 0.68）；換用另外兩個模型當評審，方法的排序不變（p.4–5）。
- **避免洩漏**：流程中的測試員看不到評分準則，也不負責最終評分（p.5）。

### 來源之間的比較

| | 2025 規則理解 | 2026 PlaytestArena |
|---|---|---|
| 評測對象 | 模型的能力 | 生成出來的遊戲 |
| 標準答案 | 模擬器自動算出 | 人類撰寫的準則 |
| 判定方式 | 與標準答案比對 | 代理人遊玩後判定 |
| 題目來源 | 程式自動產生 | 人類專家撰寫 |

前者的問題有明確對錯，可以完全自動化；後者的品質要玩了才知道，所以需要一個會玩的評審，並額外驗證評審是否可靠。兩篇之間沒有互相引用，這段比較是本 Wiki 的整理。

## 相關概念

- [遊戲規則理解（Game Rule Understanding）](game-rule-understanding.md)：第一篇評測的能力。
- [遊戲測試（Playtesting）](playtesting.md)：第二篇用來評測的方法。
- [遊戲描述語言（Game Description Language）](game-description-language.md)：讓題目與答案能自動產生的工具。
- [分布外泛化（Out-of-Distribution Generalization）](out-of-distribution-generalization.md)：設計測試集時的一種切分方式。
