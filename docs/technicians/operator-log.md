# Operator Log

Use the `Log` tab to record events that will matter during later data review or instrument troubleshooting.

Good log entries include:

- regulator adjustments
- flow balancing
- sample line changes
- unusual chromatogram behavior
- software restarts
- anything that could explain a data-quality issue

Some common events are logged automatically by the software:

- cylinder changes are logged when the operator completes the `N2 Swap` or `Cal Swap` button sequence in the `Cylinders` tab
- flask sampling start and finish events are logged when the operator uses the `Start Flask Sampling` button

## Timestamp Behavior

The `Log` tab shows the current UTC time. Each new entry uses the current UTC timestamp at the moment it is saved.

The operator name or initials are retained between entries so repeated notes can be entered quickly.

Entries are saved to:

```text
logs/log_entries.csv
```

This file is synced to Boulder daily.

## Flask Sampling Button

The `Start Flask Sampling` button is in the `Timing & Sequence` section of the control panel.

When flask sampling is active, any run step that would normally select `air1` on SSV position `3` is automatically redirected to `air2` on SSV position `7`.

Use this mode when a flask is connected and the run sequence should avoid drawing from `air1`.

The button is a toggle:

- press once to enable flask sampling mode
- press again to return to the normal SSV sequence

If the button is left on, a timeout guard returns the software to normal after the configured timeout period.

Flask sampling state changes are written to the operator log automatically by this button:

- `Flask Sampling Initiated`
- `Flask Sampling Finished`
- `Flask Sampling Finished - timeout encountered.`
