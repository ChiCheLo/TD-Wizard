# 連攜系統 — 審查記錄

> 本檔記錄 `/design-review design/gdd/連攜系統.md` 的歷次審查結果。
> 第 1、2 輪為文件內部的非正式修訂（未經 skill 執行、無獨立記錄），其發現與處理內容
> 保留在 GDD 各節的「第2輪修訂」標記中；本檔自第 3 輪（首次正式執行）起記錄。

---

## Review — 2026-09-24 — Verdict: NEEDS REVISION（已於同 session 完成修訂）

**Scope signal**: XL
**Specialists**: `systems-designer`／`qa-lead`／`art-director`／`creative-director`（senior 綜整）
**未能參與**: `game-designer`（本機 agent 清單不含此型別，與第 2 輪相同的缺口——Player Fantasy 仍未經專屬 game-design 視角覆核）
**Blocking items**: 11 ｜ **Recommended**: 7 ｜ **Nice-to-have**: 4
**Prior verdict resolved**: 首次正式審查（前兩輪為非正式內部修訂）

### 關鍵發現

`creative-director` 判定**全場唯一致命問題**是星星之火在機制上被完全支配：三技能共用 `ChainDamageMult = 0.5`，但該值是「每次判定」係數——領域類 4 次 tick 交付 2.0× `WizardBaseDamage`，星星之火單次引爆僅 0.5×。同 CD、同配方成本，卻只有 25% 傷害，外加精準瞄準要求（本作唯一的反支柱③豁免）、可完全打空且打空照樣進 CD、還需風塔配合。**為這一招付出的支柱代價已經付了，回饋卻沒拿到。**

`systems-designer` 與 `qa-lead` **各自獨立發現**同一項矛盾：4.5 秒時效的邊界語意，Edge Case 的推導（終點判定吃不到剋制，故 4.0 是失敗值）與 AC44 的斷言（恰好 4.5 秒含邊界仍有效）直接相反。

本輪另驗證並修正了 `entities.yaml` 的實際漂移——它仍在發布已被刪除的 `ChainDetectionRadius = max(AttackRadius_i)` 公式與已知失敗值 `4.0`，而 systems-index 聲明該登錄表會在撰寫後續 GDD 時自動比對。下一位作者（塔系統，設計順序 #2）會被告知錯的值才是權威。

### 專家意見分歧

**四種結束態分類法**：`qa-lead` 論證規格自洽、混淆風險低於表面，判定可接受；`art-director` 判定整套是工程完整性產物，戰鬥中只有兩軸是真的。`creative-director` 裁決採 `art-director` 方向但不採其兩軸提案——收斂為三套視覺語言（無代價取消／踉蹌／換檔），踉蹌必須保持獨立（唯一有代價者）。**使用者決定事件層保留五種 reason tag，只收斂美術工單。**

**`AttackRadius` 歸屬**：`systems-designer` 判 blocking 並建議塔層級常數（避免修改已核准文件）；`creative-director` 同意塔層級常數但降級為「一句話裁決」。**使用者採反向決定**——射程逐寶石決定，並核准回頭修改 gem-socketing Core Rule 3 為五項。理由：若半徑恆定，落點圓（第一號風險系統）的整段設計意圖會被抽空。

### 本輪修訂（11 項）

| # | 項目 | 性質 |
|---|---|---|
| 1 | 傷害係數拆分為 `ChainTickDamageMult`(0.5)／`ChainBurstDamageMult`(2.5) | 阻擋性・致命 |
| 2 | `Aiming` 新增配方持續重驗（消除「久駐免費」支配策略） | 阻擋性 |
| 3 | 新增 `ChainDetectionRadiusHysteresis`(0.4)，消除技能列邊界閃爍 | 阻擋性 |
| 4 | 第一人稱通道歸屬表定案，準心認領為主載體（`ShortStun` 原本無載體） | 阻擋性 |
| 5 | Peak 杖→塔光束改為世界座標快照（三層模型重構的視覺副作用） | 阻擋性 |
| 6 | `entities.yaml` 同步（2 項過期值 ＋ 4 項缺漏登錄） | 阻擋性 |
| 7 | 結束態語言收斂為三套；`MaterialInvalidated` 攜帶失效塔 ID | 刪減 |
| 8 | 4.5 秒邊界裁決為左閉右開 | 矛盾 |
| 9 | AC35 的 CD 起點衝突 | 矛盾 |
| 10 | AC24／25／51 的「不受影響」改為可驗證斷言 | 矛盾 |
| 11 | `AttackRadius` 裁決為逐寶石；gem-socketing Core Rule 3 改為五項 | 拍板 |

### 明確駁回（不修）

senior 判定為文件問題而非遊戲問題，記錄於此以免下一輪重複發現：死掉的巫師能施法、`ShortStun` 期間不能死、tick 累積策略、空間查詢排序保證、物件池 generation ID、⚠️／🚫 依賴標記不完整、第 2 輪修法缺 AC、容許誤差慣例自我違反、跨實例 Peak 仲裁器、Windup 蓄能亮度下限。理由：單人開發下這些在實作十分鐘內會自己撞到並修掉。

### Senior 的結論

> 這份文件**在發散，不在收斂**。第 2 輪修掉「`Active` 鎖住所有技能鍵」→ 生出第三層效果實例 → 生出光束錨點缺口、第四種結束態、一批新 AC。每一輪的修法都在製造下一輪的發現。所有真正承重的未知都標著「待 `/prototype --spike`」——再寫一萬字也不會回答其中任何一個。

**下一步為 `/prototype --spike`，不是第 4 輪 review。** 只做炎寒領域一招、第三與第一人稱各試、一週內。本 GDD 下一次修改的依據應為原型手感。

### 連帶動作

- `gem-socketing.md` 狀態由 **Approved** 改為 **Approved — 待複審**（Core Rule 3 已變更）
- `systems-index.md` 進度表與敘述同步更新
- 新增 Open Questions #14／#15／#16（三項 provisional 值待原型校正）
