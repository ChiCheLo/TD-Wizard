# Active Session State

> 最後更新：2026-10-15

| 欄位 | 內容 |
|---|---|
| **Task** | `/prototype --spike` — 連攜系統施法手感 |
| **Phase** | Build 完成，**等待使用者在 Unity 執行並回報** |
| **Spike 問題** | 巫師能否在 1 秒內完成「進入施法模式 → 讀落點圓 → 確認」，且受擊即取消不會讓連攜在怪群壓力下變成放不出來的技能？ |
| **Path** | Engine（Unity 6000.3.16f1 / URP），第三人稱 only |
| **程式碼位置** | `Assets/_Prototype/ChainCastSpike/`（可刪除，見刪除清單） |
| **紀錄位置** | `prototypes/連攜-castfeel-spike-2026-10-15/` |
| **關鍵數字** | 停留時間 **p90 是否 ≤ 1.0 秒**（game-concept §3.2 硬性約束） |

## 執行方式

1. Unity 建空場景，存成 `Assets/_Prototype/SpikeChainCast.unity`
2. 刪掉預設的 Main Camera 與 Directional Light（bootstrap 會自己生）
3. 建空 GameObject → Add Component → `SpikeSceneBootstrap`
4. 按 Play

操作：WASD 移動／滑鼠視角／**Q** 施法模式／**左鍵** 確認／**K** 殺參與塔／**F1** 輸出結果／**Esc** 放開滑鼠

## Scope（已向使用者確認）

1. 會動的巫師 ＋ 固定兩座塔（6.0 進／6.4 出遲滯）
2. 完整施法狀態機（Aiming／Windup 0.3s／ShortStun 1.0s／四種取消）
3. 壓力源敵人 ＋ 停留時間量測

**不做**：寶石屬性、傷害計算、凍土／星星之火、真 30 秒 CD、階段推進、搶佔、第一人稱、美術。

## 刻意偏離 GDD

| 項目 | GDD | spike | 理由 |
|---|---|---|---|
| `ChainSkillCooldown` | 30 秒 | 2 秒 | 30 秒每次才一個樣本，蒐不到分佈。不在驗證 CD 手感 |

## 上一個任務（已完成）

`/design-review design/gdd/連攜系統.md` 第 3 輪 — Verdict: NEEDS REVISION，11 項已修訂。
連帶把 gem-socketing 的 Core Rule 3 由四項改為五項（新增射程），該文件狀態已改為
**Approved — 待複審**。審查記錄：`design/gdd/reviews/連攜系統-review-log.md`。

## 這個 spike 之後

1. 依結果更新 `design/gdd/連攜系統.md`（Open Questions #3／#14／#15／#16 的 provisional 值）
2. `/design-review design/gdd/gem-socketing.md` — Core Rule 3 變更待複審
3. 第一人稱載體的第二個 spike（本次刻意排除）
