# Cylinder Pressures

Use the `Cylinders` tab to record important cylinder pressures. The tab is designed for routine weekly pressure logging and simple usage projection.

## How to Log Pressures

1. Open the `Cylinders` tab.
2. Check the `Current UTC` line near the top of the panel.
3. Edit only the yellow `Pressure` cells.
4. Press `Save`.

Pressures are appended to `logs/cylinder_pressures.csv` on the instrument computer. This file is synced to Boulder daily.

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

## Tank Swap Buttons

The lower tank inventory table on the `Cylinders` tab includes two buttons:

- `N2 Swap`
- `Cal Swap`

Use these buttons after the physical cylinder has already been changed. They are not planning buttons; they update the instrument records for a completed swap.

Finish every dialog after pressing a swap button. The button sequence updates more than one record, so canceling partway through can leave the inventory, operator log, or pressure log incomplete.

### N2 Swap

Use `N2 Swap` after replacing the N2 carrier-gas cylinder.

The software will ask for:

1. confirmation that the N2 tank has already been changed
2. operator name or initials
3. a log note
4. the pressure of the new N2 tank in `psi`

When completed, `N2 Swap`:

- adds an operator-log entry
- decreases the N2 full-cylinder inventory by 1
- increases the N2 empty-cylinder inventory by 1
- records the new N2 pressure in `logs/cylinder_pressures.csv`

### Cal Swap

Use `Cal Swap` after replacing one of the calibration or reference cylinders connected to the SSV.

The software will ask for:

1. confirmation that a calibration tank has already been changed
2. operator name or initials
3. which SSV calibration position was changed
4. which new tank serial number was installed
5. a log note
6. the pressure of the new tank in `psi`

The SSV positions shown in the dialog come from the current `ssv_positions.yaml` configuration. The available replacement tank serial numbers come from `logs/cal_tanks.csv`.

When completed, `Cal Swap`:

- adds an operator-log entry
- decreases the calibration full-cylinder inventory by 1
- increases the calibration empty-cylinder inventory by 1
- updates `ssv_positions.yaml` so the selected SSV port points to the new tank serial number
- refreshes the SSV and cylinder tables in the running display
- records the new tank pressure in `logs/cylinder_pressures.csv`

After using `Cal Swap`, verify that the `SSV Reference` table shows the expected tank on the expected port.

## Tank Inventory

The software also reads `logs/tank_inventory.csv`. If this file is edited manually, the `Cylinders` tab reloads it at the start of the next chromatogram cycle.
