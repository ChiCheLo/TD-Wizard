# Unity 6.3 — Deprecated APIs

Last verified: 2026-09-11

"Don't use X → Use Y" reference for Unity 6.3. Check before suggesting any of the left-column
APIs.

| Don't Use | Use Instead | Notes |
|---|---|---|
| `AccessibilityNode.selected` | `AccessibilityNode.invoked` | Renamed and deprecated in 6.3. |
| `[SerializeField]` on properties/methods/types | `[SerializeField]` on fields only | Now a **compile-time error**, not just a warning — will break the build, not just misbehave. |
| URP Compatibility Mode renderer setup (non-RenderGraph `ScriptableRenderPass`) | URP Render Graph API | Compatibility Mode is fully removed in 6.3; see `breaking-changes.md`. |
| Legacy ETC compression import setting | Default ETC compressor (auto-migrated) | Manual legacy config no longer exists. |
| Multiplayer Widgets package | Unity Building Blocks | Only relevant if/when multiplayer UI is scoped. |

## Not Yet Verified

This is a starting list, not exhaustive. If code review or implementation surfaces a
compile error referencing an unfamiliar obsolete-API warning, add it here with the
replacement API and the date verified.
