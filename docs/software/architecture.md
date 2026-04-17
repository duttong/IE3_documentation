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
| LabJack T7 | `lj.py` | Digital outputs and analog pressure, flow, and temperature signals |
| PyQt UI | `display.py` | Operator controls, live status, logs, and chromatogram view |

## Valco Serial Bus

All GSV and SSV valve objects share one `serial.Serial` connection and a class-level lock. Critical movement commands block on the lock. Background position polling can call `cp(blocking=False)` so it skips if the serial bus is busy instead of delaying movement commands.

Do not open multiple serial handles to the same valve bus.

## GC Ownership

The GC driver obtains a connection context and requests software ownership from the Agilent 8890. Commands that change GC state require ownership. The driver keeps the TCP session and license key paired and renews both together to avoid mismatched session/license state.

## User Interface

The PyQt interface includes:

- `Info`: live status, GC parameters, solenoid controls, and flask sampling
- `SSV Info`: sample flow statistics, flow balancing controls, and SSV reference table
- `Cylinders`: cylinder pressure logging and tank inventory display
- `Log`: operator event log

The UI is not a separate service. It runs inside the same Python process as the instrument controller.
