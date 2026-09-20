# 寶石嵌合系統 — 審查記錄

> 檔案：`design/gdd/gem-socketing.md`

---

## Review — 2026-09-20（第一次）— Verdict: NEEDS REVISION → 已修訂

Scope signal: XL
Specialists: game-designer、systems-designer、qa-lead、economy-designer、creative-director（full 模式）
Blocking items: 6 | Recommended: —
Prior verdict resolved: First review

**摘要**：首次審查。六項阻擋性問題全數當場修訂。

| # | 阻擋性問題 | 處置 |
|---|---|---|
| 1 | 介面契約未定義，下游無法實作 | 補齊跨系統介面契約 |
| 2 | `FinalDamage` 數值範圍算錯 | 重算，範圍修正為 0.1875 – 1.8 |
| 3 | `K > 0` 未強制，存在除以零風險 | 補上約束，新增 `ArmorReductionScale = 200` 與 `ArmorReduction` 換算式 |
| 4 | 缺少「換裝寶石」的驗收標準 | 補 AC |
| 5 | 融合經濟失衡（理性玩家不會融合、約 6% 玩家湊不齊材料） | `FusionCost` 3 → 2 |
| 6 | 關卡驗證規則範疇不明 | 精確化為「兩種可行屬性解，其中一種須為該進度點理論上可持有」 |

> ⚠️ **本次審查的收尾不完整**：修訂已寫入 GDD，但未建立本審查記錄、
> 未更新 `systems-index.md` 狀態、且**未同步 `entities.yaml` 的 `FusionCost`**。
> 三項缺漏於第二次審查後補正（見下）。

---

## Review — 2026-09-20（第二次）— Verdict: NEEDS REVISION → 已修訂

Scope signal: XL（6 公式、12 項上游相依全部未撰寫、跨 3 個未寫系統的介面契約、1 項需向上傳播的修訂）
Specialists: 無——本機未安裝 specialist agent 檔案，以 lean 等效模式單一審查者分析，
涵蓋 game-design / systems / QA / economy 四個視角
Blocking items: 1 | Recommended: 4
Prior verdict resolved: **無法判定**——文件標頭自稱已完成 full 審查並修訂 6 項阻擋性問題，
但磁碟上不存在本審查記錄、`systems-index.md` §7 仍顯示「待審查」，
故第二次審查將其視為首次審查處理。此歧義已由本記錄消除。

**Completeness**: 8 / 8 節齊備
**Dependency graph**: 12 項相依全部尚無 GDD，但**全部已於文件內正確自我標示為 provisional**，無隱藏的斷裂引用

### 阻擋性項目

**1. `game-concept.md` §4.3 的核心主張被本 GDD 證偽**（cross-doc）

§4.3 宣稱「限制型目標永遠可達成…此規則從根本消除了『隨機掉落 × 固定目標』的衝突，
不需額外的保底系統」。但本 GDD 的 Edge Cases 不得不發明一道保底機制——
「至少兩種可行屬性解」的關卡驗證規則——因為**限制型挑戰 × 敵人抗性佈局**
仍可能組合出實質不可達成的星星。

§4.3 的保證因此是不完整的。三星挑戰與關卡章節進程的 GDD 尚未撰寫，
若它們照字面信任 §4.3，將會漏掉自己該做的驗證。

→ **已處理**：於 §4.3 加入限定區塊，明確劃出鐵則的效力範圍僅止於挑戰目標本身，
並指明抗性佈局的可解性由關卡驗證規則負責、下游 GDD 必須實作。
反支柱 #4 的交叉引用同步更新。

### 建議修訂（4 項，全數已處理）

| # | 領域 | 問題 | 處置 |
|---|---|---|---|
| 1 | systems | 火的 3 層驅逐時機隱晦——未言明是建立時快照或持續重估 | 明確定義為**持續重估**：新層傷害高於當前最低者時，最低者立即失效；名額釋出時可重新遞補 |
| 2 | qa | 缺少火「持續時間中途被擠下」的 AC（現有 AC 只測靜態快照） | 新增中途驅逐與重新遞補的 AC |
| 3 | qa | 風的「同時命中」在實作粒度上未定義 | 定義為**同一次傷害結算批次**；批次內入列統一裁決，跨批次各自獨立。併新增跨批次 AC |
| 4 | economy | 融合的「數量 vs 品質」驗算對錯了比較基準——用「選哪顆寶石」的表去論證「要不要融合」 | 新增〈融合的取捨驗算〉小節，以真正的對照組重算：不融合 `1.5+1.5=3.0` vs 融合 `1.8+1.0=2.8`，並說明這 −6.7% 是刻意設計，融合真正勝出於站位稀缺時（呼應支柱②） |

### 審查未涵蓋、但同時發現的問題

**`entities.yaml` 的 `FusionCost` 仍為 3，與 GDD 的 2 不一致。**

第一次審查修訂 GDD 時，往登錄表補了 `ArmorReduction` 與 `ArmorReductionScale`
兩筆新項目，卻沒回頭改 `FusionCost`。

`/design-review` 的流程**不包含登錄表比對**——它比對 GDD 與 GDD，不比對 GDD 與 registry。
只有 `/design-system`（撰寫前讀取）與 `/consistency-check` 會讀 registry。
這是一個流程缺口，不是審查者的疏失。

→ **已處理**：`entities.yaml` 的 `FusionCost` 更正為 2，note 補上調降理由。

### 資深判斷

公式內部一致，六條全部重新推導無退化輸出（`FinalDamage` 0.1875 – 1.8 範圍正確、
`K > 0` 已強制、25% 護甲下限與 75% 減免上限同時防住除以零與實質免疫）。
介面契約具體到今天就能寫 stub 測試。驗收標準幾乎全是可測的 GIVEN/WHEN/THEN，
沒有「感覺平衡」這類無法驗證的敘述。

唯一實質問題是結構性的、不在本檔案內：它暴露了概念文件對挑戰可達成性的根本主張有缺口。
這正是 design-review 該在傳播下去之前抓到的東西——三星挑戰與關卡進程的 GDD 都還沒寫，
現在往上游修是最便宜的時機。

> **對 Scope Signal 的保留意見**：XL 主要反映「12 項上游相依全部尚未撰寫」，
> 這是專案階段早的結果，不是本系統本身的體量。
> 前 6 份 GDD 完成後重評預期會落回 L。排程時不應直接採用此值。

---

## 流程缺口記錄

1. **`/design-review` 不比對 entity registry。** 審查通過不代表登錄表同步。
   每次審查修訂數值後，須手動確認 `design/registry/entities.yaml`，
   或補跑 `/consistency-check`。
2. **審查後的收尾有三步，缺一不可**：寫審查記錄、更新 `systems-index.md` 狀態、
   同步 registry。第一次審查漏了全部三步。
