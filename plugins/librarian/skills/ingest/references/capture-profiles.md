# Capture Profiles — 偵測與解析規則（Step 0／Step 1 詳細版）

每個 profile 的目標相同：把工具落檔解析成統一三元組 **`(quote, note, locator)`**——quote＝劃線／節錄原文（exact copy，去掉標記符號但不改字）；note＝使用者自己的想法；locator＝定位資訊（原樣保留）。

## 1. `article-web`（Shape A）

- 來源：Obsidian Web Clipper 整篇 clip 進 `Inbox/`，使用者在 Obsidian 內劃線。
- quote：所有 inline `==...==` 的文字。
- note：緊接該劃線段落**下一行**的 `note:: ...`（容忍 `> [!annotation]` callout 形式）。
- locator：劃線所在的段落序位（第 N 段）。
- 全文：整篇正文 → `KB/Raw/{slug}.md`。

## 2. `youtube-transcript`（Shape A，可降級）

- 來源：Media Extended v4——使用者從逐字稿面板複製的行自帶 `[MM:SS](url#t=NNN) 台詞`。
- quote：「台詞」部分。
- note：其後的 `note:: ...`。
- locator：`[MM:SS](url#t=NNN)` 時間戳連結**原樣保留**（Literature 引文行裡要可點）。
- 全文：vault 裡有下載的完整字幕檔 → 連進 `KB/Raw/`＝Shape A；沒有 → 當 Shape B 處理並告知。

## 3. `podcast-snipd`（Shape B）

- 來源：Snipd 官方 plugin，一集一檔。
- quote：每個 snip 的逐字稿節錄。
- note：snip 的個人註欄（使用者若設過 template 會是 `note::`；沒有就抓 snip 原生 note 欄當 fallback）。
- snip 的 **AI 摘要**放 quote 後面、標「（AI 摘要）」——不與使用者原話混在一起。
- locator：snip 的音檔跳轉連結，原樣保留。

## 4. `ebook-kobo`（Shape B）

- 來源：Kobo Highlights Importer（USB 讀 sqlite），依章節分組。
- quote：每條 blockquote 劃線。
- note：該劃線的附註（template 版是 `note::`；否則抓 highlight.note 欄）。
- locator：**章節名＋章內序位**；Literature 保留章節分組（`## 章節名`）。

## 5. `highlights-list`（Shape B，可升級 A）

- 來源：Clipper 的 highlights-only 模式、Matter／Readwise 匯出等——只有劃線清單。
- quote：清單項；note：項下的 `note::` 或縮排註記；locator：出現序位。
- frontmatter 有 URL → 問一次要不要抓全文升級 A（僅此 profile；書與 podcast 不提供）。

## 共同紀律

- quote 一律 exact copy——不改寫、不 paraphrase、不補寫使用者沒寫的想法。
- Shape A 且零劃線 → 可續跑（Literature 會空，只產摘要與候選，先告知）；Shape B 且零項目 → 沒素材，停。
- 判不出 profile → 列檔案前 20 行問使用者，不硬套。

## Literature Note 統一輸出格式

```markdown
---
type: literature
source: "{原檔相對路徑或 URL}"
source_kind: article | video | podcast | book
capture_profile: {profile}
shape: A | B
derived_from: fulltext | annotation-set
slug: {slug}
captured: {date}
status: unprocessed        # → ingested（Step 3 翻）→ mined（daily-review 翻）
mined_concepts: []
---

## {章節名 或 「不分章」}

> {quote 1} — [12:34](url#t=754)（影音）／📖 第3章·§2（書）
^p-1

note:: {使用者想法 1}
```

## Source 摘要頁 frontmatter

```yaml
---
type: source
status: draft
author: ai
source_kind: {同上}
capture_profile: {同上}
shape: A | B
derived_from: fulltext | annotation-set
confidence: medium   # A=medium；B=low
source_refs: ["{Raw 路徑或 URL}", "[[Literature/{slug}]]"]
created: {date}
---
```

B 路徑頁首固定一行：「⚠️ 本頁自使用者劃線集派生，未經全文佐證；主張的完整脈絡請回原始來源。」
