# IE3 Operator Notes

<p align="center">
  <img
    src="assets/IE3_software.png"
    alt="Screencapture view of the IE3 desktop showing the IE3 control panel and chromatograms."
    width="720"
    style="width: 100%; max-width: 720px; height: auto;"
  >
</p>

<p align="center"><em>Screencapture view of the IE3 desktop showing the IE3 control panel and chromatograms.</em></p>

## Normal Startup and Shutdown

<table>
  <tr>
    <td valign="top">

When the IE3 computer restarts, the acquisition software should start automatically.

For normal startup, the **preferred method is to double click the `IE3 Run` icon on the desktop**.

  </td>
    <td width="96" valign="top" align="center">
      <img
        src="assets/ie3.png"
        alt="IE3 Run desktop icon."
        width="64"
        style="width: 64px; height: auto;"
      >
      <br>
      <em><code>IE3 Run</code> desktop icon.</em>
    </td>
  </tr>
</table>

To stop the software safely, press the **`X`** in the top right corner, close either:

- the chromatogram window
- the `IE3 Control Panel` window

Closing either window should stop the Qt application and run the normal shutdown path.

The program can also be started from a terminal by running:

```bash
ie3.py
```

## Daily Checks

Familiarize yourself with the three chromatogram channels and what they are supposed to look like. This is the best routine check for data quality.

Important parameters to monitor:

- `N2` tank pressure
- `cal1` tank pressure
- `cal2` tank pressure
- `reference` tank pressure

Also, check the four Omega temperature controllers. From top to bottom, the current setpoints are:

- Top: `190.0 C` &mdash; Channel 1 Post-Column
- Second: `110.0 C` &mdash; Channel 1 Column
- Third: `35.0 C` &mdash; Channel 2 Column
- Bottom: `34.0 C` &mdash; Channel 3 Column

## Balancing Sample Flows

Periodically, the sample flows for calibration tanks and air inlets may need to be adjusted. This can now be done from within the running IE3 program using the **`SSV Info`** tab — no need to stop and restart the software.

Target sample flow: **`50 ± 3 cc/min`**

To balance flows:

1. Click the **`SSV Info`** tab in the IE3 Control Panel.
2. Press **`Stop Sequence`** and confirm. The SSV will park at a stop port. The `↑` / `↓` / `Home` buttons will become active.
3. Press **`Sample Closed`** to open the sample loop. The button label will change to confirm the new state.
4. Use the **`↑`** and **`↓`** buttons to step the SSV to each odd-numbered port.
5. Wait ~5 seconds at each port for the live flow reading in the `Flow Statistics` table to stabilize.
6. Adjust the regulator for that port until flow is on target:
   - Calibration tanks: adjust the tank regulator.
   - Air lines: adjust the back-pressure regulator on the pump board.
7. Repeat for all odd-numbered ports. The current port-to-tank/air assignments are listed on this tab or the **`SSV Reference`** table on this tab.
8. When finished, press **`Restart Run Sequence`** (the same button, which will have changed label) to resume normal operation.

## SSV Position Table

The **`SSV Reference`** table is on the **`SSV Info`** tab. It shows the configured port-to-tank and port-to-air assignments.

## Logging Events and Changes

Use the `Log` tab in the IE3 control panel to view and create log messages. The tab shows the current UTC time, and each saved entry always uses the current UTC timestamp at the moment it is written. The software also retains the operator's name (or initials). Log events such as cylinder changes, system adjustments, and notes that may help with data processing. Flask sampling events will be captured in the log file with the `Flask Sampling Button` detailed below.

Logging events are stored in the `logs/log_entries.csv` file.

## Flask Sampling Button

The `Start Flask Sampling` button is located in the `Timing & Sequence` section of the control panel.

- Default state: `Start Flask Sampling` shown in light green
- Active state: `Flask Sampling in Progress` shown in yellow

When flask sampling is active, any run step that would normally select `air1` on SSV position `3` is automatically redirected to `air2` on SSV position `7`.

Use this mode when a flask is connected, and you want the run sequence to avoid drawing from `air1`.

The button is a toggle:

- Press once to enable flask sampling mode
- press again to return to the normal SSV sequence

There is also a timeout guard. If the button is left on, it will automatically return to normal after the configured timeout period.

Flask sampling state changes are also written to the operator log automatically:

- `Flask Sampling Initiated`
- `Flask Sampling Finished`
- `Flask Sampling Finished - timeout encountered.`

The log entry uses the operator name or initials currently entered in the `Log` tab.

## Cylinder Pressures

The `Cylinders` tab is used to record important cylinder pressures.

The table includes five cylinders:

- `AAL072269`
- `ALM-064967`
- `ALM067691`
- `N2 Carrier Gas`
- `CO2 Dopant`

How to use this tab:

1. Go to the `Cylinders` tab.
2. Check the `Current UTC` line near the top of the card.
3. Enter the current pressure values in the yellow `Pressure` cells only.
4. Update these values about once a week when cylinder pressures are checked.
5. Press `Save` to record the current UTC time and write the values to the cylinder pressure log.

If `logs/tank_inventory.csv` is edited manually, the `Cylinders` tab will reload that file automatically at the start of the next chromatogram cycle.

The other columns are calculated automatically:

- `Logged Date` shows the most recent saved reading time for that cylinder
- `Daily use` shows the estimated pressure drop in `psi/day`
- `Change by` shows the estimated date the cylinder will reach `100 psi`

If a pressure is entered incorrectly, save a corrected value for the same cylinder within 1 hour. The software will treat the later value as the correction and ignore the earlier value when calculating `Daily use` and `Change by`.

If a cylinder is replaced with a new full tank, the calculation will reset when the next saved pressure is more than `25 psi` higher than the previous reading. Smaller upward changes are treated as pressure measurement noise and do not increase the high-pressure point used for the `Daily use` estimate.

The `Change by` date is an estimate and depends on accurate regulators and pressure measurements. Use operator judgement when deciding to replace cylinders.

Pressures are logged in the `logs/cylinder_pressures.csv` file.

## Return to Normal Operation

After balancing flows, press **`Restart Run Sequence`** in the `SSV Info` tab. The SSV sequence will resume from the beginning and data collection will continue normally.


## Desktop Icon Troubleshooting

If the `IE3 Run` or `IE3 Idle` desktop icons disappear after login or reboot, reload the GNOME desktop-icons extension from a terminal:

```bash
gnome-extensions disable desktop-icons@gnome-shell-extensions.gcampax.github.com
gnome-extensions enable desktop-icons@gnome-shell-extensions.gcampax.github.com
```
