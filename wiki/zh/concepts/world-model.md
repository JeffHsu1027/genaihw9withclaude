---
title: 世界模型（World Model）
aliases: [world model, 環境模型, dynamics model, 動態模型, 基於模型的強化學習, model-based RL, MDN-RNN, WHAM]
updated: 2026-11-17
sources: [2018-world-models, 2025-world-and-human-action-models]
---

## 一句話定義

對環境的模型，能根據目前的狀態與動作預測接下來會發生什麼；可以用來訓練代理人（World Models），也可以直接用來生成內容（WHAM）。

## 說明

世界模型的想法來自[強化學習](reinforcement-learning.md)，它大致有兩條路線。一條不建立環境的模型，直接從嘗試錯誤中學習。另一條先學一個環境的模型，再利用它來規劃或訓練，稱為基於模型的強化學習（model-based RL）。

世界模型屬於後者，通常做兩件事：把高維度的觀察（例如畫面）壓縮成精簡的表示，以及預測這個表示如何隨動作變化。

有了世界模型，代理人可以在「想像」中練習，不必每一步都接觸真實環境。真實互動很慢、很貴或有風險時特別有價值。

主要的風險是模型不可能完全準確：代理人在模型裡學到的做法若利用了模型的錯誤，搬到真實環境就會失效。

世界模型也可以不搭配代理人，單獨當作生成模型：給它一段開頭，讓它生成接下來的發展（WHAM）。

## 各來源怎麼說

### World Models（2018）

[這篇論文](../sources/2018-world-models.md)的世界模型由兩部分組成（p.2–3）：

- **視覺 V**：用[變分自編碼器](variational-autoencoder.md)把畫面壓縮成潛在向量 z。
- **記憶 M**：用 MDN-RNN 預測下一個 z 的機率分布。輸出分布而非單一數值，是為了處理環境的隨機性。

世界模型以無監督方式訓練，不接觸獎勵，只負責壓縮與預測（p.5）。論文示範了它的兩種用途：

- **提供特徵**。把 z 與 M 的隱藏狀態 h 交給一個很小的控制器。賽車任務中，只用 z 得到 632 ± 251 分，加上 h 後達到 906 ± 21 分（p.6）。
- **取代真實環境**。Doom 任務中，控制器完全在世界模型產生的「夢境」裡訓練，搬回真實環境後約存活 1100 步，超過 750 步的過關標準（p.7–8）。

論文也詳細記錄了風險：控制器會找到利用模型缺陷的策略，例如讓夢境裡的怪物永遠不發射火球（p.8）。作者的對策是讓 M 輸出機率分布，並在取樣時提高溫度 τ 使夢境更難、更隨機。溫度是在真實性與可被利用的程度之間的取捨（p.9）。

作者指出的限制包括模型容量有限，以及只能一步一步模擬、缺乏階層式規劃（p.12–13）。

### World and Human Action Models towards gameplay ideation（2025）

[這篇論文](../sources/2025-world-and-human-action-models.md)說明自己延續世界模型的研究路線，並引用上面那一篇（p.3）。它的模型 WHAM 同樣分成壓縮與預測兩部分，但元件不同（p.3–4）：

- **壓縮**：用 VQGAN 把每格畫面轉成 540 個離散 token（見[向量量化](vector-quantization.md)）。
- **預測**：用 16 億參數的[自回歸 Transformer](autoregressive-transformer.md)，在畫面與手把動作交錯的序列上預測下一個 token。

有三點與上一篇不同：

- **也預測人的動作。** 模型名稱中的 Human Action 指的就是這件事：它除了預測畫面，也預測玩家接下來會按什麼（p.3）。因此它可以連畫面帶動作一起生成完整的遊玩片段（p.10）。
- **資料來自人類。** 訓練資料是 Bleeding Edge 約 50 萬條玩家軌跡，超過 7 年的遊玩時間（p.3、p.10），而不是隨機策略收集的紀錄。
- **用途是給人看。** 這個世界模型不是用來訓練代理人，而是讓遊戲創作者生成、比較與修改遊玩片段，屬於[創意支援](creativity-support.md)（p.1、p.6–7）。

評估方式也跟著改變。論文不看代理人的分數，而是看生成的畫面是否一致（FVD）、動作是否涵蓋人類行為的範圍（Wasserstein 距離），以及使用者的修改是否被保留（p.4–7）。

作者表示模型掌握了遊戲環境的 3D 結構、手把動作的效果與遊戲的時間結構（p.3），1.6B 模型可以生成長達 2 分鐘仍然一致的片段（p.4–5）。模型的上下文只有 1 秒（p.3）。

### 來源之間的比較

| | World Models（2018） | WHAM（2025） |
|---|---|---|
| 用途 | 訓練代理人：提供特徵，或取代真實環境 | 讓人類創作者發想 |
| 畫面的表示 | 變分自編碼器的連續潛在向量 | VQGAN 的離散 token |
| 序列模型 | MDN-RNN | 自回歸 transformer |
| 預測什麼 | 下一個潛在向量 | 下一格畫面與玩家的下一個動作 |
| 訓練資料 | 隨機策略的遊玩紀錄 | 人類玩家的遊玩紀錄 |
| 環境 | CarRacing 與 VizDoom | 3D 多人對戰遊戲 Bleeding Edge |
| 怎麼評估 | 代理人在真實環境的分數 | 生成片段的一致性、多樣性與持續性 |

兩篇的骨架相同：先把畫面壓縮，再預測壓縮後的表示如何變化。七年之間改變的是規模、元件與資料來源，以及世界模型的使用者從代理人變成了人。2018 那篇擔心的是控制器會利用模型的錯誤；2025 這篇關心的是模型能不能被人引導與修改。後者引用了前者，但表中的逐項對照是本 Wiki 的整理。

## 相關概念

- [變分自編碼器（Variational Autoencoder）](variational-autoencoder.md)：World Models（2018）的世界模型中負責壓縮畫面的部分。
- [演化策略（Evolution Strategies）](evolution-strategies.md)：World Models（2018）用來訓練控制器的方法。
- [分布外泛化（Out-of-Distribution Generalization）](out-of-distribution-generalization.md)：World Models（2018）的世界模型在訓練資料沒涵蓋的狀態會出錯。
- [遊戲規則理解（Game Rule Understanding）](game-rule-understanding.md)：世界模型從畫面隱含地學到遊戲如何運作。
- [強化學習（Reinforcement Learning）](reinforcement-learning.md)：世界模型這個想法的來源，也是 World Models（2018）的問題設定。
- 注意：World Models（2018）的「記憶 M」指神經網路的內部狀態，與[代理人記憶（Agent Memory）](agent-memory.md)把經驗寫成文字保存是不同的東西。
- [遊戲生成（Game Generation）](game-generation.md)：World Models 的世界模型只模仿既有的遊戲，不創造新內容；WHAM 則把世界模型用來生成供發想用的遊玩片段。
- [向量量化（Vector Quantization）](vector-quantization.md)：WHAM 用來壓縮畫面的方法。
- [自回歸 Transformer（Autoregressive Transformer）](autoregressive-transformer.md)：WHAM 用來預測未來的模型。
- [創意支援（Creativity Support）](creativity-support.md)：WHAM 的世界模型所服務的目的。
