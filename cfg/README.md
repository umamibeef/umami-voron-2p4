# Klipper Config Layout

This folder groups Klipper include files so `printer.cfg` stays small and
focused on core machine configuration. Each file here is included from
`printer.cfg`.

Files:
- `ui.cfg`: UI/misc helpers and Mainsail-related config.
- `toolhead.cfg`: Toolhead and CAN-related config.
- `macros/`: G-code macros by function (see below).
- `probes/`: Probe configurations and macros (see below).

Klicky:
- `probes/klicky/klicky-probe.cfg` is the entry point.
- Other files in `probes/klicky/` are included from there and should remain together.

Macros:
- `macros/macros.cfg`: General gcode macros (e.g., `PRINT_START` and helpers).
- `macros/homing.cfg`: Sensorless homing macros.
- `macros/bedfans.cfg`: Bed fan control macros.
- `macros/z_calibration.cfg`: Z calibration plugin config/macros.

Notes:
- Leave `printer.cfg` at the repo root.
- If you move files again, update the include paths in `printer.cfg` and
  `probes/klicky/klicky-probe.cfg` (relative includes).
