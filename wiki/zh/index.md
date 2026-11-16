# 目錄

Wiki 所有頁面的清單。每次吸收新論文後更新。

## 來源

- [LLM Game Rule Understanding through Out-of-Distribution Fine-Tuning](sources/2025-llm-game-rule-understanding.md)（2025）— 用接龍變體測試 LLM 的規則理解；少量微調能泛化到沒訓練過的變體，但規則差異太大時無效。
- [GUI Agents for Continual Game Generation](sources/2026-gui-agents-continual-game-generation.md)（2026）— 讓 GUI 代理人實際試玩生成的網頁遊戲，用於評測（PlaytestArena）與改進生成（Play2Code）。
- [World Models](sources/2018-world-models.md)（2018）— 把代理人拆成視覺、記憶、控制器；學出世界模型後用它訓練極小的控制器，甚至完全在模型的「夢境」中訓練。

## 概念

- [遊戲規則理解（Game Rule Understanding）](concepts/game-rule-understanding.md)（2 個來源）— 模型能否正確掌握一套遊戲規則，並用它來判斷、預測、遊玩，甚至評估與修改規則本身。
- [分布外泛化（Out-of-Distribution Generalization）](concepts/out-of-distribution-generalization.md)（2 個來源）— 模型在與訓練資料性質不同的資料上，仍然能維持表現的能力。
- [微調（Fine-Tuning）](concepts/fine-tuning.md)（1 個來源）— 拿一個已經預訓練好的模型，用特定任務的資料繼續訓練，讓它在該任務上表現更好。
- [遊戲描述語言（Game Description Language）](concepts/game-description-language.md)（1 個來源）— 一種用來精確描述遊戲規則的形式化語言，讓程式可以直接讀取規則並模擬遊戲。
- [GUI 代理人（GUI Agent）](concepts/gui-agent.md)（1 個來源）— 透過觀看畫面並用滑鼠與鍵盤操作圖形介面的 AI 代理人，使用軟體的方式和人一樣。
- [遊戲測試（Playtesting）](concepts/playtesting.md)（1 個來源）— 藉由實際遊玩來找出遊戲的問題，檢查的是玩起來的體驗，而不是程式碼本身。
- [遊戲生成（Game Generation）](concepts/game-generation.md)（2 個來源）— 讓 AI 依照自然語言的描述，自動產生遊戲或遊戲的一部分，例如程式碼、規則或關卡。
- [代理人記憶（Agent Memory）](concepts/agent-memory.md)（1 個來源）— 讓 AI 代理人把過去的經驗保存下來，並在之後的步驟或任務中取用的機制。
- [評測基準（Evaluation Benchmark）](concepts/evaluation-benchmark.md)（2 個來源）— 一組固定的任務加上評分方法，用來以可重複的方式衡量某種能力或某種產出的品質。
- [世界模型（World Model）](concepts/world-model.md)（1 個來源）— 代理人內部對環境的模型，能根據目前的狀態與動作預測接下來會發生什麼。
- [變分自編碼器（Variational Autoencoder）](concepts/variational-autoencoder.md)（1 個來源）— 一種神經網路，學習把輸入壓縮成一個小的潛在向量，並能從這個向量重建出近似的原始輸入。
- [演化策略（Evolution Strategies）](concepts/evolution-strategies.md)（1 個來源）— 一類模仿演化的最佳化方法：反覆產生多組候選參數，評估每一組的表現，再根據表現好的候選產生下一代。
