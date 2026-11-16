---
title: 自回歸 Transformer（Autoregressive Transformer）
aliases: [autoregressive transformer, causal transformer, 因果 transformer, decoder-only transformer, 下一個 token 預測, next-token prediction, GPT, 自回歸模型, autoregressive model]
updated: 2026-11-17
sources: [2025-world-and-human-action-models]
---

## 一句話定義

一種序列模型，每次只根據前面已有的 token 預測下一個 token，再把結果接回序列繼續往下生成。

## 說明

Transformer 用注意力機制讓序列中的每個位置參考其他位置。自回歸的版本加上因果遮罩（causal mask），每個位置只能看到自己之前的內容，也稱為 causal transformer。GPT 系列就是這種架構。

訓練時給一段序列，要模型在每個位置預測下一個 token，用交叉熵損失衡量誤差。生成時從模型輸出的機率分布取樣一個 token，接到序列後面，再預測下一個。因為是取樣，同一個開頭可以產生不同的後續。

這種模型不限於文字。只要資料能轉成離散 token，影像、聲音或動作都能用同一套方法處理。

主要的限制是上下文長度：模型只看得到最近固定數量的 token，更早的內容不會影響預測。

## 各來源怎麼說

### World and Human Action Models towards gameplay ideation（2025）

[這篇論文](../sources/2025-world-and-human-action-models.md)用自回歸 transformer 作為序列預測的主幹（p.3）。

- **輸入序列**：畫面 token（經[向量量化](vector-quantization.md)得到）與手把動作 token 交錯排列。序列中沒有分隔符號，模型要從位置嵌入判斷下一個該預測畫面還是動作（Fig. 2，p.4）。
- **實作**：以 nanoGPT 修改而成的 GPT-2 架構，用交叉熵損失訓練下一個 token 的預測（p.10）。
- **規模**：最大的模型有 1.6B 參數，上下文長度 5,560 個 token，相當於 1 秒或 10 格畫面，訓練 20 萬次更新（p.10）。
- **提示方式**：生成時可以替換畫面或動作的 token，藉此用手把動作控制生成，或讓模型接著使用者修改過的畫面往下生成（p.3）。

作者說明選擇這個架構的理由與三種能力有關（p.3）：一致性需要能掌握畫面與動作之間依賴關係的序列模型；多樣性需要模型保留資料中的條件分布；持續性需要模型能以修改過的畫面或動作為條件。另一個理由是可擴充性，也就是資料與運算越多表現越好。

論文也仿照大型語言模型的做法分析規模：訓練不同大小的模型，觀察損失隨運算量的變化，並由 15M 到 206M 的結果外推，準確預測 894M 模型的最終損失（p.10）。

## 相關概念

- [向量量化（Vector Quantization）](vector-quantization.md)：把畫面轉成這個模型能處理的離散 token。
- [世界模型（World Model）](world-model.md)：這裡的 transformer 擔任世界模型中預測未來的部分。
- [創意支援（Creativity Support）](creativity-support.md)：取樣帶來多樣性，替換 token 帶來可修改性。
