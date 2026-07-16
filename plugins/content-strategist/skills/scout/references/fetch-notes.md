# 抓取與驗證註記

- **XML 被判二進位**：web fetch 對 `Content-Type: application/xml`／`application/rss+xml` 常回 `[binary data]`。「Content-Type 是合法 feed MIME」即可視為端點存在的證據；要讀內容可經 `https://r.jina.ai/<原始網址>` 代理（代理偶有舊快取，需交叉比對）。`text/xml` 可直接讀。
- **Reddit**：對非瀏覽器存取回 403，部分環境整域封鎖。subreddit 一律降級為「搜尋訊號源」。
- **PubMed 個人 feed**：搜尋結果 RSS 必須由使用者在 pubmed.ncbi.nlm.nih.gov 對自己的搜尋字串按「Create RSS」產生帶 token 的專屬網址——引導使用者操作，不可代生 URL。
- **已知無公開 RSS**：Anthropic 官方 blog、The Batch（deeplearning.ai）——設為「搜尋訊號源」。The Verge、Ars Technica 在部分環境被封鎖。
- **HBR**：`hbr.org/rss` 是 404，真 feed 在 `https://feeds.harvardbusiness.org/harvardbusiness/`。
- **YouTube feed 不穩**：官方 `videos.xml?channel_id=` 偶爾靜默 24–48h，備援 `openrss.org/feeds/youtube/<channel_id>`。
