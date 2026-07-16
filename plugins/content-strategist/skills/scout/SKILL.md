---
name: scout
description: 資訊採集雷達（圖書館員的採集技能）。首跑透過幾輪問答建立你的「來源地圖」——從內建的分級預設清單（Tier 1 官方一手源／Tier 2 領域意見領袖／Tier 3 觀察名單）勾選＋自訂來源，之後每次執行（或排程每天跑）從 RSS 與開放 API 抓新內容、依相關度評分挑出 top N、每條附「為什麼值得你讀」存進 Inbox/，再接 /ingest 進知識庫。Use when 使用者說「scout」「幫我掃一下我領域的新資訊」「今天有什麼值得讀」「設定資訊雷達」「我想追蹤某個領域／某些來源」「每天幫我搜某領域」。Do NOT use for：已有檔案要入庫（ingest）、查知識庫（kb-search）、一次性查資料（直接搜尋即可）。
---

# scout

原則：**輕資產、來源誠實**。scout 走 RSS 與開放 API——拿得到的拿好，拿不到的明講。

## Setup（首跑，或使用者說「重新設定來源」）

**1. 問答建檔.** 來回問（一次最多兩題，不是問卷轟炸）：①想追什麼領域？具體想回答什麼問題？②已經在看哪些來源（人、網站、頻道）？③語言偏好（繁中／英文／混合）？④每次要幾條（預設 3）？

**2. 出示預設清單.** 讀 [`references/sources/`](references/sources/) 對應領域的清單，依 Tier 展示讓使用者勾選：
- **Tier 1 官方一手源**：機構與官方部落格、期刊資料庫——curation 時權重最高
- **Tier 2 領域意見領袖**：長期產出、有既有信譽的個人
- **Tier 3 觀察名單**：聚合器、社群、值得試看的新秀
**收錄準則**：預設清單只收「夠大、夠權威、夠上游——多數人的引用來源」的一手資訊生產者；個人追蹤的在地創作者、中文自媒體屬「自訂來源」，setup 第 3 步再加。
沒有對應領域 → 現場搜尋提候選並全部標【未驗證】，驗證後才進 config。

**3. 自訂來源探測.** 使用者補的每個來源，先猜常見 RSS pattern 再**實際抓取驗證**：一般網站 `/feed`、`/rss`、`/index.xml`；Substack `＋/feed`；YouTube `youtube.com/feeds/videos.xml?channel_id=…`（不穩時換 `openrss.org/feeds/youtube/…`）；Podcast 用節目 RSS。驗證的實務陷阱見 [`references/fetch-notes.md`](references/fetch-notes.md)。
抓不到 → 誠實分級，不硬撐：**搜尋訊號源**（之後只靠網路搜尋摘要提及，不承諾原文）或**手動源**（教使用者自己貼連結進 Inbox）。Threads／Facebook／Instagram／Reddit 一律屬此類，setup 時就講明。

**4. 寫 `KB/scout.config.md`.** 來源表：名稱／tier／通道（rss｜api｜search-signal｜manual）／URL／驗證日期。寫完立刻跑一次 Run 讓使用者看到成果。

## Run（每次執行；排程掛的也是這個）

**1. 抓取.** 逐源抓 feed（web fetch）；API 源用其開放介面（如 PubMed E-utilities）。失效的 feed 記進報告，不略過不硬編。
**2. 去重.** `KB/log.md` 記錄已見連結（append-only），重複跳過。
**3. Curation.** 依「與使用者關注問題的相關度 × 新穎度 × 來源 tier」挑 top N。每條寫一句**為什麼值得你讀**——必須連到使用者 setup 時說的關注問題，不得只是標題改寫。
**4. 落檔.** `Inbox/scout-YYYY-MM-DD.md`：每條＝標題／連結／來源（tier）／三句內摘要／為什麼值得讀。抓得到全文的附全文（讓 ingest 走 Shape A）；抓不到的附連結＋摘要並標明「未含全文」。
**5. 收尾.** 報告本次掃了幾源、幾條新內容、feed 健康狀態；問「哪幾條要 ingest？」——ingest 是另一個 skill，不自動跑。

## 排程

使用者說「每天早上跑」→ 建排程任務掛 Run mode。

## 硬邊界

- **來源與內容零虛構**：沒實際抓到的，不寫摘要、不出現在清單；feed URL 沒驗證過不進 config。
- **只寫** `Inbox/`、`KB/scout.config.md`、`KB/log.md`——KB 其他區與使用者手寫區一律不碰（入庫是 ingest 的事、開卡是 daily-review 的事）。
- 社群平台（Threads／FB／IG／Reddit）**不承諾原文**，setup 時主動說明原因。
- Curation 是建議：讀不讀、收不收，拍板永遠是使用者。
