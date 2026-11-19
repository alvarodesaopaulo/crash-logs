# Experimental GAMMA Procedural Movement Animations tuning

These files are an experimental LTX-only tuning pass based on comparing the supplied GAMMA configuration with the original PMA Lua controller.

## Installation

1. Back up the currently installed GAMMA PMA LTX files.
2. Use these files as replacements for the corresponding GAMMA LTX patches, preserving the same relative paths and patch-loading arrangement used by your installation.
3. Keep GAMMA's `procedural_movement.script`. Do **not** install the original PMA `procedural_movement.script` as part of this test.
4. Test each movement state in-game. Restore the backup if you encounter unexpected blending or animation behavior.

## Parameter comparison

The format is `animation path, playback speed, power`.

| Layer | State | GAMMA supplied | Experimental |
|---:|---|---:|---:|
| 0 | Aim walk | 0.8, 0.4 | 1.0, 0.2 |
| 1 | Aim crouch | 0.8, 0.4 | 0.7, 0.2 |
| 2 | Crouch | 0.8, 0.6 | 1.0, 0.3 |
| 3 | Walk | 0.8, 0.8 | 1.0, 0.5 |
| 4 | Run | 0.9, 1.25 | 1.0, 0.6 |
| 5 | Sprint | 1.15, 0.75 | 0.8, 0.8 |
| 6 | Aim idle | 0.5, 0.2 | 0.5, 0.2 |
| 7 | Idle | 0.5, 0.7 | 0.5, 0.7 |

The experimental values for layers 0–5 are based on the original controller's `game.play_hud_anm()` arguments:

- Aim walk: speed 1.0, power 0.2.
- Aim crouch: speed 0.7, power 0.2.
- Crouch: speed 1.0, power 0.3.
- Walk: speed 1.0, power 0.5.
- Run: speed 1.0, power 0.6.
- Sprint: speed 0.8. The original controller uses power 0.5 for generic sprint and 0.8 for pistol/rifle sprint; this single GAMMA layer cannot reproduce that weapon-dependent distinction. The experimental value 0.8 favors the pistol/rifle setting.

Idle and aim-idle retain the original controller's values.

## Important differences and limitations

- This changes movement-layer tuning only. It does not replace GAMMA's Lua controller or add new Lua behavior.
- The original PMA configuration used `movement\\none.anm` for all eight layers and manually played animations from Lua. GAMMA instead configures movement layers in LTX. These are different architectures; do not combine the original controller with this LTX setup without testing.
- The original controller selected among `sprint_generic.anm`, `sprint_pistol.anm`, and `sprint_rifle.anm`. GAMMA's supplied LTX uses one `movement\\sprint.anm` layer, so this configuration does not reproduce weapon-specific sprint selection.
- The proposed values are reasoned starting points, not verified in-game results. Playback speed and blend power may feel different under GAMMA's engine-side movement-layer implementation than under the original Lua calls.
- These files do not configure backpack animations. They also do not establish or fix any backpack/HUD animation-channel conflict.

## Files

- `mod_system_pma.ltx`: movement layers 0–5 and 7, plus the existing base setting.
- `mod_system_pma_idle_aim.ltx`: movement layer 6 (aim idle), kept separate as in GAMMA.
