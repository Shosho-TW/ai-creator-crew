# Librarian — 人馬卡片盒 Plugin（開源文件草案 v1）

> 本檔＝未來 GitHub repo 的 README 底稿＋完整技術文件。2026-07-13 由 Cowork 整理；【修修：…】處待拍板。

---

## 1. 這是什麼

Librarian 是一組 Claude（Cowork／Claude Code）skill，在你的 Markdown 筆記資料夾（如 Obsidian vault）上實作 **Centaur Zettelkasten（人馬卡片盒）**：一套人與 AI 分工的知識庫方法。

分工的判準是一句話：**省時間的交出去，省思考的留下來。**

- **AI 做損耗性摩擦**：檢索、轉錄、渲染、歸檔、索引、查證、格式——做再多也不會讓你變聰明的事。
- **人做生產性摩擦**：用自己的話寫卡、決定卡與卡的連結、寫文章的每一個定稿句子——理解就是在這些地方長出來的。

這個分界有實證基礎：逐字級的 AI 建議會讓不同作者的產出彼此變像（CHI 2025, arXiv:2409.11360）、會反向改變作者本人的態度且事先警告無效（Science Advances 2026, eadw5578）；而 AI 點子只當「刺激」呈現、不嵌進產出時，反而提升群體多樣性（ACM CI 2025, arXiv:2401.13481）。所以整套 plugin 的介面設計原則是：**AI 給的永遠是問題、候選與標記，沒有可以直接「接受」的正文字句。**

命名：Librarian＝圖書館員——替你顧知識庫的那位 AI 船員。

## 2. 建議的 repo 結構

```
librarian/
├── README.md                     ← 本文件的精簡版
├── LICENSE                       ← 【修修：建議 MIT（上游 my-claude-devteam 亦為 MIT）】
├── plugin.json                   ← Cowork plugin manifest（打包時由 create-cowork-plugin 生成）
├── skills/
│   ├── librarian-setup/SKILL.md
│   ├── capture/SKILL.md
│   ├── ingest/SKILL.md
│   ├── daily-review/SKILL.md
│   ├── kb-search/SKILL.md
│   └── centaur-writer/
│       ├── SKILL.md
│       └── references/
│           ├── question-rules.md
│           ├── verification-checklist.md
│           └── output-conventions.md
├── docs/
│   ├── vault-spec.md             ← §7 的獨立版
│   ├── red-lines.md              ← 紅線五條的解說版（canonical 在 vault CLAUDE.md）
│   └── design-notes.md           ← 研究依據與設計決策
├── examples/
│   └── 小雲的卡片盒/              ← 完整示範 vault（20 檔，全帶 demo: true）
└── evals/                        ← skill-creator 測試案例（見 §9）
```

## 3. Skill 一覽

| Skill | 一句話 | 寫入範圍 |
|---|---|---|
| librarian-setup | 一次性安裝：建雙層骨架、規則、設定、範本 | vault 全域（只補缺，不覆蓋） |
| capture | 靈感筆記：一句話原封不動接住 | KB/Fleeting/ |
| ingest | 讀完的文章＋劃線 → 文獻筆記＋AI wiki 候選（HITL） | Literature／Annotations／Raw／Wiki |
| daily-review | 候選卡三選一開卡；正文與連結理由人寫 | Permanent（僅記帳欄）＋善後 |
| kb-search | 自然語言查庫，分層排名 | 唯讀 |
| centaur-writer | 口述先行的原創文章流程 | Drafts/ |

觸發語與完整 prompt 見各 skill 的 SKILL.md frontmatter `description`——那就是 Claude 決定何時啟用的依據，也是本 plugin 的「API 文件」。

## 4. 安裝

三種方式擇一：

1. **單 skill 安裝**（目前）：把 `skills/<name>/` 打包成 `.skill`（zip 改副檔名），在 Claude 介面點 Save skill。六個可獨立裝，互相發現：任何 skill 發現 vault 缺 `librarian.config.md` 會自動轉呼叫 librarian-setup。
2. **Plugin 安裝**（打包後）：`plugin.json`＋全部 skills 包成 `.plugin`，一次裝整包。
3. **Claude Code**：clone repo，把 `skills/` 內容放進專案 `.claude/skills/`。

前置需求：一個掛載進 Cowork 工作區（或 Claude Code 專案）的筆記資料夾。Obsidian 非必需，但 graph、backlinks 的體驗最好。

## 5. 五分鐘上手

```
你：初始化我的知識庫              → librarian-setup 建骨架、問三個設定
你：記一下：回程好像比去程難調     → capture 存進 Fleeting
你：這篇讀完了，劃線在這（貼上）    → ingest 渲染文獻筆記、提案 wiki 候選、停下等你拍板
（隔天早上）
你：每日回顧                      → daily-review 端出候選卡，你三選一；開卡時正文你自己寫
你：我寫過咖啡因的卡嗎？           → kb-search 分層排名
你：我要寫一篇文章，口述在這……     → centaur-writer 跑口述先行流程
```

## 6. 各 skill 詳解

### 6.1 librarian-setup

- **檔案**：`SKILL.md`（單檔）。
- **職責**：冪等安裝。①確認掛載的 vault；②建雙層資料夾（見 §7）；③寫 vault 的 `CLAUDE.md`（權限表＋紅線五條——全 plugin 的單一事實來源）；④寫 `librarian.config.md`；⑤放三種筆記範本（Templates/）。已存在的檔案一律不覆蓋。
- **Prompt 設計重點**：AskUserQuestion 只問三件事（用哪個資料夾、要不要 Drafts 寫作區、資料夾命名預設或自訂）；結尾用驗收清單交代建了什麼、下一步做什麼。
- **紅線五條（寫進 vault CLAUDE.md 的內容）**：①AI 絕不寫 `KB/Permanent/` 正文與 status，只維護記帳欄；②AI 的事實宣稱附來源錨點，溯回 Raw／Annotations；③Wiki 是 candidate 層，AI 產頁必帶 `author: ai`；④AI 不自建 MOC；⑤終端證據只能是 Sources／Raw／Annotations，禁止拿 Wiki 頁互當事實來源（防自我餵食）。

### 6.2 capture

- **檔案**：`SKILL.md`（單檔）。
- **職責**：把一句話存成 `KB/Fleeting/{YYYY-MM-DD-HHmm}-{前幾字}.md`，frontmatter `type: fleeting / created / via / status: open`。
- **Prompt 設計重點**：三個「不」——不改寫原話、不分類、不展開討論。存好只回一句短確認。這是刻意的：捕捉的摩擦要趨近零，AI 在這裡多說一句話都是干擾。
- **生命週期**：capture → 隔天 daily-review → 開卡／略過 → status 翻 processed、移 `_processed/`。靈感筆記是暫存區。

### 6.3 ingest

- **檔案**：`SKILL.md`（單檔）。
- **職責**：兩步、中間強制停。Step 1：劃線原封存 `KB/Annotations/{slug}.md`（每條帶錨點 ^a1…）→ 渲染 `KB/Literature/{slug}.md`（劃線 blockquote＋使用者的 note::，不改寫）→ 全文存 `KB/Raw/`（有才存）→ 在對話列出 3–8 個 Concept 候選，**停**。Step 2：使用者逐條 accept／defer／exclude 後，才寫 `KB/Wiki/`、更新 index 與 log。
- **Prompt 設計重點**：「劃線是強調訊號，不是內容過濾器」——有全文就吃全文，AI 的 wiki 才給得出使用者漏看的東西。使用者若順口說「順便開張卡／連到某張卡」，ingest 照做入庫、那半句明確回絕（開卡走 daily-review）。
- **邊界**：整本書一次收→請一章一章來；只想要摘要不入庫→不觸發。

### 6.4 daily-review

- **檔案**：`SKILL.md`（單檔）。
- **職責**：五步。①掃描（open fleeting＋未 mined 的劃線，有 note:: 的優先；每週首跑加掃 stale seedling 與孤兒卡）；②候選清單——每張給「建議卡名（宣告句）＋為什麼＋來源錨點＋相關既有卡 chips（按支持／反駁／延伸分組）」；③使用者三選一（開卡／略過／之後再說，snooze 14 天自動歸檔）；④開卡——AI 建檔並預填 frontmatter 記帳欄，**正文空白等人寫**，空正文不算完成；連結由人挑、人給理由，AI 才機械寫入 `支持:: [[對方]] — 理由`；⑤善後記帳（翻狀態、更新 index、append log）。
- **Prompt 設計重點**：這是整套系統唯一的生產性摩擦現場，prompt 裡把分工寫成死線：「AI 撈候選、排上桌、跑腿記帳；判斷與寫字是人的」。連結方向定死「本卡→對方」，反向靠 backlinks，不鏡像寫入。
- **心理設計**：「之後再說」佇列 14 天自動歸檔——不製造罪惡感；戰報只報張數，不評價。

### 6.5 kb-search

- **檔案**：`SKILL.md`（單檔）。
- **職責**：唯讀。查詢詞擴展（含中英同義）→ 三層搜尋（Permanent 檔名／aliases／typed edges → MOCs → Wiki＋Literature）→ 排名輸出，每條附「為什麼相關」與層級標示；找不到直說，不硬湊。
- **Prompt 設計重點**：輸出格式把「永久卡（你的話）」與「Wiki（AI 候選）」在視覺上分開；並主動提醒紅線⑤——拿結果去寫作時，Wiki 頁不能當事實來源引用，要順錨點回 Raw／Annotations。

### 6.6 centaur-writer

- **檔案**：`SKILL.md`＋`references/question-rules.md`（反問三硬規則＋訪談／攻防問題庫）＋`references/verification-checklist.md`（內建查證與除味保底）＋`references/output-conventions.md`（config、檔名、frontmatter 慣例）。
- **職責**：六 Phase——開工確認 → 攤卡（只列不排）→ 作者口述（連結構一起講）→ 保序整理（v1＋三種便利貼【缺口】【重複】【待查證】）→ 反問（訪談＋攻防各一輪，錨定原句、問句無候選答案、問完不自答）→ 作者在自己的編輯器改寫 → 查證＋除味報告存新檔。
- **Prompt 設計重點**：六條紅線是流程的靈魂，其中最不直覺的兩條——「AI 對正文的新增只能是便利貼，不能是句子」與「使用者說『你先寫我再改』時改走口述流程」——各配了一個具體例子，因為這兩條是一般助手最容易破的地方。
- **與其他 skill 的關係**：Phase 1 攤卡呼叫 kb-search 的搜尋邏輯；Phase 6 若裝有更完整的查證／除味 skill 則優先用之，否則用內建 checklist。

## 7. Vault 規格（雙層結構）

```
<vault>/
├── CLAUDE.md                 ← 權限表＋紅線五條（單一事實來源）
├── librarian.config.md       ← type: librarian-config；drafts_folder / reports_folder / format
├── Templates/                ← tpl-fleeting / tpl-literature / tpl-permanent
├── KB/
│   ├── Fleeting/（_processed/）   🧑＋AI capture｜type: fleeting｜status: open→processed
│   ├── Literature/                🤖 render｜type: literature｜劃線＋人的 note::
│   ├── Annotations/               🤖 store｜劃線原始庫，錨點 ^a1…
│   ├── Permanent/                 🔒 正文 human-only｜type: permanent｜status: seedling→evergreen
│   ├── MOCs/                      🧑｜入口卡
│   ├── Raw/  Wiki/{Sources,Concepts,Entities}   🤖｜Karpathy 式下層（原文倉庫＋AI wiki）
│   ├── index.md（generated）  log.md（append-only）
└── Drafts/articles/_reports/      centaur-writer 草稿與報告
```

永久卡 frontmatter（全部是 AI 記帳區，人只寫正文與 typed edges）：

```yaml
type: permanent
status: seedling        # 升 evergreen 只能人做
author: human
created / modified      # AI 維護
source_refs: ["[[Literature/…]] ^a2"]
aliases: []
```

Typed edges 寫在正文（Dataview inline field），連結＋理由同一行、方向一律本卡→對方：

```
支持:: [[另一張卡]] — 一句人寫的理由
反駁:: …    延伸:: …
```

## 8. 示範 vault：examples/小雲的卡片盒

一個用六個 skill 從無到有走完一輪的完整 vault（20 檔）：三則靈感（一則已處理）、一篇文獻的 Annotations／Literature／Raw／Wiki 候選頁、一份每日回顧單（候選卡＋三選一實錄）、三張帶 typed edges 的永久卡、一張 MOC、index 與 log。全部 frontmatter 帶 `demo: true`；README 聲明內容為虛構示範、領域知識未查證——示範的是**形式與流程**。

## 9. 測試

每個 skill 用 skill-creator 的迴圈驗證：同一測試 prompt 跑「有 skill／無 skill」兩版，斷言逐條評分（如 centaur-writer 的斷言：保序、不確定的研究標【待查證】不寫成事實、問句無候選答案、未新增作者沒講的論點、不代寫）。現況：**centaur-writer 已過兩輪（5/5、3/3，baseline 0）；其餘五個已通過整合示範（examples/ 即產物），單獨的 eval 迴圈待補**——這是 open source 前的必辦事項。`evals/` 目錄保存測試案例與斷言。

## 10. Roadmap

- **Wave 2**：draft-map（永久卡層的論證地圖，餵 centaur-writer 攤卡）、negative-list-builder（diff AI 初稿與作者定稿，累積個人「刪改禁止清單」）、kb-gardener（每週清掃）、moc-assistant（人開口才供候選）。
- 收編候選：title-brainstorm。
- 多語系：目前 prompt 為繁體中文（設計對象是中文創作者）；英文版屬歡迎的 contribution。

## 11. 貢獻與授權

- PR 紅線：任何改動不得違反 §6.1 的五條紅線與 centaur-writer 的六條紅線——它們是這個 plugin 存在的理由，不是可調參數。
- 新 skill 提案請附 eval 案例（有／無 skill 對照）。
- 授權：【修修：拍板 LICENSE，建議 MIT】

---

*本文件整理自 2026-07-13 的設計 session；研究出處見 README 內文連結。*
