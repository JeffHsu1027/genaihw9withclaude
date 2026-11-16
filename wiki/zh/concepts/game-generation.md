---
title: 遊戲生成（Game Generation）
aliases: [AI 遊戲生成, 遊戲程式生成, game code generation, 規則生成]
updated: 2026-11-16
sources: [2026-gui-agents-continual-game-generation, 2025-llm-game-rule-understanding]
---

## 一句話定義

讓 AI 依照自然語言的描述，自動產生遊戲或遊戲的一部分，例如程式碼、規則或關卡。

## 說明

遊戲生成可以發生在不同層次。最完整的是直接產生可執行的遊戲程式；較局部的則是產生規則、關卡或素材，再交給既有的引擎執行。

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

### 來源之間的比較

兩篇處理的層次不同。前者生成整個可執行的遊戲，並從成品的行為判斷好壞；後者關心規則本身，並認為理解規則是生成規則的前提。兩篇之間沒有互相引用，這段比較是本 Wiki 的整理。

## 相關概念

- [遊戲測試（Playtesting）](playtesting.md)：判斷生成結果能不能玩的方法。
- [遊戲規則理解（Game Rule Understanding）](game-rule-understanding.md)：生成與修改規則的前提能力。
- [評測基準（Evaluation Benchmark）](evaluation-benchmark.md)：衡量生成品質的方式。
