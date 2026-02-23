# Umami Voron 2.4 Klipper Config

## Overview
This repo contains the full Klipper configuration set for the printer.
`printer.cfg` is the entry point and includes modular files under `cfg/`,
plus probe and macro configs. The repo root also contains service configs
(`moonraker.conf`, `crowsnest.conf`, etc.) and slicer notes.

## Key Paths
- `cfg/ui.cfg`: Symlink to Mainsail config (`/home/voron/mainsail-config/mainsail.cfg`).
- `stealthburner_leds.cfg`: Stealthburner LED status macros (included by `printer.cfg`).
- `toolhead.cfg`: Toolhead and CAN-related config.
- `macros/`: G-code macros by function (see below).
- `probes/`: Probe configurations and macros (see below).

## Klicky
- `probes/klicky/klicky-probe.cfg` is the entry point.
- Other files in `probes/klicky/` are included from there and should remain together.

## Macro Files
- `macros/macros.cfg`: General gcode macros (e.g., `PRINT_START` and helpers).
- `macros/homing.cfg`: Sensorless homing macros.
- `macros/bedfans.cfg`: Bed fan control macros.
- `macros/z_calibration.cfg`: Z calibration plugin config/macros.

## Notes
- Leave `printer.cfg` at the repo root.
- If you move files again, update the include paths in `printer.cfg` and
  `probes/klicky/klicky-probe.cfg` (relative includes).

## Third‑Party Modifications (Local Changes)
These files originate from upstream projects but have been modified locally in this repo:
- `cfg/probes/klicky/klicky-variables.cfg`: updated bed size limits (`variable_max_bed_x/y`) to 350.
- `cfg/probes/klicky/klicky-macros.cfg`: `PROBE_CALIBRATE` moves to bed center before calibration and adds a manual-step note.
- `stealthburner_leds.cfg`: LED data pin set to `EBBCan:gpio16` for the SB2209 toolhead.
- `cfg/macros/bedfans.cfg`: M190 includes `status_heating/status_ready` hooks; bed fans use a single pin.
- `cfg/macros/z_calibration.cfg`: added `DEBUG_ZCAL_STATE` helper macro for troubleshooting.

## Macro Cheat Sheet
- `PRINT_START` — main start sequence.
- `PRINT_END` — end sequence.
- `G32` — QGL + park.
- `PARK` — park toolhead.
- `M141` — set chamber temp target (wraps `SET_TEMPERATURE_FAN_TARGET`).
- `PROBE_CALIBRATE` — Klicky probe calibration flow.
- `PROBE_ACCURACY` — Klicky probe repeatability test.
- `BED_MESH_CALIBRATE` — Klicky‑aware mesh routine.
- `QUAD_GANTRY_LEVEL` — Klicky‑aware QGL routine.
- `RUN_CALCULATE_SWITCH_OFFSET` — helper for z_calibration switch offset.
- `BEDFANSSLOW` / `BEDFANSFAST` / `BEDFANSOFF` — manual bed fan control.
### Overrides (Replace Built-ins)
- `M190` — wait for bed temperature; also manages bed fans and uses `TEMPERATURE_WAIT`.
- `M109` — wait for hotend temperature; passes through to the built-in with status updates.
- `M140` — set bed temperature; routes through `SET_HEATER_TEMPERATURE` so bed fans logic is applied.
- `SET_HEATER_TEMPERATURE` — adds bed fan control based on target temperature.
- `TURN_OFF_HEATERS` — turns off bed fans then calls the built-in heater shutdown.
