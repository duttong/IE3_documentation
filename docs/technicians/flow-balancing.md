# Flow Balancing

Sample flows for calibration tanks and air inlets may need periodic adjustment. This can be done from the running IE3 program in the `SSV Info` tab.

Target sample flow: `50 +/- 3 cc/min`

## Procedure

1. Open the `SSV Info` tab in the IE3 Control Panel.
2. Press `Stop Sequence` and confirm. The SSV parks at a stop port. The step buttons become active.
3. Press `Sample Closed` to open the sample loop. The button label changes to confirm the new state.
4. Use the up and down buttons to step the SSV to each odd-numbered port.
5. Wait about 5 seconds at each port for the live flow reading in the `Flow Statistics` table to stabilize.
6. Adjust the regulator for that port until flow is on target.
7. Repeat for all odd-numbered ports.
8. Press `Restart Run Sequence` to resume normal operation.

## What to Adjust

| Port type | Adjustment |
| --- | --- |
| Calibration tank | Tank regulator |
| Air inlet | Back-pressure regulator on the pump board |

## SSV Reference

The `SSV Reference` table on the `SSV Info` tab shows the current port assignments. In the current configuration, odd ports are active sample positions and even ports are stop ports.

| SSV position | Assignment |
| ---: | --- |
| `1` | high standard |
| `3` | `air1` |
| `5` | reference |
| `7` | `air2` |
| `9` | low standard |

If the assignments on the instrument differ from this table, use the values shown in the running software. The software reads the table from `ssv_positions.yaml`.

## Return to Normal Operation

After balancing flows, press `Restart Run Sequence`. The run sequence restarts from the beginning.
