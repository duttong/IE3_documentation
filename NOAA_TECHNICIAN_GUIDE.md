# IE3 Operator Notes
## March 26, 2026
## Contact: Geoff.Dutton@noaa.gov

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

Periodically, the sample flows for calibration tanks and air inlets may need to be adjusted.

To balance flows:

1. Stop the main IE3 program (press the **`X`** in the top right corner of the chromatogram or control panel screens).
2. The two screens should close and go away.
3. Double-click the **`IE3 Idle`** icon on the desktop.
4. Idle mode allows you to manually step the Stream Selection Valve (SSV) with the buttons on the valve.
5. Make sure the sample solenoid is open. Click/toggle the "Sample Closed" button on the IE3 control panel software. This button should change states and turn green.
6. When idle mode starts, it is safe to manually step the Stream Selection Valve (`SSV`). The SSV is the top-most valve on the valve panel. Use the up/down arrows on the valve panel to move through positions.
7. Systematically, step the SSV to the port positions for the calibration tanks and the air intake positions. These are all odd-number positions. There is an SSV position guide in the IE3 control panel software—press the "SSV position" tab to see the current tank and air assignments.
8. Check the `Sample Flow` value on the control panel at each odd-numbered position. Target sample flow: **`50 ± 3 cc/min`**.
- For calibration tanks, adjust the tank regulator.
- For air lines, adjust the back-pressure regulators on the pump board.
9. Once all flows are set to the target. Close the IE3 Idle program and restart the IE3 run program.

## SSV Position Table

There is an SSV position guide in the IE3 control panel software—press the `SSV Position` tab to see the current tank and air assignments.

## Logging Events and Changes

Use the `Log` tab in the IE3 control panel to view and create log messages. By default, the logging software will use the current date and time and retain the operator's name (or initials). Log events such as cylinder changes, system adjustments, and notes that may help with data processing. Flask sampling events will be captured in the log file with the `Flask Sampling Button` detailed below.

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

Only the `Pressure` column is edited manually. Press `Save` to record the current UTC time and write the values to the cylinder pressure log.

Pressures are logged in the `logs/cylinder_pressures.csv` file.

## Return to Normal Operation

After balancing, close idle mode and restart normal operation by double-clicking the `IE3 Run` icon on the desktop.


## Desktop Icon Troubleshooting

If the `IE3 Run` or `IE3 Idle` desktop icons disappear after login or reboot, reload the GNOME desktop-icons extension from a terminal:

```bash
gnome-extensions disable desktop-icons@gnome-shell-extensions.gcampax.github.com
gnome-extensions enable desktop-icons@gnome-shell-extensions.gcampax.github.com
```
