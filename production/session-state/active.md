# Active Session State

> 最後更新：2026-09-20

| 欄位 | 內容 |
|---|---|
| **Task** | 寶石嵌合系統 GDD — 審查後修訂與收尾 |
| **Status** | **Complete**（兩輪 `/design-review` 皆已處理完畢） |
| **File** | `design/gdd/gem-socketing.md` |
| **Next** | `/design-system 連攜系統`（設計順序第 6 位，**須於全新 session 執行**） |

## 本次處理

第二輪 `/design-review` 的結果（1 項阻擋 + 4 項建議），加上審查未涵蓋但同時發現的
登錄表漂移，全部處理完畢。

| # | 項目 | 檔案 |
|---|---|---|
| 1 | `FusionCost` 3 → 2（登錄表漂移修正） | `design/registry/entities.yaml` |
| 2 | §4.3 鐵則加入限定：不涵蓋敵人抗性佈局 | `design/gdd/game-concept.md` |
| 3 | 火的層數驅逐明確定義為**持續重估**（非建立時快照） | `gem-socketing.md` |
| 4 | 風的「同時」定義為**同一次傷害結算批次** | `gem-socketing.md` |
| 5 | 新增〈融合的取捨驗算〉——改用正確的對照組 | `gem-socketing.md` |
| 6 | 新增 3 條 AC（火中途驅逐與遞補、風跨批次） | `gem-socketing.md` |
| 7 | 建立審查記錄（補記第一、二輪） | `design/gdd/reviews/gem-socketing-review-log.md` |
| 8 | 進度改為 Reviewed；§8 約束補上 §4.3 的限定 | `design/gdd/systems-index.md` |

## ⚠️ 已知流程缺口（下次審查務必注意）

**`/design-review` 不比對 `design/registry/entities.yaml`。**
它比對 GDD 與 GDD，不比對 GDD 與 registry。第一輪審查把 `FusionCost` 從 3 改成 2，
但沒回頭同步登錄表，兩輪之間漂移了整整一份文件的時間。

**審查後的收尾有三步，缺一不可**：
1. 寫 `design/gdd/reviews/[system]-review-log.md`
2. 更新 `systems-index.md` §7 狀態與總進度
3. **同步 `entities.yaml`**（或補跑 `/consistency-check`）

第一輪三步全漏。

## 進度

- 系統 GDD：**1 / 32 已設計且已審查**（寶石嵌合）
- 設計順序中已完成：第 5 位

## 傳給下游的硬約束（累計）

1. **敵人須新增「防禦力」概念**（`Armor`、`WeakAttribute`、`ResistAttribute`）
2. **關卡的敵人抗性 × 挑戰禁用項，須至少留下兩種可行屬性解**
   ——`game-concept.md` §4.3 的鐵則**不足以**保證可達成性，三星挑戰與關卡進程的 GDD
   必須自行實作此驗證
3. 塔僅單槽；基礎塔未嵌合仍可攻擊
4. 嵌合需付費 `C`（建造費 60%）

## 下一步

**`/design-system 連攜系統`**——設計順序第 6 位，直接依賴寶石嵌合剛定案的五屬性行為，
也是專案的第一號風險（連攜技能 UI 可讀性）所在。

> 若想先驗證手感而非繼續寫文件，可在連攜系統 GDD 完成後直接跳 `/prototype --spike`，
> 不必等前 6 份 GDD 全滿。

## 未 commit

```
M design/gdd/game-concept.md
M design/gdd/gem-socketing.md
M design/gdd/systems-index.md
M design/registry/entities.yaml
?? design/gdd/reviews/gem-socketing-review-log.md
M production/session-state/active.md
```
