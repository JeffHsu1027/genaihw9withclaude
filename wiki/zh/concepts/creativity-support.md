---
title: 創意支援（Creativity Support）
aliases: [creativity support, 創意發想, ideation, 發散思考, divergent thinking, 反覆修改, iterative practice, 一致性, consistency, 多樣性, diversity, 持續性, persistency]
updated: 2026-11-17
sources: [2025-world-and-human-action-models]
---

## 一句話定義

用工具協助人產生並發展新的想法，重點是輔助人的創作過程，而不是代替人完成作品。

## 說明

創作通常不是一次到位。常見的描述是兩種思考交替進行：發散思考（divergent thinking）盡量產生多種不同的可能，收斂思考再從中挑選並打磨。過程中創作者會反覆修改，往往要看到成果才知道哪裡不對。

創意支援工具的設計因此關心幾件事：能不能快速提供多個選項、能不能讓人直接動手調整、調整之後工具會不會沿用，以及人是否仍然覺得作品是自己的。最後一點常稱為創作主導權（creative agency）。

這與一般衡量 AI 的角度不同。自動化任務看的是正確率與效率；創意支援看的是工具能否配合人的工作方式。一個輸出品質很高、卻無法被引導或修改的模型，對創作者的幫助可能有限。

## 各來源怎麼說

### World and Human Action Models towards gameplay ideation（2025）

[這篇論文](../sources/2025-world-and-human-action-models.md)從使用者研究出發。作者訪談 27 位遊戲創作者，歸納出兩個主題（p.2–3）：

- 發散思考產生的多樣點子，必須放在一致的遊戲世界中才有意義。
- 創作者要能掌控反覆修改的過程，例如直接修改畫面，並讓模型沿用這些修改。

作者把它們轉成三種可以評估的模型能力（p.3）：

- **一致性（consistency）**：生成內容前後連貫，並符合遊戲機制。
- **多樣性（diversity）**：同一個起點能產生多種有意義差異的結果。
- **持續性（persistency）**：使用者的修改會被保留在後續生成中。

論文接著訓練 WHAM 模型並分別評估這三項，再做出概念原型 WHAM Demonstrator，示範分支生成、回頭改選與直接修改畫面等互動方式（p.6–7）。

論文的主張是：模型的評估應該由創作者的需求來決定，而不是只衡量任務完成的效果與效率；為創作而做的模型也不會是最終產品，而是創作流程中的一個環節（p.7）。作者也指出，過去的創意支援工具需要為特定領域手工定義結構，生成模型則可以從資料中學到這些結構（p.1）。

## 相關概念

- [世界模型（World Model）](world-model.md)：這篇論文用來支援發想的模型類型。
- [遊戲生成（Game Generation）](game-generation.md)：這裡生成的是供發想用的遊玩片段。
- [評測基準（Evaluation Benchmark）](evaluation-benchmark.md)：三種能力各有對應的衡量方式。
- [遊戲規則理解（Game Rule Understanding）](game-rule-understanding.md)：一致性要求生成內容符合遊戲機制。
