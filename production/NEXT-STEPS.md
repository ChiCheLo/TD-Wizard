# TD Wizard — 進度與下一步

> 最後更新：2026-09-15
> 階段：**Concept**（見 `stage.txt`）｜審查模式：**lean**（見 `review-mode.txt`）

---

## 📍 目前位置

**概念設計已完成。** 完整內容在 → **[design/gdd/game-concept.md](../design/gdd/game-concept.md)**

> ⚠️ 所有設計內容以 `game-concept.md` 為**單一真實來源**。
> 本檔只記錄進度與下一步，**不重複設計內容**，避免兩處分歧。

**一句話**：3D 動作塔防。玩家是兼任工程師的巫師，建造防禦塔、嵌合屬性寶石，
並用自己的站位作為連攜的中繼點。核心動詞是**接通**。

---

## ✅ 已完成

| 項目 | 產出 |
|---|---|
| 引擎設定 | `docs/engine-reference/unity/` — Unity 6000.3.16f1 已釘，版本落差文件已建 |
| 技術規範 | `.claude/docs/technical-preferences.md` |
| 專案守則 | `CLAUDE.md` |
| **概念設計** | **`design/gdd/game-concept.md`** — 支柱、核心循環、MVP、範圍分層、風險 |
| **美術聖經** | **`design/art/art-bible.md`** — 9 節完整：視覺規則、色彩系統、資產標準 |
| **系統索引** | **`design/gdd/systems-index.md`** — 32 系統、相依分層、設計順序、進度追蹤 |

---

## ➡️ 下一步

```
/create-architecture
```

**為什麼是這個**：系統清單與相依關係已經定了，接下來要產出**技術架構藍圖**
與必要的 ADR 清單，決定這些系統在程式碼裡怎麼組織。

架構定案前不要開始寫系統 GDD 或程式碼——`CLAUDE.md` 的「架構核心」目前
仍是「尚未定義」，那條規則要求架構先行。

**架構階段要處理的關鍵決策**（已由 `/map-systems` 識別）：
- 事件匯流排的實作方式（所有系統的溝通骨幹）
- `ISaveable` 序列化契約
- MaterialPropertyBlock 破損材質（art-bible §8 硬規則）
- URP Render Graph 遮擋描邊

之後才是 `/design-system` 逐個撰寫 GDD（順序見 systems-index §6）。

---

## 🎨 art-bible 期間新增的決策

| 決策 | 內容 |
|---|---|
| 攝影機 | **肩後跟隨**，非俯視上帝視角。不提供戰術俯視模式 |
| 小地圖 | **必備**，第一人稱與肩後皆同 |
| 塔的狀態 | **破損程度**表現，不用血條。裂縫透出屬性色光 |
| 法杖 | 純法杖，建造／修理／施法全用它 |
| 世界觀 | **要塞與城鎮**（非學院） |
| 材質 | Stylized PBR（非手繪）——為了與採購素材相容 |
| 角色客製化 | 列入未來規劃，但**模型須做成可分離部件** |
| 屬性編碼 | 色相 + 符文；**形狀改為編碼等級** |

---

## 之後的完整路線

```
✅ /setup-engine          Unity 6.3 已釘、參考文件已建
✅ /brainstorm            game-concept.md 已產出
✅ /art-bible             art-bible.md 已產出（9 節完整）
✅ /map-systems           systems-index.md 已產出（32 系統）
⬜ /create-architecture   ← 下一步。架構藍圖與 Required ADR 清單
⬜ /architecture-decision (×N)    逐項記錄架構決策
⬜ /create-control-manifest       彙整成可執行的規則表
⬜ /architecture-review           架構覆蓋率驗證
───────────────────────── 以下回到設計 ─────────────────────────
⬜ /design-system (×N)    逐系統寫 GDD（順序見 systems-index §6）
⬜ /review-all-gdds       跨系統一致性檢查
⬜ /gate-check            階段關卡驗證
⬜ /ux-design → /vertical-slice → /create-epics → /create-stories → /sprint-plan
⬜ /dev-story (×N)        進入實作
```

> 忘記自己在哪時，打 `/help` 或 `/project-stage-detect`。

---

## ⚠️ 帶進下游的三個待解問題

詳見 `game-concept.md` §13。摘要：

1. **「弱輸出」的數值下限** → 交給巫師系統 GDD
2. **連攜中轉是否也需站定不動** → 需實測驗證（可考慮 `/prototype --spike`）
3. **手把支援是否提升為 Full** → 交給 `/ux-design`

art-bible 另外傳下的約束（詳見該檔尾「傳給下游的約束」）：

4. **關卡須小、好記、有地標** — 長廊型與視線封閉迷宮型在肩後視角下體驗極差
5. **寶石屬性上限約 8 種** — 受色相可分性限制
6. **空間音效升級為必需系統** — 須比原規劃更早處理
