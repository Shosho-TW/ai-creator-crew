# AI Creator Crew 使用教學

> 版本 v0.2.1｜更新 2026-07-15｜對應 plugin：librarian 0.2.1、content-strategist 0.2.0

## 1. 安裝（一次）

1. Claude Desktop（Cowork）→ Customize → Plugins → **Add marketplace** → 貼 `https://github.com/Shosho-TW/ai-creator-crew`
2. 對 **librarian** 和 **content-strategist** 各按 Install，重開一個新對話。

## 2. 建環境（一次，5 分鐘）

1. 在電腦上建一個空資料夾（例：`文件/我的知識庫`）。
2. Cowork 左側 **Projects** → **+** → 選 **Use an existing folder** 指向剛建的資料夾 → 命名（例：`我的知識庫`）。之後每次在這個 Project 開對話，資料夾自動就位、記憶持續累積。
3. 在 Project 裡開新對話，說：**「初始化我的知識庫，要示範內容」**。
4. 用 Obsidian 開啟該資料夾 → 打開 Graph view，會看到 7 個節點連成的小網。
5. （建議）Project 設定的 Instructions 填一句：「這個 Project 是我的人馬卡片盒知識庫，用 librarian 與 content-strategist 的 skill 維護它。」

## 3. 日常怎麼用（對 Claude 講人話就會觸發）

| 情境 | 你說 | 誰上工 | 產出去哪 |
|---|---|---|---|
| 冒出一個想法 | 「記一下：……」 | capture | `KB/Fleeting/` |
| 設定資訊來源（一次） | 「設定我的資訊雷達，領域是X」 | scout | `KB/scout.config.md` |
| 每天掃新資訊 | 「幫我掃今天的新資訊」（可排程） | scout | `Inbox/` |
| 讀完一篇文章 | 用 Obsidian Web Clipper 抓進 `Inbox/`、劃線 `==…==`、想法寫 `note::`，然後說「ingest 這篇」 | ingest | `KB/Literature/`＋`KB/Wiki/`（你逐條拍板後） |
| 每天整理 | 「每日回顧」→ 對候選卡三選一，正文自己寫 | daily-review | `KB/Permanent/` |
| 找過去的筆記 | 「查我知識庫有沒有關於X的」 | kb-search | 對話（附出處） |
| 不知道做什麼題目 | 「幫我選題，主題是X，受眾是Y」 | topic-picker | `Planning/topics-…` |
| 幫題目想標題 | 「幫我想標題，題目是……」 | title-first | `Planning/titles-…` |
| 想寫一篇文章 | 「用我的口述起稿」 | centaur-writer | `Drafts/articles/` |

## 4. 建議的第一週

- 第 1 天：建環境 → Clip 一篇你喜歡的文章 → 劃線＋note:: → ingest → 看圖譜長出第一條線。
- 第 2 天起：早上「每日回顧」開 1–2 張卡；讀到好文章就 ingest。
- 週末：「幫我選題」把下週要做的內容定下來。

## 5. 疑難排解

- **剛裝完或更新完，第一次呼叫 skill 出現「Unknown skill」**：skill 註冊需要一點時間。重開一個新對話，或把同一句話再說一次即可。
- **skill 沒被觸發**：改用直接點名，例如「用 topic-picker 幫我選題」（用 skill 名，不加 plugin 前綴）。

## 6. 更多文件

- 首航逐步教學：[plugins/librarian/docs/first-loop.md](plugins/librarian/docs/first-loop.md)
- 各工具抓文章的方法：[plugins/librarian/docs/capture-guide.md](plugins/librarian/docs/capture-guide.md)
- 完整技術文件：[plugins/librarian/docs/full-documentation.md](plugins/librarian/docs/full-documentation.md)
- 安裝疑難：[plugins/librarian/INSTALL.md](plugins/librarian/INSTALL.md)
