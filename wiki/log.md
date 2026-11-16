# 更新紀錄

只能往下新增，不可修改既有紀錄。標題格式：`## [YYYY-MM-DD] 類型 | 標題`。

## [2026-11-16] setup | 建立專案

- 建立 `CLAUDE.md`、四個範本、樣式表、中英文首頁與空的 Wiki 目錄。
- `raw/` 放入三篇待處理論文。

## [2026-11-16] ingest | LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning

- 新增來源頁：`2025-llm-game-rule-understanding`
- 新增概念頁：`game-rule-understanding`、`out-of-distribution-generalization`、`fine-tuning`、`game-description-language`
- 更新既有頁面：無（這是第一篇來源）
- 以上各頁皆含中英 Markdown 與中英 HTML；兩個首頁各新增 1 張來源卡片與 4 張概念卡片。

## [2026-11-16] ingest | GUI Agents for Continual Game Generation

- 新增來源頁：`2026-gui-agents-continual-game-generation`
- 新增概念頁：`gui-agent`、`playtesting`、`game-generation`、`agent-memory`、`evaluation-benchmark`
- 其中 `game-generation` 與 `evaluation-benchmark` 同時收錄 `2025-llm-game-rule-understanding`，是第一批有兩個來源的概念頁。「評測」這個共同點由擁有者提出。
- 更新既有頁面：
  - `2025-llm-game-rule-understanding`：補上「與其他來源的關係」，相關概念新增兩項。
  - `fine-tuning`：相關概念新增 `agent-memory`。
  - `game-rule-understanding`、`game-description-language`、`out-of-distribution-generalization`：相關概念新增 `evaluation-benchmark`（第一項另加 `game-generation`）。
- 以上各頁的中英 Markdown 與中英 HTML 皆已同步；兩個首頁各新增 1 張來源卡片與 5 張概念卡片。

## [2026-11-16] ingest | World Models

- 新增來源頁：`2018-world-models`
- 新增概念頁：`world-model`、`variational-autoencoder`、`evolution-strategies`
- 更新既有頁面：
  - `out-of-distribution-generalization`：加入本篇（把握度高，論文 p.9 明確提到偏離訓練分布），並新增「來源之間的比較」。現為 2 個來源。
  - `game-rule-understanding`：加入本篇（把握度中，論文未使用此說法，由本 Wiki 連結，擁有者同意），並新增「來源之間的比較」，註明頁首定義為工作定義。現為 2 個來源。
  - `2025-llm-game-rule-understanding`：「與其他來源的關係」補上本篇。
- 刻意不連結：`agent-memory`（本篇的 M 是神經網路內部狀態，意義不同）、`game-generation`（模仿既有遊戲而非創造新遊戲）、`evaluation-benchmark`（只使用現成任務）、`2026-gui-agents-continual-game-generation`（目的與方法皆不同）。前兩者在 `world-model` 頁加了避免混淆的說明。
- 規則更新：`CLAUDE.md` 6.1 第 3 步新增「更新既有頁面須附把握度，並列出刻意不連結之處」。
- 以上各頁的中英 Markdown 與中英 HTML 皆已同步；兩個首頁各新增 1 張來源卡片與 3 張概念卡片，並更新 2 張概念卡片的來源數量。
