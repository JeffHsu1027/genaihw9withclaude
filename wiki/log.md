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

## [2026-11-17] ingest | World and Human Action Models towards gameplay ideation

- 新增來源頁：`2025-world-and-human-action-models`
- 新增概念頁：`creativity-support`、`vector-quantization`、`autoregressive-transformer`
- 更新既有頁面：
  - `world-model`：加入本篇（把握度高，論文 p.3 明確說明延續世界模型研究並引用 World Models），新增「來源之間的比較」；別名新增 WHAM；相關概念新增三個新概念，並把原本「世界模型與遊戲生成不同」的註記改寫為兩篇的對照。現為 2 個來源。
  - `2018-world-models`：「與其他來源的關係」補上本篇（把握度高，有直接引用）。
  - `evaluation-benchmark`：加入本篇（把握度中，論文提出評估方法並釋出測試軌跡，但未自稱 benchmark），「來源之間的比較」擴充為三欄。現為 3 個來源。
  - `game-rule-understanding`：加入本篇（把握度中，論文未使用此說法，由本 Wiki 連結，擁有者同意），「來源之間的比較」擴充為三欄。現為 3 個來源。
  - `game-generation`：加入本篇（把握度中偏低，生成的是遊玩片段而非可玩的遊戲）。經擁有者同意放寬一句話定義，提示不再限於自然語言，產出納入遊玩片段；「說明」補一句，「來源之間的比較」改為三欄表格。現為 3 個來源。
  - `variational-autoencoder`：只在相關概念新增 `vector-quantization`，未列為來源。
- 刻意不連結：`out-of-distribution-generalization`（持續性實驗把元件放到沒出現過的位置，但論文未以分布外的框架討論，只寫在來源頁；擁有者同意）、`playtesting` 與 `gui-agent`（WHAM 生成遊玩畫面，不是去玩或測試遊戲）、`agent-memory`（持續性是保留使用者的修改，與保存經驗無關）、`fine-tuning`、`evolution-strategies`、`game-description-language`（論文未使用）。
- 未寫入「與其他來源的關係」：`2025-llm-game-rule-understanding`、`2026-gui-agents-continual-game-generation`。本篇沒有引用它們；已查過 2026 那篇的全文，也沒有引用本篇。
- 以上各頁的中英 Markdown 與中英 HTML 皆已同步；兩個首頁各新增 1 張來源卡片與 3 張概念卡片，並更新 4 張概念卡片的來源數量（`game-generation` 的卡片文字同步改為新定義）。

## [2026-11-17] lint | 檢查 Wiki（第四篇來源吸收後）

- 範圍：4 個來源頁、15 個概念頁（中英 Markdown 與中英 HTML 共 76 個頁面檔）、兩個首頁、兩份 `index.md`、`raw/README.md`。
- 沒有問題的項目：失效連結 0；沒有來源連到的概念頁 0；Markdown 與 HTML 不一致 0；缺少語言版本 0；中英版的區塊結構、頁碼與連結集合一致；首頁卡片無缺漏，來源數量正確；來源之間「與其他來源的關係」皆為雙向。
- 更正上一筆吸收時的回報：當時指出 `2025-llm-game-rule-understanding` 的 HTML 與 Markdown 有一處差異，原因是檢查用的轉換程式沒有處理 `<https://…>` 這種自動連結寫法；頁面本身正確，沒有修改任何 Wiki 檔案。
- 回報給擁有者、尚未修改的項目：
  - `world-model`：一句話定義與「說明」以「代理人內部的模型」與基於模型的強化學習為前提，已不涵蓋第二個來源（WHAM 不是給代理人用的）；「相關概念」中三條以「這裡」開頭的說明只適用於 World Models（2018）。
  - 在多個頁面出現但沒有自己頁面的概念：大型語言模型（LLM，9 個頁面、2 篇來源）、強化學習（5 個頁面、2 篇來源）。
  - `variational-autoencoder` 的別名含「自編碼器、autoencoder、潛在空間」等通用詞，與新建的 `vector-quantization` 有重疊。
  - `wiki/log.md` 第一筆紀錄的類型是 `setup`，不在 `CLAUDE.md` 規定的三種類型內。
- 未發現互相矛盾的敘述。
- 本次沒有修改任何頁面，只新增這筆紀錄。

## [2026-11-17] lint | 依健檢結果修改

- 依擁有者對上一筆健檢的決定修改：
  - `world-model`：一句話定義放寬為不限於代理人，並依擁有者要求以括號標註來源（World Models、WHAM）；「說明」補上不搭配代理人的用法；刪除 WHAM 小節中「本頁開頭的說法不完全適用」一句；「相關概念」中原本以「這裡」「這篇論文」指稱的四條改為明寫 World Models（2018）。
  - 新增概念頁 `reinforcement-learning`（來源：`2018-world-models`、`2025-llm-game-rule-understanding`，含「來源之間的比較」）。兩個來源頁的 `concepts` 與「相關概念」各加一項；`world-model`、`evolution-strategies`、`fine-tuning` 的「相關概念」各加一條連結。
  - `variational-autoencoder`：別名移除「自編碼器、autoencoder」。
- 擁有者決定不建立大型語言模型（LLM）的概念頁，理由是屬於常識內容。
- 未處理：`wiki/log.md` 第一筆紀錄的 `setup` 類型，等擁有者決定是否修改 `CLAUDE.md`。
- 以上各頁的中英 Markdown 與中英 HTML 皆已同步；兩個首頁各新增 1 張概念卡片，並更新 `world-model` 卡片的定義文字。所有「我的筆記」區塊未更動。

## [2026-11-17] lint | 規則更新：log 類型

- 經擁有者同意修改 `CLAUDE.md` 6.1 第 11 步：註明 `setup` 類型只用於建立專案時的第一筆紀錄，之後不再使用。上一筆健檢留下的未處理項目至此結清。
- 沒有修改任何 Wiki 頁面。
