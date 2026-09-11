# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

---

## 專案概況

| 項目 | 值 |
|---|---|
| 專案名稱 | TD Wizard |
| 引擎 | Unity **6000.3.16f1**（Unity 6.3）|
| 渲染管線 | URP（Universal Render Pipeline）|
| 語言 | C# |
| 輸入系統 | New Input System（`Assets/InputSystem_Actions.inputactions`）|
| 目標平台 | **PC / Steam**（Windows 優先）|
| 版控 | git，`main` 分支，remote: `ChiCheLo/TD-Wizard` |
| 大型檔案 | Git LFS 已設定（見 `.gitattributes`）|

> **目前狀態：專案骨架階段。** `Assets/` 只有 URP 範本預設內容，尚無自製程式碼、場景或設計文件。遊戲類型、核心機制、系統架構**皆未定義**。

---

## Technology Stack

- **Engine**: Unity 6000.3.16f1
- **Language**: C#
- **Build System**: Unity Build Pipeline
- **Asset Pipeline**: Unity Asset Import Pipeline + Addressables

## Engine Version Reference

@docs/engine-reference/unity/VERSION.md

---

## Agent 行為規則（最高優先）

### 撰寫程式碼前必讀
- 命名規範：嚴格遵守 [.claude/docs/technical-preferences.md](.claude/docs/technical-preferences.md) → Naming Conventions
- 數值（塔傷害、波次、成本、CD 等）：只使用 GDD 中定義的值。**GDD 尚未建立前，不要自行發明任何數值。**
- 資料夾位置：新 Script 必須依「專案資料夾結構」放入正確子資料夾

### 禁止事項
- 不加 `Debug.Log`、`try/catch`、`TODO` 註解，除非我明確要求
- 不建立 GDD 未定義的系統或類別
- 不自行重構我沒有提到的程式碼
- 不假設未定義的數值，先問我
- 不自行 commit 或 push，除非我明確要求

### 文件建立規則
- **不主動建立 `.md` 說明文件**（README、使用說明、總結報告等一律不要）
- **例外**：遊戲開發 skill（`/brainstorm`、`/design-system`、`/create-architecture`、`/create-stories` 等）在其指定目錄寫入 markdown 是**允許且預期**的行為，見下方「文件產出位置」

### 遇到模糊情況
- 規格未定義 → 問我，不自行決定
- 多種實作方式 → 列出選項後等我確認，再動手

### 回覆格式
- 用**繁體中文**回覆
- 直接輸出程式碼，不加前言說明與結語總結
- 多檔案時用 `// Path: Assets/_Game/Scripts/XXX/XXX.cs` 標示路徑

---

## 專案資料夾結構

### 程式與資源（Unity）
```
Assets/
├── _Game/                  ← 所有自製內容放這裡，與套件資源隔離
│   ├── Scripts/            ← C# 程式碼（依系統分子資料夾）
│   ├── Prefabs/
│   ├── Scenes/
│   ├── Art/
│   ├── Audio/
│   └── Data/               ← ScriptableObject 數值資產
├── Settings/               ← URP 設定（Unity 產生，勿手動改）
└── InputSystem_Actions.inputactions
```

### 文件產出位置（遊戲開發 skill 使用）
```
.claude/docs/technical-preferences.md   ← 技術規範（20+ 個 skill 會讀）
design/gdd/                             ← 遊戲設計文件
design/architecture/                    ← 架構文件與 ADR
production/sprints/                     ← 衝刺規劃
production/qa/bugs/                     ← 缺陷追蹤
prototypes/                             ← 拋棄式原型
tests/                                  ← 測試
```

---

## 架構核心

**尚未定義。** 架構需先經 `/map-systems` → `/design-system` → `/create-architecture` 產出後才填入此處。

在架構確立前：
- 不要假設任何分層規則或跨系統溝通模式
- 不要建立 Manager / System 類別
- 若我要求寫程式但架構未定，先提醒我這點

---

## 效能預算（PC / Steam）

| 指標 | 目標 |
|---|---|
| 幀率 | 60 FPS（1080p，中階顯卡）|
| Draw Call | < 500 |
| RAM | < 2 GB |
| 啟動時間 | < 10 秒 |

> 塔防遊戲的特性是單位數量會隨波次成長，**大量同類單位的渲染與 AI 更新是主要效能風險**。實作敵人生成前先確認是否需要物件池或 GPU Instancing。

---

## Git 規範

- 分支：`main`（穩定）← `feature/xxx` / `fix/xxx`
- **Unity 專案禁止 commit 的目錄**：`Library/`、`Temp/`、`Logs/`、`*.csproj`、`*.slnx`（已在 `.gitignore`）
- `.meta` 檔案**必須**跟著資產一起 commit，不可遺漏
- 大型二進位資產（貼圖、音檔、模型）走 Git LFS

---

## 常用 skill 路線

本機安裝了 73 個遊戲開發 skill（位於 `~/.claude/commands/`）。以目前階段，建議順序：

1. `/start` — 引導式 onboarding，判斷該從哪開始
2. `/brainstorm` — 把 TD Wizard 的構想結構化成概念文件
3. `/setup-engine` — 將 Unity 6.3 釘入設定並補齊版本參考資料
4. `/map-systems` — 拆解系統與相依關係
5. `/design-system` — 逐系統撰寫 GDD

---

## 已知環境限制

- claude.ai 與 Figma 兩個 MCP connector **尚未授權**，需在 claude.ai 的 Settings → Connectors 連接
- `claude` CLI 不在 PATH 上（僅有 VSCode extension），無法使用 `claude mcp` 指令
- Unity MCP（`unity-mcp`）**可用**，已註冊於本專案
