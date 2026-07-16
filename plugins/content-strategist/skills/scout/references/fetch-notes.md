# 抓取與驗證實務筆記（2026-07-15 實測）

- **XML 被判二進位**：web fetch 對 `Content-Type: application/xml`／`application/rss+xml` 常回 `[binary data]`。此時「Content-Type 是合法 feed MIME」即為端點存在的證據；要讀內容可經 `https://r.jina.ai/<原始網址>` 代理（代理偶有舊快取，需交叉比對）。`text/xml`（如台灣央行）可直接讀。
- **Reddit**：reddit.com 在 Cowork 環境被整域封鎖，且 Reddit 對非瀏覽器存取回 403。subreddit 一律降級為「搜尋訊號源」。
- **PubMed 個人 feed**：搜尋結果 RSS 必須由使用者在 pubmed.ncbi.nlm.nih.gov 對自己的搜尋字串按「Create RSS」產生帶 token 的專屬網址——skill 引導操作步驟，**不可代生 URL**。
- **已知無公開 RSS**（2026-07 驗證，多 pattern 皆失敗）：Anthropic 官方 blog、The Batch（deeplearning.ai）——設為「搜尋訊號源」。另 The Verge、Ars Technica 被 Cowork 平台封鎖（403 blocklist）。
- **HBR 陷阱**：`hbr.org/rss` 是 404，真 feed 在 `https://feeds.harvardbusiness.org/harvardbusiness/`。
- **YouTube feed 不穩**：官方 `videos.xml?channel_id=` 偶爾靜默 24–48h，備援 `openrss.org/feeds/youtube/<channel_id>`。
