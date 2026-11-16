# 遊戲 × AI 論文 Wiki ｜ Games × AI Paper Wiki

以 LLM Wiki 方式維護的論文知識庫，主題是 AI 如何生成、理解與遊玩遊戲。中英並存。

A paper knowledge base maintained in the LLM Wiki style, about how AI generates, understands, and plays games. Available in Chinese and English.

## 運作方式

1. 把論文 PDF 放進 `raw/`。
2. 對 AI 說「處理新論文」。
3. AI 依照 [`CLAUDE.md`](CLAUDE.md) 的規則閱讀論文，撰寫 `wiki/` 內的 Markdown，同步成網頁，並更新首頁。

## 結構

| 位置 | 內容 |
|---|---|
| `CLAUDE.md` | 給 AI 的規則：頁面格式、命名、工作流程、檢查清單 |
| `raw/` | 原始論文。PDF 只留在本機，不上傳；[連結清單在此](raw/README.md) |
| `wiki/zh/`、`wiki/en/` | Wiki 本體（Markdown），內容的唯一來源 |
| `index.html`、`sources/`、`concepts/` | 中文網站 |
| `en/` | 英文網站 |
| `templates/`、`assets/` | 頁面範本與樣式表 |

## 兩種頁面

- **來源頁**：一篇論文一頁。
- **概念頁**：一個概念一頁，整合不同論文的說法。
