---
title: 遊戲描述語言（Game Description Language）
aliases: [GDL, SGDL, Solitaire GDL]
updated: 2026-11-16
sources: [2025-llm-game-rule-understanding]
---

## 一句話定義

一種用來精確描述遊戲規則的形式化語言，讓程式可以直接讀取規則並模擬遊戲。

## 說明

用自然語言寫的規則容易有歧義，程式也無法直接執行。遊戲描述語言（GDL）把規則寫成固定格式，例如有哪些物件、初始狀態、可以做哪些動作、每個動作的條件，以及勝利條件。

有了 GDL，同一套程式就能模擬許多不同的遊戲，只要換一份描述檔。這對研究很有用：可以大量產生規則的變體、自動驗證某個動作是否合法、算出正確的下一個狀態，進而自動產生附帶標準答案的題目。

GDL 的表達範圍通常是設計上的取捨：語言越簡單越容易使用，但能描述的遊戲種類也越少。

## 各來源怎麼說

### LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning（2025）

[這篇論文](../sources/2025-llm-game-rule-understanding.md)提出接龍專用的 SGDL（Solitaire GDL），目標是在語言保持簡單的前提下支援大量接龍變體（p.4）。一份 SGDL 檔案有五個區段（p.4–5）：

- `$cards`：使用幾副牌、有哪些花色與牌。
- `$initial`：有哪些牌堆、初始如何發牌、牌面朝上或朝下。
- `$actions`：可以做的動作（移動一張牌、移動一疊牌、抽牌）與各自的合法條件。
- `$auto`：條件成立時自動執行的動作（選用）。
- `$win`：勝利條件。

條件可以用 And 與 Or 組合成樹狀結構（p.5）。

值得注意的是，SGDL 只用來定義遊戲與產生資料，並不直接給 LLM 看。框架會把 SGDL 轉成自然語言的規則描述再交給模型，理由是自然語言更容易泛化到其他遊戲，而且先前研究顯示 LLM 對自然語言規則的理解較好（p.4）。

## 相關概念

- [遊戲規則理解（Game Rule Understanding）](game-rule-understanding.md)：GDL 讓這項能力可以被大規模、自動化地測試。
- [評測基準（Evaluation Benchmark）](evaluation-benchmark.md)：GDL 讓題目與標準答案可以自動產生。
