# Unity 6.3 — Breaking Changes

Last verified: 2026-09-11

Changes introduced by Unity 6.3 (6000.3.x) relative to the LLM's training-era baseline
(~Unity 6000.0–6000.2 / 2023.x). Check this before writing or reviewing code that touches
these areas.

## Rendering

- **URP Compatibility Mode is fully removed.** `RenderGraphSettings.enableRenderCompatibilityMode`
  is now **read-only and always returns `false`**. Any project still relying on Compatibility
  Mode must be converted to URP Render Graph on Unity 6.0–6.2 *before* upgrading to 6.3 — there
  is no fallback once on 6.3. **Do not suggest Compatibility Mode-era renderer feature code
  (`ScriptableRenderPass` patterns that assume non-RenderGraph execution) for this project.**

## Scripting / Attributes

- `[SerializeField]` may now **only** be applied to fields. Applying it to a property, method,
  type, or other code element is a **compile-time error** (previously this may have silently
  no-op'd or warned). Audit any generated code that puts `[SerializeField]` on non-field members.

## Platform Support

- Minimum supported Android version raised to **7.1 (API level 25)**.
- `x86-64` (Magic Leap) target architecture support is limited to **existing projects only** —
  do not recommend it for new configuration.
- The legacy ETC texture compression mode is **removed**; projects using it are auto-migrated to
  the default ETC compressor. Don't suggest manually configuring "Legacy ETC" in import settings.

## Networking

- `UnityWebRequest` now uses **HTTP/2 by default**. If TD Wizard ever adds a backend/leaderboard
  call, don't assume HTTP/1.1-only server behavior when advising on request configuration.
- **Multiplay Hosting** is no longer supported in Editor/runtime as of 6.3 (service itself
  shuts down 2026-03-31). **Not applicable — TD Wizard is confirmed single-player
  (decision 2026-09-11, see `production/NEXT-STEPS.md`).** Listed only so the constraint is
  on record if that decision is ever revisited.

## UI

- **Multiplayer Widgets are deprecated** in favor of "Unity Building Blocks." Not applicable —
  TD Wizard is single-player and has no multiplayer UI.

## Not Yet Verified

This list reflects what surfaced in initial research (2026-09-11) and is **not guaranteed
exhaustive**. Re-run `/setup-engine refresh` before major upgrades or when an agent hits an
unexpected compile error that looks version-related, and append findings here.
