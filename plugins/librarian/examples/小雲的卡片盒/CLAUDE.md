# 小雲的卡片盒——權限規則（librarian-setup 生成）

| 路徑 | 權限 |
|---|---|
| KB/Fleeting/ | 🧑＋AI capture 寫入；AI 只翻 status 與搬移，不改字 |
| KB/Literature/ | 🤖 render（劃線＋人的 note::，AI 不改寫 note） |
| KB/Annotations/ | 🤖 原始庫，不可改寫 |
| KB/Permanent/ | 🔒 正文與 status human-only；AI 只維護記帳欄 |
| KB/MOCs/ | 🧑 人建；AI 只供候選清單 |
| KB/Raw/、KB/Wiki/ | 🤖 AI 的下層 |
| KB/index.md | 🤖 generated，人不手編 |
| KB/log.md | 🤖 append-only，不改歷史 |

## 紅線五條（所有 skill 出手前讀這裡）

1. AI 絕不寫 `KB/Permanent/` 的正文與 status；只維護 created／modified／source_refs／aliases。
2. AI 寫下的每個事實宣稱附來源錨點，溯回 Raw 或 Annotations。
3. Wiki 是 candidate 層，不冒充永久卡：AI 產頁 frontmatter 必帶 `author: ai`。
4. AI 不自建 MOC；只在人開口時供候選。
5. 終端證據只能是 Sources／Raw／Annotations，不得拿另一個 Wiki 頁當事實來源。
