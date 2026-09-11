# Technical Preferences — TD Wizard

Engine: Unity 6000.3.16f1 (Unity 6.3)
Render Pipeline: URP (Universal Render Pipeline)
Platform: PC / Steam (Windows primary)
Language: C#
Input: New Input System
Network: **Single-player only — no netcode, no online services** (decision 2026-09-11)

---

## Naming Conventions

| 類型 | 規範 | 範例 |
|---|---|---|
| Class / Struct | PascalCase | TowerData, WaveSpawner |
| Interface | I + PascalCase | IDamageable, ITargetable |
| Public 變數 | camelCase | baseDamage, waveIndex |
| [SerializeField] | camelCase | towerData, muzzleFlash |
| Private 變數 | _camelCase | _currentHealth, _cooldownTimer |
| 方法 | PascalCase | CalcDamage(), OnEnemyDied() |
| 常數 | UPPER_SNAKE_CASE | MAX_WAVE_COUNT |
| Enum 值 | PascalCase | TowerTier.Advanced |
| 事件 | On + PascalCase | OnWaveCleared |
| 檔名 | PascalCase，與類別同名 | TowerController.cs |

---

## Input & Platform

- **Target Platforms**: PC (Steam)
- **Input Methods**: Keyboard/Mouse, Gamepad
- **Primary Input**: Keyboard/Mouse
- **Gamepad Support**: Partial
- **Touch Support**: None
- **Platform Notes**: 滑鼠為主要操作（選取地塊放塔、UI 點擊升級）。手把為次要支援，UI 需額外設計 d-pad 導航，不可有僅靠 hover 觸發的互動。鍵盤做快捷鍵（暫停、加速）。

## Testing

- **Framework**: Unity Test Framework (NUnit)
- **Status**: 已記錄選型，尚未執行 `/test-setup` 建立實際測試專案結構

---

## Engine Specialists

- **Primary**: unity-specialist
- **Language/Code Specialist**: unity-specialist（C# review）
- **Shader Specialist**: unity-shader-specialist（Shader Graph、HLSL、URP 材質）
- **UI Specialist**: unity-ui-specialist（UI Toolkit UXML/USS、UGUI Canvas）
- **Additional Specialists**: unity-dots-specialist（ECS / Jobs / Burst）、unity-addressables-specialist（資產載入與記憶體管理）
- **Routing Notes**: 架構與一般 C# review 走 primary。任何 ECS/Jobs/Burst 程式走 DOTS specialist。渲染與視覺效果走 shader specialist。所有介面實作走 UI specialist。

### File Extension Routing

| File Extension / Type | Specialist to Spawn |
|-----------------------|---------------------|
| Game code (.cs) | unity-specialist |
| Shader / material (.shader, .shadergraph, .mat) | unity-shader-specialist |
| UI / screen (.uxml, .uss, Canvas prefabs) | unity-ui-specialist |
| Scene / prefab (.unity, .prefab) | unity-specialist |
| General architecture review | unity-specialist |

---

## Performance Budgets (PC / Steam)

| 指標 | 目標 |
|---|---|
| 幀率 | 60 FPS（1080p，中階顯卡）|
| Draw Call | < 500 |
| RAM | < 2 GB |
| 啟動時間 | < 10 秒 |
| 場景載入 | < 5 秒 |

**塔防專屬風險**：單位數量隨波次成長。大量同類敵人的渲染與 AI 更新是主要瓶頸，實作敵人生成系統前須評估物件池（Object Pooling）與 GPU Instancing。

---

## Forbidden Patterns

- No `Debug.Log`（unless explicitly requested）
- No `try/catch`（unless explicitly requested）
- No `TODO` comments（unless explicitly requested）
- No systems not defined in an approved GDD
- No self-refactoring of untouched code
- No magic numbers — 數值一律來自 GDD 或 ScriptableObject
- No `GameObject.Find` / `FindObjectOfType` in runtime hot paths
- No per-frame `GetComponent` calls — 快取於 `Awake`
- No committing `Library/`, `Temp/`, `Logs/`, `*.csproj`, `*.slnx`
- No networking / multiplayer / authority-check scaffolding — 本作為純單機，**不為多人預留架構**。
  邏輯與表現分離是為了可測試性，不是為了 netcode。

---

## Folder Structure

自製內容一律放 `Assets/_Game/`，與 Package Manager 匯入的資源隔離。

```
Assets/_Game/
├── Scripts/     ← 依系統分子資料夾
├── Prefabs/
├── Scenes/
├── Art/
├── Audio/
└── Data/        ← ScriptableObject 數值資產
```

---

## Status Notes

- **架構未定**：系統分層、跨系統溝通模式（EventBus / DI / 直接引用）尚未決定，須由 `/create-architecture` 與 ADR 產出。
- **測試框架未建立**：尚未執行 `/test-setup`。
- **Unity 6.3 知識落差**：此版本可能超出 LLM 訓練資料範圍。執行 `/setup-engine` 以補齊版本專屬 API 差異的參考文件。
