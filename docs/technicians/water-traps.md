# Water Traps

Use the `H2O Traps` tab to check and adjust the two Peltier water traps that dry the sample stream before it reaches the drying system and GC.

## Live Readings

The table shows, for each trap:

| Column | Meaning |
| --- | --- |
| `Trap` | Trap label (`water_trap_1`, `water_trap_2`) |
| `Setpoint (°C)` | Current commanded setpoint |
| `Actual (°C)` | Most recently read temperature |

The `Last read` line above the table shows when the values were last updated from the controllers. Readings refresh automatically every few seconds.

Normal operating temperature is `4.0 °C`. Allow 30–60 minutes after startup for a trap to reach setpoint.

## How to Change a Setpoint

1. Open the `H2O Traps` tab.
2. Edit the yellow `Setpoint (°C)` cell for the trap you want to change.
3. Press `Enter`.

The new setpoint applies immediately. Setpoints must stay above freezing to avoid ice forming in the trap; the software rejects setpoints outside `0-100 °C`.

Setpoints can also be changed from the command line with `water_traps.py --set`; see [configuration](../software/configuration.md#water_trapsyaml).

## Water Trap Solenoid

The `Water Trap Open` / `Water Trap Closed` button controls the shared solenoid for both traps. The software also opens and closes this solenoid automatically each chromatogram cycle; a manual button press bypasses the automatic timing.

## When to Escalate

- A trap reading `-999` means the controller did not respond. Check that it is powered on and that the RS-485 cable is connected, then leave a log note.
- If one trap's temperature swings by several degrees while the other holds steady near setpoint, this usually points to a difference in controller configuration rather than sensor noise. Leave a log note and contact the project maintainer rather than changing settings from the controller's front panel.
