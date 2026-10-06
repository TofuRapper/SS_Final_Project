# 多人連線合作解謎遊戲 🦖

**▶ 立即遊玩：https://ssfp-2.web.app**（多人連線，可開兩個分頁一起玩）

以 Cocos Creator 2.4 與 TypeScript 開發的多人即時連線 2D 合作解謎平台遊戲，後端採用 Firebase Realtime Database，並部署於 Firebase Hosting 供瀏覽器直接遊玩。本專案為團隊期末專案，以 Git 分支協作完成。

> 本 repo 為原專案 [elfnt/SS_Final_Project](https://github.com/elfnt/SS_Final_Project) 的完整副本，保留所有組員的 commit 紀錄。

## 遊戲概念

玩家選擇不同顏色的恐龍角色，進入大廳後一同出發，合作護送一顆會因摔落而受損的恐龍蛋通過關卡；途中需協力推箱、觸發機關讓橋樑移動或旋轉、躲避雷射。遊戲同時暗藏一名隨機指定的「內鬼」暗中破壞，其他玩家需在投票階段找出並放逐內鬼，依結果進入不同結局。

## 系統架構

```
本機客戶端 ──寫入──▶ Firebase Realtime DB ──推送──▶ 其他玩家客戶端
Player.ts                players/{playerId}           Multiplayer.ts
 每 0.1 秒上傳位置        位置、面向、動畫、角色        監聽加入／移動／離線
Vote.ts 寫入選票         games/{gameId}               Other-Player.ts
LobbyManager 開始遊戲     state、imposter、voting      lerp 平滑插值移動
```

## 我負責的部分

| 模組 | 內容 |
| --- | --- |
| 玩家控制與同步（`Player.ts`） | 移動、跳躍、動畫；以 0.1 秒間隔上傳位置與狀態，平衡即時性與資料庫寫入量 |
| Firebase 整合（`FirebaseManager.ts`） | 資料庫初始化、登入驗證、資料結構設計、隨機指定內鬼 |
| 多人連線（`Multiplayer.ts`、`Other-Player.ts`） | 監聽玩家加入／移動／離線並動態生成遠端角色；以線性插值避免網路延遲造成瞬移 |
| 大廳與投票（`LobbyManager.ts`、`Vote.ts`） | 房主開始遊戲、監聽狀態切換場景；即時統計選票、判定放逐與勝負 |
| 場景流程 | 登入、選單、角色選擇、大廳、遊戲、投票、兩種結局共 9 個場景的串接 |

箱子、橋樑、彈簧、雷射、感應器、恐龍蛋摔落傷害、護盾條與音效等關卡元件由其他組員實作。

## 技術

Cocos Creator 2.4.8 · TypeScript · Firebase Realtime Database · Firebase Authentication · Firebase Hosting

## 執行

以 Cocos Creator 2.4.8 開啟專案資料夾，開啟 `assets/scene` 中的登入場景並預覽；或建置為 Web Mobile 後以 `firebase deploy` 部署。
