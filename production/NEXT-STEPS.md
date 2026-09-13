# TD Wizard — 進度與下一步

> 最後更新：2026-09-13
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

---

## ➡️ 下一步

```
/art-bible
```

**為什麼是這個**：視覺識別規格會**閘控所有美術製作**，而且會影響技術架構決策
（渲染、VFX、UI 系統）。它必須在寫系統 GDD 之前完成。

`game-concept.md` §9 已經有「視覺識別錨點」（明亮奧術 Luminous Arcane），
`/art-bible` 會從那裡展開成完整規格。

**特別注意**：本作第一號技術風險是**連攜中轉的可讀性**。視覺規則
「屬性先於美感」就是為了防這個風險而訂的，art-bible 階段務必守住。

---

## 之後的完整路線

```
✅ /setup-engine          Unity 6.3 已釘、參考文件已建
✅ /brainstorm            game-concept.md 已產出
⬜ /art-bible             ← 下一步。視覺識別規格
⬜ /design-review design/gdd/game-concept.md    驗證概念完整性
⬜ /map-systems           拆解系統與相依關係
⬜ /design-system (×N)    逐系統寫 GDD
⬜ /review-all-gdds       跨系統一致性檢查
⬜ /gate-check            進入架構階段前的關卡驗證
───────────────────────── 以上為 Concept 階段 ─────────────────────────
⬜ /create-architecture → /architecture-decision (×N) → /create-control-manifest → /architecture-review
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
