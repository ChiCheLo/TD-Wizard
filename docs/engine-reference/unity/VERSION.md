# Unity — Version Reference

| Field | Value |
|-------|-------|
| **Engine Version** | 6000.3.16f1 (Unity 6.3 LTS) |
| **Project Pinned** | 2026-09-11 |
| **LLM Knowledge Cutoff** | January 2026 |
| **6000.3.0f1 Release Date** | 2026-01-21 |
| **6000.3.16f1 Release Date** | 2026-05-20 |
| **Risk Level** | **HIGH** — version released at/after LLM knowledge cutoff |
| **Last Docs Verified** | 2026-09-11 |

## Why This Is High Risk

Unity 6.3 LTS (6000.3.0f1) shipped on the same month as the LLM's knowledge cutoff and introduced
several significant new subsystems (Box2D v3, Enhanced Audio Foundation, native SVG in UI Toolkit,
runtime UI data binding). The installed patch (6000.3.16f1) is 4 months and 16 patch releases
further ahead. Code suggestions should not assume deep familiarity with 6.3-specific APIs without
checking `breaking-changes.md` and `deprecated-apis.md` first.

## Post-Cutoff Version Timeline

| Version | Released | Notes |
|---|---|---|
| 6000.3.0f1 | 2026-01-21 | First 6.3 LTS release. Box2D v3, Enhanced Audio Foundation, hybrid 2D/3D scenes. |
| 6000.3.2f1 – 6000.3.15f1 | 2026-02 – 2026-05 | Incremental patches |
| 6000.3.16f1 | 2026-05-20 | **Installed version.** UI Toolkit / Painter2D / Shader Graph preview fixes. |

## Before Suggesting Unity 6.3 Code

1. Check `breaking-changes.md` for anything touching Compatibility Mode, Android minimum version, or ETC compression.
2. Check `deprecated-apis.md` before using `[SerializeField]` on non-fields, or `AccessibilityNode.selected`.
3. Check `current-best-practices.md` for URP Render Graph, UI Toolkit runtime binding, and Box2D v3 (`UnityEngine.LowLevelPhysics2D`) usage.
4. If uncertain about an API's existence or signature in 6.3, use WebSearch rather than asserting from training data.

Run `/setup-engine refresh` periodically to catch new patch releases and re-verify this file.
