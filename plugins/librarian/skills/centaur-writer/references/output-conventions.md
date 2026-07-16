# 輸出慣例：設定檔、檔名、frontmatter

原則：**frontmatter 全部是 AI 記帳區，作者永遠不用碰**；作者只寫正文。格式一律 Markdown（.md）。

## 設定檔（首次執行建立，之後只讀不問）

位置與名稱：使用者筆記資料夾根目錄的 `centaur-writer.config.md`。
查找順序：①這個檔 → ②若在 plugin 環境，找 plugin 共用設定（如 `librarian.config.md`）→ ③都沒有，用選擇題問一次並寫入①。

```yaml
---
type: centaur-writer-config
drafts_folder: Drafts/articles        # voice-draft 存放處
reports_folder: Drafts/articles/_reports  # 查證與除味報告存放處
format: md
---
（本檔由 centaur-writer 首次執行時建立；改設定直接編輯 frontmatter。）
```

首次設定只問三題：草稿存哪個資料夾（給 2–3 個常見選項＋自填）、報告存哪（預設草稿夾下 `_reports`）、沒掛筆記資料夾時是否改為「全部貼在對話裡」。

## 草稿檔

檔名：`<題目>-voice-draft-v2.md`（作者改寫後另存或直接改本檔皆可；AI 之後不動它）。

```yaml
---
type: article-draft
stage: voice-draft-v2      # voice-draft-v2 → author-final → verified（AI 依流程翻轉）
title: 減肥不能靠意志力
audience: 想減肥但一直失敗的上班族
publish_target: blog
created: 2026-07-13        # AI 預填
modified: 2026-07-13       # AI 維護
source_refs: []            # Phase 1 攤卡引用的卡（wikilink 清單）
open_flags: 3              # 正文內未解的【缺口】【待查證】數（AI 維護）
verify_report: ""          # Phase 6 報告檔 wikilink（AI 回填）
---
（正文＝Phase 4 的 voice-draft-v2；便利貼標記在正文內。作者改寫時只動正文。）
```

## 報告檔（Phase 6 產出，永遠是新檔）

檔名：`<題目>-verify-report-<YYYY-MM-DD>.md`，存 `reports_folder`。

```yaml
---
type: verify-report
article: "[[減肥不能靠意志力-voice-draft-v2]]"
created: 2026-07-13
verdict: pass-with-flags   # pass / pass-with-flags / needs-fixes
open_items: 2              # 待作者拍板的判斷點數
---
```

正文結構固定：①判斷點清單（最上面）→②逐條查證筆記→③除味檢查結果→④來源清單＋「查證後不建議引用」黑名單。

## 記帳紀律

- AI 只在「建檔、Phase 6 回填、stage 翻轉」三個時機碰 frontmatter，改的 key 僅限上列欄位。
- 報告與建議永遠存新檔，不改作者的草稿檔正文。
- 沒掛筆記資料夾時：所有輸出完整貼在對話裡，frontmatter 一併附上，請作者自己存。
