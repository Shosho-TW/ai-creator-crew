---
name: daily-review
description: >
  每日回顧：掃描知識庫裡「還沒處理的東西」（新的劃線註解、status: open 的靈感筆記），整理成候選卡清單——每張附 AI 建議卡名、為什麼值得開卡、以及相關既有卡的連結候選——讓使用者對每張三選一（開卡／略過／之後再說）。開卡時 AI 只預填記帳欄與連結候選，**正文與連結理由永遠留給使用者親手寫**。Use when 使用者說「每日回顧」「daily review」「今天有什麼該開卡的」「處理一下 inbox」「開卡」。Do NOT use for: 收新東西進庫（ingest／capture）、查詢（kb-search）、AI 代寫卡片正文（永遠不做）。
---

# daily-review — 每天一次，把值得的升級成永久卡

這是整套系統唯一「生產性摩擦」的現場：用自己的話寫卡、自己決定連結，是使用者長理解的地方。所以本 skill 的分工死線：**AI 撈候選、排上桌、跑腿記帳；判斷與寫字是人的**。出手前讀 vault 根目錄 `CLAUDE.md`（紅線五條）；沒有就先跑 librarian-setup。

## Step 1｜掃描（AI）

- `KB/Fleeting/` 裡 `status: open` 的靈感筆記（全部）。
- `KB/Literature/` 裡尚未 mined 的條目——**有 note:: 的劃線優先**（使用者寫過反應的才是強訊號）；含強評價字眼（「太重要」「要記起來」）置頂；純劃線無 note 預設不進候選（雜訊控制）。
- 每週第一次執行時加掃：`status: seedling` 超過 30 天沒動的卡、沒有任何連結的孤兒卡。

## Step 2｜候選清單（AI 排上桌）

每張候選給四樣，在對話裡列出：

1. **建議卡名**（一句宣告句，可被反駁的主張，不是主題詞——「褪黑激素是身體的訊號」✅、「褪黑激素筆記」❌）。卡名只是建議，使用者可改。
2. **為什麼**：一句話——這條為什麼值得成為永久卡。
3. **來源**：出處錨點（哪則 fleeting／哪條劃線）。
4. **相關既有卡 chips**：搜 `KB/Permanent/` 找出可能相關的 2–3 張，按方向分組列出（可能支持／可能反駁／可能延伸）——**只列候選，關係成不成立、理由是什麼，使用者決定**。

## Step 3｜三選一（人）

- **開卡** → 進 Step 4。
- **略過** → fleeting 翻 `status: processed` 移 `_processed/`；劃線標 `skipped`（不再出現）。
- **之後再說** → 標 `snoozed: <日期>`；14 天沒動自動歸檔，不製造罪惡感佇列。

## Step 4｜開卡（人寫，AI 記帳）

1. AI 建檔 `KB/Permanent/{卡名}.md`，照 Templates/tpl-permanent.md：frontmatter（type: permanent／status: seedling／author: human／created／modified／source_refs 帶錨點／aliases）**全部 AI 預填**；**正文空白**，只留一行提示「用自己的話，一卡一概念」。
2. 使用者口述或打字**正文**——AI 原話放入，不改寫、不擴寫、不潤飾。**正文空白不算開卡完成**，下次回顧會再出現。
3. 連結：使用者從 chips 挑（或自己說），**並給一句理由**；AI 才把 `支持:: [[對方卡]] — 理由` 機械寫入本卡（方向一律本卡→對方，反向靠 backlinks，不鏡像寫入）。使用者沒給理由 → 問一次，不代擬。

## Step 5｜善後（AI）

處理完的 fleeting 翻狀態移 `_processed/`；Literature 補 `mined_concepts` 與 `status: mined`；更新 `KB/index.md`；`KB/log.md` append 今天的異動（不改歷史）。最後回一句戰報：開了幾張、略過幾張、盒子裡現在幾張卡。

## 節奏建議（使用者問才講）

一天幾張都好，零張也是合法的一天——這套系統靠複利不靠爆發；比張數更重要的是「寫的那幾張是自己的話」。
