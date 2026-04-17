# Startup and Shutdown

## Normal Startup

The normal startup method is the `IE3 Run` desktop launcher. The launcher activates the IE3 conda environment and starts `run_ie3_loop.sh`.

From a terminal, the matching autonomous entry point is:

```bash
run_ie3_loop.sh
```

`run_ie3_loop.sh` starts `ie3.py` and only restarts it automatically after a controlled restart-at-end exit. This is the intended path for normal unattended operation.

Direct `ie3.py` modes are available for maintenance, diagnostics, development, or explicit control of a running process. They are not the normal operator startup path:

```bash
ie3.py --status
ie3.py --prerun
ie3.py --safe
ie3.py --warmup
ie3.py --test
ie3.py --cleanup
ie3.py --idle
```

## Controlled Restart

Use a controlled restart when code or configuration has changed but the current sequence should finish cleanly:

```bash
ie3.py --restart-at-end
```

The running process finishes the current sequence, writes final data, exits with the dedicated restart code, and the wrapper starts a fresh process.

## Graceful Stop

To ask a running process to stop:

```bash
ie3.py --stop
```

To stop from the GUI, close either the chromatogram window or the `IE3 Control Panel` window.

## Full Instrument Power Down

For a complete IE3 system shutdown and restart, use the [power down and power up procedure](../technicians/power-cycle.md). That procedure includes the order for calibration tanks, `N2`, the Linux computer, Agilent GC, and UPS power.

## SSH Display

If connected over SSH and the Qt windows should appear on the instrument's local monitor instead of the forwarded X11 session:

```bash
nohup env DISPLAY=:0 XAUTHORITY=$HOME/.Xauthority run_ie3_loop.sh >/tmp/ie3.out 2>&1 &
```

Use this only when you understand which display session owns the instrument desktop.
