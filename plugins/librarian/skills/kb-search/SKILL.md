---
name: kb-search
description: >
  用自然語言查詢知識庫：對 KB/Permanent（人寫的權威層）與 KB/Wiki、Literature（AI 的 candidate 層）做關鍵字擴展搜尋，回傳排名清單——每條附「為什麼相關」與層級標示，永久卡優先。Use when 使用者說「查 KB」「查知識庫」「我有沒有寫過關於 X 的卡」「知識庫裡有什麼跟 X 有關」「kb search」。Do NOT use for: 收東西進庫（ingest／capture）、開卡（daily-review）、上網查資料（那是 web search）、對查到的內容代寫文章（centaur-writer）。
---

# kb-search — 問你的盒子

唯讀操作：**只搜、只讀、不改任何檔**。出手前確認 vault 已初始化（有 `librarian.config.md`）；沒有就先跑 librarian-setup。

## 流程

1. **展開查詢詞**：把使用者的自然語言拆成 3–6 個關鍵字與同義詞（含英文對應詞——卡名與 aliases 可能混用中英）。
2. **分層搜尋**（用檔案搜尋工具，命中後讀該檔確認相關性）：
   - 第一層 `KB/Permanent/`——檔名（卡名＝主張句，命中檔名通常就是命中主張）、aliases、正文、typed edges。
   - 第二層 `KB/MOCs/`——入口卡命中＝送整條路徑。
   - 第三層 `KB/Wiki/` 與 `KB/Literature/`——candidate 層。
3. **排名輸出**，每條四欄：

```
1.（永久卡）褪黑激素是身體的「天黑了」訊號，不是安眠藥
   為什麼相關：直接回答你問的「褪黑激素到底做什麼」
   連結：支持→調時差的主角是光；來源：Literature/長途飛行與生理時鐘
2.（Wiki·AI 候選）Concepts/褪黑激素 —— AI 整理頁，尚未經你消化
```

永久卡排前面；Wiki／Literature 標明是 candidate 層。找不到就直說「盒子裡沒有」，並提示最接近的 2–3 張——不硬湊。

## 引用紀律（紅線 5）

使用者拿搜尋結果去寫東西時：**永久卡可以引（那是他自己的話）；Wiki 頁不能當事實來源引用**——要引事實，順著 Wiki 頁的錨點回到 Raw／Annotations 的原文。回答裡遇到這種情況要主動提醒一句。
