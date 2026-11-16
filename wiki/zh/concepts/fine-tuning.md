---
title: 微調（Fine-Tuning）
aliases: [fine-tune, finetuning, 監督式微調]
updated: 2026-11-16
sources: [2025-llm-game-rule-understanding]
---

## 一句話定義

拿一個已經預訓練好的模型，用特定任務的資料繼續訓練，讓它在該任務上表現更好。

## 說明

預訓練模型已經具備廣泛的一般知識，微調是在這個基礎上用少量的任務資料調整參數，成本遠低於從頭訓練。

微調的主要風險是過度專精：模型在訓練過的資料類型上進步，卻可能失去原本的通用性。所以評估微調時，只看訓練過的任務並不夠，還要看沒訓練過的相近任務有沒有跟著進步或退步。

訓練資料的設計會影響結果，例如樣本數量、涵蓋的類型有多廣，以及答案是否附帶推理過程。

## 各來源怎麼說

### LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning（2025）

[這篇論文](../sources/2025-llm-game-rule-understanding.md)用接龍的遊戲推進題目微調 GPT-4o-mini，並比較四種多樣性不同的訓練集（ft0 到 ft3）（p.7）。發現如下：

- **少量樣本就有效。** 單一遊戲 200 個樣本就讓 Blind Alley's 的下一盤面準確率從 0.14 升到 0.82（p.9）。
- **樣本不是越多越好。** 100、200、300 三種樣本數中，200 的效果最好（p.8）。
- **多樣性的取捨。** 訓練的變體越多，每個變體分到的樣本越少，分布內表現可能下降，但分布外表現通常提升（p.9）。
- **附上推理過程有幫助。** 訓練資料含逐步的合法性解釋，作者認為這有助於合法性判斷的泛化；下一盤面沒有對應的解釋，泛化效果就較不穩定（p.8）。
- **小模型可以追上大模型。** 微調後的 GPT-4o-mini 在多數情況下追上或超越未微調的 DeepSeek-v3 與 Llama 3.3 70B（p.8）。

作者也說明，微調後的表現仍不令人滿意（p.9）。

## 相關概念

- [分布外泛化（Out-of-Distribution Generalization）](out-of-distribution-generalization.md)：檢驗微調是否過度專精的方式。
- [遊戲規則理解（Game Rule Understanding）](game-rule-understanding.md)：這裡微調要改善的能力。
- [代理人記憶（Agent Memory）](agent-memory.md)：另一種讓系統從經驗進步的方式，差別在於不改變模型參數。
