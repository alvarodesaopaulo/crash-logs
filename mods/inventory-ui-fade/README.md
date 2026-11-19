# Inventory UI Fade (experimental)

Adds a 200 ms fade-in and fade-out to the GAMMA/Anomaly inventory UI.

## Install

1. Back up your installed `gamedata/scripts/ui_inventory.script`.
2. Install this folder as a separate MO2 mod, preserving the relative path `gamedata/scripts/ui_inventory.script`, or copy that file over the corresponding file in your active GAMMA UI mod.
3. Enable the mod after the UI mod that supplies the inventory script, so this file wins file conflicts.

## What it changes

- Starts a fade-in after the inventory dialog is shown.
- Delays the existing hide/unregister sequence until fade-out completes.
- Advances the animation in `UIInventory:Update()`.
- Recursively scans Lua tables owned by the inventory instance, including referenced controls in item-cell containers, cells, equipment slots, and tooltip objects.
- Fades texture alpha and text alpha separately while preserving RGB and original alpha.
- Re-scans during the animation to include controls created dynamically.

## Compatibility / testing notes

This is an experimental first build, not a verified release. It uses `GetTextureColor`, `SetTextureColor`, `GetTextColor`, and `SetTextColor`. Availability may differ between X-Ray executable branches/builds. Color access is protected with `pcall`, so unsupported accessors should be skipped rather than stopping the inventory script.

The scan can only reach controls stored in Lua tables owned by `UIInventory`. XML-created child controls that are not retained in those tables may remain visible during the fade. In-game testing is required to identify any such controls. If the inventory fails to open or Lua errors appear, disable the mod and restore your backup; send the relevant `xray_*.log`.

Default duration: 200 ms. Change `INVENTORY_FADE_DURATION` near the added code to adjust it.
