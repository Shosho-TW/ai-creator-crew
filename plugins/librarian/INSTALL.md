# 安裝指南

拿到這個網址的你，只要會做一件事：**把網址交給你的 Claude**。剩下的它會帶你走。

## 最短路徑（推薦）：加入市集

1. 打開 Claude 桌面版的 **Cowork** 模式 → 側欄 **Customize** → **Plugins** → **Add marketplace**。
2. 貼上市集網址：`https://github.com/Shosho-TW/ai-creator-crew`
3. 在清單裡找到 **librarian**，按 **Install**。裝完後貼這句話給 Claude：

   > 我剛裝好 librarian plugin，請一步一步帶我完成 vault 初始化。

（用 Claude Code 的人：`/plugin marketplace add Shosho-TW/ai-creator-crew` → `/plugin install librarian@ai-creator-crew`。）

## 替代路徑：把網址交給你的 Claude

貼上這段話：

> 這是 Librarian 知識庫 plugin 的 repo：https://github.com/Shosho-TW/ai-creator-crew 。請讀它的 README，然後一步一步帶我完成安裝和 vault 初始化。

照 Claude 的指示走。它會讀完文件、告訴你每一步做什麼。

## Claude 會帶你做的事（先知道全貌）

| 步驟 | 誰做 | 說明 |
|---|---|---|
| 1. 取得 skill 檔 | Claude | 從本 repo 的 `skills/` 取得個別 skill（走市集安裝則跳過此步） |
| 2. 安裝 skill | **你點一下** | 把檔案拖進 Cowork 對話，按「Save skill」——這是安全閘，Claude 不能替你按 |
| 3. 掛載筆記資料夾 | **你點一下** | 在 Cowork 把你的筆記資料夾（或新資料夾）加進工作區 |
| 4. 初始化 vault | Claude | 你說「初始化我的知識庫」，librarian-setup 建好整套結構 |
| 5. （選）劃線快捷鍵 | 你（5 分鐘） | 照 setup 給的指引裝 Templater、綁兩個快捷鍵——或直接用預裝好的 Starter Vault |
| 6. 首航 | 你＋Claude | 照 [docs/first-loop.md](docs/first-loop.md) 走完第一圈：手寫初始卡 → Clipper 抓文章劃線註記 → ingest → 每日回顧開卡 → Graph 看見第一條線 |

**用 Claude Code 的人**：步驟 2 不用點——請 Claude `git clone` 本 repo，把 `skills/` 內容放進專案的 `.claude/skills/`，全程自動。

## 常見問題

- **一定要 Obsidian 嗎？** 不用。任何 Markdown 資料夾都行；Obsidian 的 graph 與反向連結體驗最好。
- **手機呢？** 手機管捕捉（quick capture、Snipd），消化與寫作在桌機。同步方案見 README 連到的完整文件。
- **AI 會不會亂改我的筆記？** 永久卡正文它碰不了——這是紅線第一條，寫在每個 vault 的 CLAUDE.md 裡。
