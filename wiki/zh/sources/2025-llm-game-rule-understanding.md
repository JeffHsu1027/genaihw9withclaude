---
title: LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning
authors: Bahar Bateni, Benjamin Pratt, Jim Whitehead
year: 2025
venue: AIIDE 2025（第 21 屆 AAAI 人工智慧與互動數位娛樂研討會）
url: https://doi.org/10.1609/aiide.v21i1.36804
raw: raw/LLM game rule understanding through out-of-distribution fine-tuning.pdf
added: 2026-11-16
concepts: [game-rule-understanding, out-of-distribution-generalization, fine-tuning, game-description-language, evaluation-benchmark, game-generation]
---

## 一句話總結

作者用自訂的接龍遊戲描述語言自動產生「這一步合不合法、下一個盤面是什麼」的題庫，發現少量微調就能提升 GPT-4o-mini 對規則的理解，而且能泛化到沒訓練過的接龍變體；但規則差異太大時，微調幾乎沒有幫助。

## 研究問題

大型語言模型（LLM）原生對規則的理解很差。針對特定規則微調可以改善，卻犧牲了預訓練模型最大的優勢：泛化到沒看過的規則集。若要讓 LLM 協助遊戲設計（評估、修改或生成規則），這種泛化能力是必要的（p.1）。

論文提出三個問題（p.1–2）：

1. 未經訓練的 LLM 對遊戲規則的理解有多好？
2. 針對特定規則微調能改善多少？（分布內）
3. 這種微調能否泛化到規則沒看過的其他遊戲？（分布外）

## 方法

**測試平台：接龍（Solitaire）。** 接龍的規則簡單，都是依花色與點數在牌堆之間移動牌，但變體極多、玩法差異大，適合測試跨規則的泛化（p.2）。

**SGDL 與模擬框架。** 作者設計了接龍專用的[遊戲描述語言](../concepts/game-description-language.md) SGDL，一個檔案以 `$cards`、`$initial`、`$actions`、`$auto`、`$win` 五個區段定義一種變體（p.4–5）。框架據此提供可玩的遊戲、通用的遊戲代理人，以及以範本產生文字的生成器（p.4）。程式庫收錄 Spider、Klondike、Pairing、Free Cell 四個家族共 30 多種變體（p.5）。程式與資料集公開於 <https://github.com/iambb5445/SolitaireGDL>（p.4）。

**題目形式：遊戲推進（game progression）。** 每一題給定盤面與一個動作，模型要判斷動作是否合法，合法時還要輸出下一個盤面（p.2）。作者刻意選這種題型，因為它不需要搜尋，可以單純衡量規則是否被理解與正確套用（p.3）。規則以自然語言而非 SGDL 提供給模型（p.4）。

**資料集產生。** 每個變體隨機產生 100 個初始盤面，用深度優先搜尋代理人模擬到獲勝或 2000 步為止，再從走過的盤面中取樣；每個盤面配一個隨機動作，合法與不合法各半（p.6–7）。框架同時產生逐步的合法性解釋，但不解釋下一個盤面如何產生（p.5）。

**距離指標。** 以規則差異的數量計算兩個變體的距離，並正規化到 0 與 1 之間。同家族的變體距離較近；Thirteen 的規則是點數相加為 13，與其他變體差異特別大（p.6）。

**微調設定。** 所有微調都以 GPT-4o-mini 為基礎，訓練資料的多樣性逐步增加（p.7）：

| 模型 | 訓練用的變體 | 樣本總數 |
|---|---|---|
| ft0 | 1 個（Blind Alley's） | 100、200 或 300 |
| ft1 | 同一家族的 5 個 | 200 |
| ft2 | 2 個家族共 5 個 | 200 |
| ft3 | 3 個家族共 10 個 | 200 |

**測試集。** 6 個變體各 500 題，依與訓練集的關係挑選：分布內（ID）、同家族但分布外（IF）、分布外（OOD）。例如 Blind Alley's 對所有模型都是分布內，Baker's Game 對所有模型都是分布外（p.7）。

**提示與評分。** 模型以 JSON 輸出盤面、動作、逐步思考、是否合法與下一個盤面，並附 4 個少樣本範例。評分指標為合法性準確率與下一盤面準確率，後者只計算合法的動作（p.8）。

## 主要結果

**未微調時表現很差。** GPT-4o-mini 的合法性準確率平均為 0.67，而隨機猜測約為 0.5（p.8）。錯誤分為兩類：引用了錯誤的規則或盤面資訊，以及規則執行錯誤，與 RuleBench 的觸發錯誤、執行錯誤相符（p.8）。

**微調幾乎總是有幫助。** 下表為 GPT-4o-mini 的結果，每格為「合法性準確率／下一盤面準確率」（Table 5，p.9）：

| 測試變體 | 未微調 | ft0（200） | ft3 |
|---|---|---|---|
| Blind Alley's（全部分布內） | 0.63／0.14 | 0.86／0.82 | 0.78／0.59 |
| Legion（同家族、分布外） | 0.62／0.14 | 0.69／0.67 | 0.79／0.67 |
| Baker's Game（全部分布外） | 0.75／0.29 | 0.73／0.79 | 0.88／0.86 |
| Thirteen（僅 ft3 分布內） | 0.81／0.00 | 0.83／0.00 | 0.80／0.02 |

- 只用單一遊戲、200 個樣本微調，下一盤面準確率就從 0.14 升到 0.82（Blind Alley's）。樣本數 100、200、300 之中，200 的效果最好（p.8）。
- 訓練資料越多樣，分布內表現可能下降，但分布外表現提升：Baker's Game 在 ft3 達到 0.88／0.86（p.9）。
- 泛化有極限：Thirteen 的規則與其他變體差異太大，各個微調模型都沒有明顯進步（p.8–9）。
- 重新組合相似的規則比面對全新規則容易泛化。Spider 與 Klondike 家族雖然不同，卻是相似規則的不同組合（p.8）。
- 逐步的合法性解釋有助於泛化：完全分布外的資料集上，合法性準確率大致隨訓練多樣性提升；下一盤面準確率則更受「是否在分布內」影響，作者推測是因為沒有為它提供逐步解釋（p.8）。

**與較大的模型比較。** 未微調的 DeepSeek-v3 與 Llama 3.3 70B 表現明顯較好（例如 Baker's Game 分別為 0.93／0.87 與 0.87／0.67），但微調後的 GPT-4o-mini 在多數情況下能追上或超越它們（p.8–9）。

## 限制與未解問題

- 只微調了 GPT-4o-mini 一個模型，選它是因為速度快、限制少，目標是分析微調的效果而非追求最佳表現（p.7、p.9）。
- 微調後的表現仍不令人滿意（p.9）。
- 只為「動作是否合法」產生逐步解釋，沒有為「下一個盤面如何產生」產生解釋（p.9）。
- 只測試遊戲推進這一種題型，沒有涵蓋遊戲遊玩與規則空間（p.3）。
- ft3 的訓練集包含差異很大的 Thirteen，可能拖累了它在其他變體上的表現（p.8）。
- 作者提出的後續方向：加入工具使用來讀取盤面資訊、用中介的邏輯形式表示規則、以強化學習改善推理步驟（p.9）。

## 相關概念

- [遊戲規則理解（Game Rule Understanding）](../concepts/game-rule-understanding.md)
- [分布外泛化（Out-of-Distribution Generalization）](../concepts/out-of-distribution-generalization.md)
- [微調（Fine-Tuning）](../concepts/fine-tuning.md)
- [遊戲描述語言（Game Description Language）](../concepts/game-description-language.md)
- [評測基準（Evaluation Benchmark）](../concepts/evaluation-benchmark.md)
- [遊戲生成（Game Generation）](../concepts/game-generation.md)

## 與其他來源的關係

- [GUI Agents for Continual Game Generation（2026）](2026-gui-agents-continual-game-generation.md)：兩篇都在建立與遊戲有關的評測方法，但評測的對象不同。這一篇評測的是模型，問 LLM 能否依文字規則推演盤面，標準答案由模擬器算出；那一篇評測的是生成出來的遊戲，由代理人實際遊玩後依人類撰寫的準則判定。兩篇的動機也相近，都希望 AI 能在遊戲開發中提供回饋。那一篇沒有引用這一篇，以上是本 Wiki 的整理。
- [World Models（2018）](2018-world-models.md)：兩篇都要求模型「給定目前狀態與動作，預測下一個狀態」。這一篇把規則以文字明確交給 LLM，並逐題檢查答案；那一篇不提供任何規則，讓神經網路只從畫面學出遊戲如何運作。兩篇也都碰到分布外的問題：這一篇是規則差異太大時微調無效，那一篇是控制器走到訓練資料沒涵蓋的狀態時世界模型會出錯。兩篇沒有引用關係，以上是本 Wiki 的整理。

## 我的筆記

