# Cylinder Pressures

Use the `Cylinders` tab to record important cylinder pressures. The tab is designed for routine weekly pressure logging and simple usage projection.

## How to Log Pressures

1. Open the `Cylinders` tab.
2. Check the `Current UTC` line near the top of the panel.
3. Edit only the yellow `Pressure` cells.
4. Press `Save`.

Pressures are appended to `logs/cylinder_pressures.csv` on the instrument computer.

## Calculated Columns

| Column | Meaning |
| --- | --- |
| `Logged Date` | Most recent saved reading time for that cylinder |
| `Daily use` | Estimated pressure drop in `psi/day` |
| `Change by` | Estimated date the cylinder will reach `100 psi` |

Very slow use rates are displayed as `0` and `-` instead of a distant projected date.

## Corrections and Cylinder Changes

If a pressure was entered incorrectly, save a corrected value for the same cylinder within 1 hour. The software treats the later value as the correction for usage calculations.

If a cylinder is replaced with a full tank, the usage estimate resets when the next saved pressure is more than `25 psi` higher than the previous reading. Smaller upward changes are treated as measurement noise.

Use operator judgement when deciding when to replace cylinders. The `Change by` date is an estimate, not an automatic replacement instruction.

## Tank Inventory

The software also reads `logs/tank_inventory.csv`. If this file is edited manually, the `Cylinders` tab reloads it at the start of the next chromatogram cycle.
