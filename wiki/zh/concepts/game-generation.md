---
title: 遊戲生成（Game Generation）
aliases: [AI 遊戲生成, 遊戲程式生成, game code generation, 規則生成, 遊玩片段生成, gameplay generation, 程序化內容生成, procedural content generation, PCG]
updated: 2026-11-17
sources: [2026-gui-agents-continual-game-generation, 2025-llm-game-rule-understanding, 2025-world-and-human-action-models]
---

## 一句話定義

讓 AI 依照人給的提示（例如自然語言描述或參考畫面），自動產生遊戲或遊戲的一部分，例如程式碼、規則、關卡或遊玩片段。

## 說明

遊戲生成可以發生在不同層次。最完整的是直接產生可執行的遊戲程式；較局部的則是產生規則、關卡或素材，再交給既有的引擎執行。還有一種做法不產生程式，而是由模型直接生成遊玩的畫面。

它比一般的程式生成困難，原因在於「對不對」很難定義。程式能執行不代表遊戲能玩，能玩也不代表符合原本的設計意圖。因此遊戲生成的研究往往同時要處理兩個問題：怎麼生成，以及怎麼判斷生成的結果好不好。

## 各來源怎麼說

### GUI Agents for Continual Game Generation（2026）

[這篇論文](../sources/2026-gui-agents-continual-game-generation.md)生成的是完整的網頁遊戲（HTML/CSS/JS），並把既有做法整理成三種（p.2、p.6）：

- **靜態生成**：從提示一次產生整個遊戲，之後不再修正。
- **端到端的代理式系統**：分多步產生程式，並在執行階段檢查，但只到語法與編譯層次。
- **持續演化**：論文提出的做法，讓寫程式與[試玩](playtesting.md)反覆對話。

三種做法的平均準則通過率依序為 29.7%、52.2% 與 66.8%（p.6）。論文的結論是，生成與評測都需要一個實際去玩的角色（p.8）。

### LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning（2025）

[這篇論文](../sources/2025-llm-game-rule-understanding.md)本身不生成遊戲，但把生成當作長期目標。它關心的是規則層次：希望 LLM 能在遊戲開發中評估、修改或生成規則（p.1）。

論文把這類任務稱為「規則空間」，並指出它需要模型考慮所有可能的盤面與動作，比只推演單一步驟困難（p.3）。作者認為要做到這一點，模型必須先能理解並泛化到沒看過的規則，所以論文先研究比較基礎的[遊戲規則理解](game-rule-understanding.md)（p.1、p.3）。

### World and Human Action Models towards gameplay ideation（2025）

[這篇論文](../sources/2025-world-and-human-action-models.md)生成的不是可以執行的遊戲，而是遊玩片段：一連串的遊戲畫面與玩家的手把動作（p.3）。把它收進這一頁是比較寬的用法。論文自己的定位是延續計算創意與程序化內容生成（procedural content generation）的研究傳統（p.1）。

與前兩篇相比有幾點不同：

- **產出**：由[世界模型](world-model.md) WHAM 直接生成畫面，背後沒有遊戲程式或明文規則（p.3）。
- **提示方式**：不用文字描述。使用者選幾格畫面作為起點，也可以直接修改畫面或輸入手把動作來引導生成（p.7）。
- **目的**：不是做出成品，而是在發想階段讓創作者快速看到多種可能的發展（p.6–7）。作者說明這不是可以直接放進開發流程的工具（p.9）。
- **範圍**：模型只用一個既有的遊戲訓練，生成的內容都在這個遊戲的範圍內（p.3）。使用者可以把元件放到原本沒有的位置，但論文沒有示範生成新的遊戲。

作者認為這類模型的價值在於能從資料學到複雜領域的結構，不必再為每個領域手工打造（p.1、p.7）。

### 來源之間的比較

| | 2026 GUI Agents | 2025 規則理解 | 2025 WHAM |
|---|---|---|---|
| 生成什麼 | 可執行的網頁遊戲 | 不生成；以規則的生成為長期目標 | 遊玩片段（畫面與動作） |
| 提示 | 文字描述 | （不適用） | 起始畫面、修改過的畫面、手把動作 |
| 怎麼判斷好壞 | 代理人試玩後依準則判定 | 逐題檢查規則是否套用正確 | 一致性、多樣性與持續性 |
| 人的角色 | 提供提示與評分準則 | 設計規則與題目 | 在生成過程中反覆挑選與修改 |

三篇處理的層次不同。GUI Agents 生成整個可執行的遊戲，並從成品的行為判斷好壞。規則理解那篇關心規則本身，並認為理解規則是生成規則的前提。WHAM 不產生遊戲本體，而是生成「玩起來可能是什麼樣子」的片段，供人在動手製作之前發想。三篇之間沒有互相引用，這段比較是本 Wiki 的整理。

## 相關概念

- [遊戲測試（Playtesting）](playtesting.md)：判斷生成結果能不能玩的方法。
- [遊戲規則理解（Game Rule Understanding）](game-rule-understanding.md)：生成與修改規則的前提能力。
- [評測基準（Evaluation Benchmark）](evaluation-benchmark.md)：衡量生成品質的方式。
- [世界模型（World Model）](world-model.md)：不寫程式，直接生成遊玩畫面的模型。
- [創意支援（Creativity Support）](creativity-support.md)：生成用來協助人發想，而不是產出成品。
