# Unity 6.3 — New Features & Best Practices Since Training Cutoff

Last verified: 2026-09-11

New capabilities introduced in 6.3 that the LLM may not default to suggesting, with notes on
relevance to TD Wizard (tower defense, PC/Steam, URP).

## Box2D v3 (`UnityEngine.LowLevelPhysics2D`)

New low-level 2D physics API integrating Box2D v3: multi-threaded performance, improved
determinism, better debug visualization (Editor and Runtime). **Relevant if TD Wizard uses 2D
physics** (e.g., projectile paths, area-of-effect overlap checks) — prefer this over legacy
`Physics2D` for new systems once architecture decides on 2D vs 3D. If the game is 3D (towers/
enemies as 3D models on a 2D-ish map), this is likely not applicable — confirm with architecture
docs before assuming either way.

## Enhanced Audio Foundation

Opt-in via Project Settings → Audio → Audio Foundation = "Enhanced" (Windows/macOS only).
Benefits: no more audio engine reset/state loss when the default output device changes; device
enumeration/start/stop moved off the main thread (removes a class of main-thread hitches).
**Recommend enabling this for TD Wizard** given PC/Steam target — device-switching hitches
(e.g., user plugging in headphones mid-game) are a real annoyance in tower defense games with
constant SFX layering (tower fire, enemy death, wave start).

## UI Toolkit Runtime Improvements

- **Native SVG support** — crisp vector icons at any resolution. Good fit for tower/UI icons
  that need to scale cleanly across resolutions.
- **New UI target in Shader Graph** — custom UI shaders (e.g., stylized range-indicator circles,
  cooldown radial fills) can now be authored in Shader Graph instead of hand-written shaders.
- **Runtime data binding** — UI elements can bind directly to game data/properties without
  boilerplate glue code. Worth using for HUD elements that update frequently (gold count, wave
  number, tower stats panel) instead of manual `Update()`-driven label refreshes.
- **New runtime controls**: `TabView`, `ToggleButtonGroup` — useful for a tower-selection
  sidebar or build-menu categories.

## VFX Graph

Instancing support for GPU events across URP/HDRP, plus new samples/templates. Relevant once
VFX work starts (tower muzzle effects, enemy death effects, wave-clear celebration) — GPU
instancing matters here specifically because tower defense scales up VFX instance count with
wave progression.

## URP Render Graph (Mandatory, Not Optional)

Since Compatibility Mode is removed (see `breaking-changes.md`), **all custom renderer features
must target the URP Render Graph API** from the start. Don't scaffold renderer features using
older non-RenderGraph tutorials/patterns even if they're more common in training-era material.

## Open Question for This Project

None of the above has been evaluated against TD Wizard's actual architecture yet (none exists —
see `CLAUDE.md` status). Revisit this file once `/create-architecture` determines 2D vs 3D
approach and rendering needs, and prune sections that turn out irrelevant.
