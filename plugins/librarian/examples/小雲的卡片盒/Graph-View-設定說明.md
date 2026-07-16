---
type: doc
demo: true
---
# Graph View 設定說明（.obsidian/graph.json）

> 套用方式：**先完全關閉 Obsidian**（不只關分頁），確認 `.obsidian/graph.json` 是本設定，再重開 vault 打開 Graph View——Obsidian 關閉時會把當前 UI 狀態寫回檔案，開著改會被蓋掉。

## 顏色語意（照知識庫的層級，不是隨機配色）

| 顏色 | 對象 | 為什麼 |
|---|---|---|
| 🟡 金 #F2C14E | KB/Permanent 永久卡 | 主角。人親手寫的權威層，全圖最亮 |
| 🔴 橘紅 #E4572E | KB/MOCs 入口卡 | 進場的門，一眼找得到 |
| 🔵 藍 #4C9AFF | KB/Literature 文獻筆記 | 素材層：卡的出生地 |
| 🟣 紫 #9B7EDE | KB/Wiki AI 候選頁 | AI 的 candidate 層，刻意用冷色與人區隔 |
| ⚪ 灰 #8A8F98 | KB/Fleeting 靈感筆記 | 還沒處理的暫存，灰色漂浮的點＝inbox 待辦 |

視覺敘事剛好對應課程：**金色的網**（你的理解）長在**藍色素材**上，**紫色**是 AI 備料、永遠在外圈，**灰點**等著每日回顧撈。

## 過濾（拿掉的都是雜訊）

- `KB/index.md`、`KB/log.md`——索引與日誌連向所有頁，不濾掉整張圖會塌成毛球（最重要的一條）。
- `Templates/`、`Drafts/`、`KB/Raw/`、`KB/Reviews/`、`KB/Annotations/`、`_processed/`、README／CLAUDE／config——工作檔與原始庫，不屬於知識網。
- `hideUnresolved: true`——沒建立的連結不畫虛點。

## 其他調校

箭頭開啟（`showArrow`）——連結方向「本卡→對方」有語意，值得畫出來；節點與文字調大（`nodeSizeMultiplier 1.45`、`textFadeMultiplier -0.6`）——卡少的新 vault 要讓卡名早點浮出來；斥力與連結距離放寬（`repelStrength 12`、`linkDistance 180`）——小網不要擠成一團。卡片超過百張後，可把 nodeSize 調回 1.0～1.2。

## 給 Starter Vault 建置的備忘

`graph.json` 屬於 `.obsidian` 快照層——建置 Starter Vault 時把這份設定一起放進快照（G3 規格 §4 步驟 6 實測後、步驟 7 打包前），學員開圖第一眼就是分好層的樣子，不用自己調。
