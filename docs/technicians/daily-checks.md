# Daily Checks

## Chromatograms

Review the three chromatogram channels during normal operation. Watch for:

- missing chromatograms
- unusual baseline changes
- missing, shifted, or unusually shaped peaks
- repeated bad injections
- channel behavior that changes suddenly from the recent pattern

## Pressures

Check the important tank pressures:

- `N2` carrier gas
- `cal1`
- `cal2`
- `reference`

Record cylinder pressures about once per week in the `Cylinders` tab. Record changes, regulator adjustments, and unusual readings in the `Log` tab.

## Sample Flows

Check the recorded mean sample flows on the `SSV Info` tab. These flows are stored values from the most recent sample on each air stream. The target flow for each port is `50 +/- 3 cc/min`. If the flows are significantly out of range, see the [flow-balancing instructions](flow-balancing.md).

## Omega Temperature Controllers

Check the four Omega temperature controllers. From top to bottom, the current setpoints are:

| Controller | Setpoint | Description |
| --- | ---: | --- |
| Top | `190.0 C` | Channel 1 post-column |
| Second | `110.0 C` | Channel 1 column |
| Third | `35.0 C` | Channel 2 column |
| Bottom | `34.0 C` | Channel 3 column |

If a controller is far from setpoint, leave a log note and contact the project maintainer before changing PID or method settings.

## Data Sync

Daily sync is expected to send recent chromatogram run directories plus the cylinder and operator logs to Boulder. See [data sync](../operations/data-sync.md) for the operational details.
