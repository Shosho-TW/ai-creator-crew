# 小雲的卡片盒（課程示範 vault）

> ⚠️ 全部內容為**示意**：小雲是《張修修的AI創作者新世紀》虛構的示範學員；睡眠科學內容未經查證，示範的是**筆記的形式與流程**，不是醫學事實。每個檔案 frontmatter 都帶 `demo: true`。

這個 vault 由 Librarian plugin 的六個 skill 建成，對照如下：

| 動作 | Skill | 產物 |
|---|---|---|
| 建雙層骨架＋規則＋範本 | librarian-setup | 資料夾結構、CLAUDE.md、librarian.config.md、Templates/ |
| 接住三則靈感 | capture | KB/Fleeting/（一則已處理進 _processed/） |
| 讀完一篇文章入庫 | ingest | KB/Annotations/＋KB/Literature/＋KB/Raw/＋KB/Wiki/Concepts/ |
| 隔天早上開卡 | daily-review | KB/Reviews/ 的回顧單＋KB/Permanent/ 三張卡（含 typed edges） |
| 查詢 | kb-search | （唯讀，見回顧單末尾的示範查詢） |
| 寫文章 | centaur-writer | Drafts/articles/（本示範尚未產草稿） |

結構分兩層：**上層是小雲的**（Fleeting／Literature 的 note::／Permanent／MOCs——正文與連結理由都是人寫）；**下層是 AI 的**（Raw／Wiki／index／log，Karpathy 式 LLM wiki，只當備料層）。
