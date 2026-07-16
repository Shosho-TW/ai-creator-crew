---
name: capture
description: >
  靈感筆記快速捕捉：把使用者丟出的一句話、一個念頭，原封不動存進知識庫的 KB/Fleeting/，加上時間戳與 status: open，等每日回顧處理。Use whenever 使用者說「記一下」「capture」「靈感筆記」「丟進 inbox」「幫我記住這個想法」，或明顯是在拋一個未成形的點子要暫存（不是要討論、不是要寫作）。Do NOT use for: 讀完文章要入庫（ingest）、開永久卡（daily-review）、查知識庫（kb-search）、待辦事項或行程（那不是筆記系統的事）。
---

# capture — 接住，就好

靈感筆記的唯一任務是接住。這一步的摩擦越小越好，所以本 skill 只做一件事、不問任何問題。

## 規則

1. **原話一字不動**。不改寫、不補完、不修錯字——修掉的可能正是靈感的線索。
2. **不整理、不分類、不建連結**。那是每日回顧的事。
3. **一則一檔**：`KB/Fleeting/{YYYY-MM-DD-HHmm}-{原話前幾個字}.md`。
4. frontmatter（AI 記帳，使用者不用管）：

```yaml
---
type: fleeting
created: 2026-07-13T22:10:00
via: cowork
status: open
---
（原話）
```

5. 存好回一句短確認（「接住了」＋檔名），**不要**開始討論那個想法——使用者現在要的是放下，不是展開。他若接著說「跟我聊聊這個」才切換成對話。
6. vault 沒有 `librarian.config.md` → 先跑 librarian-setup。
7. 使用者問「我有哪些沒處理的靈感」→ 列 `KB/Fleeting/` 裡 status: open 的清單（日期＋原話），提醒可跑 daily-review，僅此而已。

## 別搞混：這裡收「想法」，不收「材料」

capture 收的是**你腦中冒出的一句話**。讀到的文章、影片字幕、podcast 片段、電子書劃線是**材料**——它們由 capture 工具（Web Clipper／Media Extended／Snipd／Kobo importer）落進 `Inbox/`，讀完由 **ingest** 處理。使用者丟來一整篇文章或一批劃線 → 指去 ingest，不要存成靈感筆記。

## 生命週期（給你判斷用，不用唸給使用者聽）

capture（本 skill）→ 隔天 daily-review 出現在候選 → 開卡／併入既有卡／丟掉 → AI 把 status 翻 processed 並移入 `KB/Fleeting/_processed/`。靈感筆記是暫存區，不是收藏品。
