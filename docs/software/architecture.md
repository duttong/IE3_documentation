# Architecture

## Overview

IE3 runs as a Python 3.11 desktop instrument-control application. The main process creates an `Instrument_IE3` object, starts hardware drivers, schedules timed actions, receives detector data, writes output files, and launches the PyQt interface.

The design is intentionally direct:

- hardware-specific code lives in small driver modules
- timing and sequence behavior are configured in `setup.py`
- instrument-specific addresses and paths are configured in `ie3_config.py`
- the UI reads state from the running instrument object and calls high-level control methods

## Hardware Boundaries

| Hardware | Module | Notes |
| --- | --- | --- |
| Agilent 8890 GC | `gc8890.py` | SOAP/HTTP control plus detector data sockets |
| Valco valves | `valco.py` | One shared serial connection for GSV and SSV valves |
| Omega controllers | `omega.py` | Reads temperatures, setpoints, and writes PID or setpoint values |
| Peltier water trap controllers | `water_traps.py` | Dedicated RS-485 bus for the two Omega CNi trap controllers |
| LabJack T7 | `lj.py` | Digital outputs and analog pressure, flow, and temperature signals |
| PyQt UI | `display.py` | Operator controls, live status, logs, and chromatogram view |

## Valco Serial Bus

All GSV and SSV valve objects share one `serial.Serial` connection and a class-level lock. Critical movement commands block on the lock. Background position polling can call `cp(blocking=False)` so it skips if the serial bus is busy instead of delaying movement commands.

Do not open multiple serial handles to the same valve bus.

## Water Trap Bus

The two Peltier water trap Omega CNi controllers communicate over their own dedicated RS-485 bus, separate from the column-can Omega bus, because the two buses run at different baud rates. The column-can Omega autodetect logic excludes the water trap's configured port from its search, so it cannot mistake a reply from the wrong bus for a column controller.

Water trap temperatures and setpoints are read on a background timer and cached; the engineering-data timer reads from that cache rather than waiting on serial I/O. The background poll skips its own tick if the previous read has not finished, so two overlapping reads cannot collide on the shared connection.

## GC Ownership

The GC driver obtains a connection context and requests software ownership from the Agilent 8890. Commands that change GC state require ownership. The driver keeps the TCP session and license key paired and renews both together to avoid mismatched session/license state.

## User Interface

The PyQt interface includes:

- `Info`: live status, GC parameters, solenoid controls, and flask sampling
- `SSV Info`: sample flow statistics, flow balancing controls, and SSV reference table
- `Cylinders`: cylinder pressure logging and tank inventory display
- `Log`: operator event log
- `H2O Traps`: live Peltier water trap temperatures, setpoints, and solenoid control

The UI is not a separate service. It runs inside the same Python process as the instrument controller.
