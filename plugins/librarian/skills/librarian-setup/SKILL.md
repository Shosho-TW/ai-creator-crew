---
name: librarian-setup
description: >
  Librarian 知識庫的一次性安裝：在使用者掛載的筆記資料夾（如 Obsidian vault）建立人馬卡片盒的完整骨架——人的上層（Fleeting／Literature／Annotations／Permanent／MOCs）、AI 的下層（Raw／Wiki／index／log，Karpathy 式 LLM wiki）、權限規則 CLAUDE.md、共用設定檔 librarian.config.md、三種筆記範本與寫作草稿區。Use when 使用者說「安裝知識庫」「初始化 vault」「建卡片盒」「librarian setup」「開始用 Librarian」，或其他 Librarian skill（capture／ingest／daily-review／kb-search／centaur-writer）發現 vault 缺 librarian.config.md 需要先初始化時。Do NOT use for: 已初始化 vault 的日常操作。
---

# librarian-setup — 建立你的人馬卡片盒

人馬卡片盒＝上下兩層：**上層是你的**（親手寫的卡與連結，AI 不碰正文）；**下層是 AI 的**（原文倉庫與 AI 編的 wiki，替你備料）。這個 skill 把兩層的地基一次打好。**冪等**：已存在的資料夾與檔案一律不覆蓋，只補缺的。

## 流程

### 1. 確認 vault

工作區裡有掛載的筆記資料夾 → 用選擇題確認用哪個、以及三件事：①要不要一併建寫作草稿區（給 centaur-writer 用，預設要）；②資料夾名稱用預設（下表）還是自訂；③**空盒子還是含示範內容**——示範內容＝6 張互連的概念卡＋1 張範例永久卡＋圖譜配色，讓第一次開 Obsidian 就看得到一張連好的小網（預設含）。沒有掛載資料夾 → 請使用者先把筆記資料夾加進工作區，停。

### 2. 建結構（缺什麼補什麼）

```
<vault>/
├── CLAUDE.md                  ← 權限規則（見下）
├── librarian.config.md        ← 共用設定檔
├── Inbox/                     ← capture 工具的落點（Clipper／Snipd／Kobo 匯入），等 ingest 處理
├── Templates/                 ← 三種筆記範本＋兩個劃線快捷範本
│   ├── tpl-fleeting.md
│   ├── tpl-literature.md
│   ├── tpl-permanent.md
│   ├── highlight.md           ← Templater：選字→==劃線==
│   └── annotate.md            ← Templater：跳輸入框→下一行 note::
├── KB/
│   ├── Fleeting/              🧑＋AI capture｜靈感筆記（含 _processed/ 子夾）
│   ├── Literature/            🤖 render｜文獻筆記
│   ├── Annotations/           🤖 store｜劃線與註解原始庫
│   ├── Permanent/             🔒 正文 human-only｜永久卡
│   ├── MOCs/                  🧑｜索引入口（AI 不自建）
│   ├── Raw/                   🤖｜原文倉庫（Karpathy 下層）
│   ├── Wiki/                  🤖｜AI 編的 wiki（Sources / Concepts / Entities）
│   ├── index.md               🤖 generated｜全庫索引
│   ├── log.md                 🤖 append-only｜異動日誌
│   └── taste-profile.md       🤖 raw 區｜ingest HITL 的 defer／exclude 品味紀錄
├── Planning/                  ← 內容企劃產出區（topic-picker 題目表、title-first 標題候選）
└── Drafts/articles/_reports/  （選配）centaur-writer 草稿與報告區
```

### 3. 寫入 vault 的 CLAUDE.md（權限表＋紅線）

CLAUDE.md 內容固定為：上表的權限標示，加上**五條紅線**（所有 Librarian skill 的單一事實來源，其他 skill 出手前都要讀這份）：

1. AI 絕不寫 `KB/Permanent/` 的正文與 status；只能維護記帳欄（created／modified／source_refs／aliases）。
2. AI 寫下的每個事實宣稱都要附來源錨點，能溯回 Raw 或 Annotations。
3. Wiki（candidate 層）不冒充永久卡：AI 產的頁面 frontmatter 必帶 `author: ai`。
4. AI 不自建 MOC——MOC 是人的擠壓點，AI 只能在人開口時提供候選清單。
5. Concept 與產出的終端證據只能是 Sources／Raw／Annotations，不得拿另一個 Wiki 頁當事實來源（防自我餵食）。

### 4. 寫 librarian.config.md

```yaml
---
type: librarian-config
vault_root: .
drafts_folder: Drafts/articles
reports_folder: Drafts/articles/_reports
format: md
---
```

centaur-writer 等 skill 依「skill 專屬 config → librarian.config.md → 問使用者」順序查找。

### 5. 範本內容

- **tpl-fleeting**：frontmatter `type: fleeting / created / via / status: open`＋一行提示「原話一字不動，處理完就丟」。
- **tpl-literature**：frontmatter `type: literature / source_kind / slug / status: unprocessed`＋結構「劃線（blockquote）＋ note::（用自己的話）」。
- **tpl-permanent**（照人馬卡片盒規格）：檔名＝一句宣告句；frontmatter `type: permanent / status: seedling / author: human / created / modified / source_refs: [] / aliases: []` 全是 AI 記帳區；正文提示「一卡一概念，用自己的話（AI 不代筆）」；文末三行 `支持:: `／`反駁:: `／`延伸:: `——連結＋理由同一行，方向一律「本卡→對方」，理由是人的判斷不可省略。

### 5.5 兩個 Templater 快捷範本＋手動安裝指引

`Templates/highlight.md` 內容：`==<% tp.file.selection() %>==`
`Templates/annotate.md` 內容：`<% "\n" %>note:: <% tp.system.prompt("你的想法") %>`

這兩個範本配 **Templater**（社群 plugin）用：選字按快捷鍵變 `==劃線==`、再按一鍵跳輸入框把想法寫成下一行的 `note::`——正是 ingest 解析的格式。**skill 裝不了 plugin 本體**（Obsidian 的安全閘），所以建檔之後给使用者這段指引：

1. Obsidian 設定 → Community plugins → 搜「Templater」→ 安裝並啟用。
2. Templater 設定 → Template folder location 填 `Templates`。
3. Templater 設定 → Template Hotkeys → 加入 `highlight.md` 與 `annotate.md`。
4. Obsidian 設定 → 快捷鍵 → 搜範本名 → 建議綁 `Mod+Shift+H`（劃線）與 `Mod+Shift+A`（註記）——**用 Mod 不用 Ctrl**，Mac 上才會自動變 Cmd。
5. 測試：選字按劃線鍵 → 變 `==字==`；按註記鍵 → 輸入 → 下一行出現 `note:: ...`。

（拿到預裝好的 Starter Vault 的使用者跳過這段——快照裡已綁好。）

### 5.7 示範內容（選「含示範」才做）

- 把 `assets/starter/` 的 6 張概念卡複製到 `KB/Wiki/Concepts/`、1 張範例永久卡複製到 `KB/Permanent/`。這些是隨包的示範素材（frontmatter 帶 `demo: true`、範例卡標【可刪除】），原樣複製、不是 AI 現場代寫——紅線 1 不適用於隨包範本。
- 把 `assets/graph.json` 複製到 `<vault>/.obsidian/graph.json`（`.obsidian/` 不存在就建立；已有 graph.json 則不覆蓋、改為提示使用者手動合併）。
- 提示使用者：用 Obsidian 開啟這個資料夾，打開 Graph view——應看到 7 個節點連成的小網。

### 6. 驗收

列出建立／已存在清單給使用者過目，一句話說明下一步：用 Obsidian 開啟資料夾看圖譜；丟第一則靈感筆記用 capture，讀完東西用 ingest，隔天早上跑 daily-review。
