# BigTreeTech SKR 1.4 Turbo/TMC2209 Config for Průša MK3S

## Requirements
- [Průša MK3S, MK3S+, or MK3 with MK3S/MK3S+ upgrade](https://www.prusa3d.com/original-prusa-i3-mk3/) is required for filament runout to work properly.
- BigTreeTech SKR 1.4 Turbo motherboard
- 5 x BigTreeTech TMC2209s (DIAG pin location differs from Watterott's & similar designs)

## Upgrade Notes
* ⚠️ Cut or desolder the Z & E driver DIAG pins or they will interfere with PINDA & filament runout detection. ⚠️
* Set the jumpers under your drivers to "TMC2208-UART MODE":

  <img src="https://user-images.githubusercontent.com/13375512/74117621-24415000-4b6d-11ea-8811-f867e187ea0c.png" width="50%">

## Changes to Start G-code
The `W` in Průša's `G28 W ; home all without mesh bed level` default G-code does not exist in Marlin and [`G80 ; mesh bed leveling`](https://marlinfw.org/docs/gcode/G080.html) cancels the current motion mode, so no bed leveling will take place.

Below are some example start G-code scripts from popular slicers to get you started.

### PrusaSlicer
Paste the start G-code block below in the "Custom G-code" section under "Printer Settings". Use the "Custom G-code" section under "Filament Settings" to add the [Linear Advance](https://marlinfw.org/docs/features/lin_advance.html) (`M900 K0.0`) value since it can be saved on a per-filament basis.
```gcode
G90 ; use absolute coordinates
M83 ; extruder relative mode
M104 S170 ; preheat hotend to 170
M140 S[first_layer_bed_temperature] ; set bed temp
M190 S[first_layer_bed_temperature] ; wait for bed temp
M104 S[first_layer_temperature] ; set hotend temp
G28 ; home all
M420 S1 ; enable bed leveling
G1 X0 Z0.6 Y-2.0 F1000.0 ; go outside print area
M109 S[first_layer_temperature] ; wait for hotend temp
G92 E0.0
G1 X60.0 E9.0 F1000.0 ; intro line
G1 X100.0 E12.5 F1000.0 ; intro line
G92 E0.0
```
