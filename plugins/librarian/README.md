# Librarian — 人馬卡片盒 Plugin

> 版本 v0.2.0｜更新 2026-07-15


在你的 Markdown 筆記資料夾（如 Obsidian vault）上，用 Claude 跑一套人機分工的知識庫：**Centaur Zettelkasten（人馬卡片盒）**。

分工只有一句話：**省時間的交出去，省思考的留下來。** AI 做檢索、轉錄、渲染、歸檔、索引、查證——做再多也不會讓你變聰明的事；你做用自己的話寫卡、決定卡與卡的連結、文章的每一個定稿句子——理解在這些地方長出來。

這個分界有實證基礎：逐字級的 AI 建議會讓不同作者的產出彼此變像（[CHI 2025](https://arxiv.org/abs/2409.11360)）、會反向改變作者本人的態度且事先警告無效（[Science Advances 2026](https://www.science.org/doi/10.1126/sciadv.adw5578)）。所以整套 plugin 給你的永遠只有**問題、候選與標記**——沒有任何可以直接「接受」的正文字句。

## 六個 skill

| Skill | 做什麼 | 你說什麼觸發 |
|---|---|---|
| **librarian-setup** | 一次性安裝：雙層 vault 骨架（人的上層＋AI 的下層）、權限規則、範本 | 「初始化我的知識庫」 |
| **capture** | 靈感筆記：一句話原封不動接住，等每日回顧 | 「記一下：……」 |
| **ingest** | 讀完的文章／影片／podcast／電子書劃線 → 文獻筆記＋來源摘要＋AI wiki 候選（你逐條裁決才寫入） | 「ingest 這篇」 |
| **daily-review** | 掃出候選卡讓你三選一；開卡時**正文與連結理由永遠你親手寫** | 「每日回顧」 |
| **kb-search** | 自然語言查庫，人寫的權威層優先、AI 候選層標明 | 「我寫過 X 的卡嗎？」 |
| **centaur-writer** | 口述先行的寫作流程：你講、AI 保序整理＋反問，定稿每一句是你的 | 「我要寫一篇文章」 |

## 快速上手（只需要一條路）

1. 瀏覽器裝 [Obsidian Web Clipper](https://obsidian.md/clipper)，看到好文章整篇 clip 進 vault 的 `Inbox/`。
2. 在筆記軟體裡讀它：好句子劃線（`==...==`），想法寫在劃線下一行（`note:: 你的想法`）——用 Starter Vault 的話有快捷鍵。
3. 讀完跟 Claude 說「**ingest 這篇**」，然後隔天早上說「**每日回顧**」。

裝好後的第一圈完整走法（含初始卡與完成判準）：**[docs/first-loop.md](docs/first-loop.md)**。影音與電子書等其他來源見 [docs/capture-guide.md](docs/capture-guide.md) 的進階附錄——skill 都吃得動，但課程只需要文章這條。

## 安裝

見 [INSTALL.md](INSTALL.md)——最短路徑：把本 repo 網址貼給你的 Claude，說「照 README 帶我安裝」。

## 五條紅線（這是 plugin 存在的理由，不是可調參數）

1. AI 絕不寫 `KB/Permanent/`（永久卡）的正文與狀態，只維護記帳欄。
2. AI 寫下的每個事實宣稱附來源錨點，能溯回原文。
3. AI 產的 wiki 頁永遠標 `author: ai`，不冒充你的筆記。
4. AI 不自建 MOC（索引入口是人的擠壓點）。
5. 事實的終端證據只能是原文層，不得拿 AI 的 wiki 頁互當來源（防自我餵食）。

完整規格：[docs/full-documentation.md](docs/full-documentation.md)｜材料匯入教學（文章／YouTube／Podcast／Kobo 四路線）：[docs/capture-guide.md](docs/capture-guide.md)｜示範 vault：[examples/小雲的卡片盒](examples/小雲的卡片盒)（用 Obsidian 開，Graph View 已配好分層配色；內容為虛構示範）。

## License

MIT（見 LICENSE）。原始方法論出自 Sönke Ahrens《卡片盒筆記》與 Niklas Luhmann 的實踐；AI 下層設計參考 Andrej Karpathy 的 LLM wiki 構想。
