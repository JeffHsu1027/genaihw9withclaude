---
title: 向量量化（Vector Quantization）
aliases: [VQ, VQ-VAE, VQGAN, ViT-VQGAN, 離散 token, discrete token, 影像 tokenizer, image tokenizer, 碼本, codebook]
updated: 2026-11-17
sources: [2025-world-and-human-action-models]
---

## 一句話定義

把連續的向量換成一份有限碼本中最接近的那一個條目，讓資料可以用一串離散的編號來表示。

## 說明

語言模型處理的是離散的 token，影像卻是連續的像素。向量量化是讓影像也能變成 token 的常見做法。

流程是：編碼器先把影像壓縮成一小格一小格的向量，每個向量再換成碼本（codebook）中最接近的條目，只留下該條目的編號。一張影像就變成一串整數。解碼器再從這些編號還原出近似的影像。碼本的大小就是「詞彙量」。

VQ-VAE 是這種做法的代表。VQGAN 在訓練時另外加入感知損失與對抗損失，讓還原的影像在高壓縮率下看起來仍然清晰。

主要的取捨是 token 數量。每張影像用越多 token，還原品質越好，但序列越長，後續模型的運算越慢，同樣的上下文長度能容納的影像也越少。

## 各來源怎麼說

### World and Human Action Models towards gameplay ideation（2025）

[這篇論文](../sources/2025-world-and-human-action-models.md)用 VQGAN 把每一格遊戲畫面轉成離散 token，作者稱把資料表示成離散 token 序列是整個做法的關鍵（p.3）。

論文訓練了兩個編碼器（p.10）：

- 較小的模型（15M–894M）：約 60M 參數的 VQGAN，把 128 × 128 的畫面編成 256 個 token，詞彙量 4,096。
- 1.6B 模型：約 300M 參數的 ViT-VQGAN，把 300 × 180 的原始解析度畫面編成 540 個 token，詞彙量 16,384。

兩者都先用重建損失與感知損失訓練，再加入 GAN 損失（p.10）。

作者指出每張畫面的 token 數是關鍵的超參數，要在畫面品質、生成速度與上下文長度之間取捨（p.3）。論文中可以看到取捨的兩面：

- 1.6B 模型使用較高解析度，重建品質的上限較高，一致性指標 FVD 因此更好（p.4）。
- 同一個模型在動作多樣性上略差。作者推測是畫面 token 變多、詞彙量變大，使動作 token 在損失中的比重下降（p.5）。

手把動作也被離散化：按鍵本來就是離散的，搖桿座標切成 11 個區間（p.3）。這一步是直接分段，不經過碼本。

## 相關概念

- [變分自編碼器（Variational Autoencoder）](variational-autoencoder.md)：同樣把影像壓縮成精簡的表示，但得到的是連續向量。
- [自回歸 Transformer（Autoregressive Transformer）](autoregressive-transformer.md)：接收這些離散 token 並預測下一個。
- [世界模型（World Model）](world-model.md)：向量量化在這裡負責世界模型中壓縮畫面的部分。
