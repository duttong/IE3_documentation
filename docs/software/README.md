# Software Guide

IE3 is a flat Python application. For normal autonomous operation, the desktop `IE3 Run` launcher starts `run_ie3_loop.sh`; that wrapper starts `ie3.py` and handles controlled restart-at-end exits. `ie3.py` coordinates hardware drivers, timing, engineering data, chromatogram capture, and the PyQt user interface.

## Main Pages

- [Architecture](architecture.md): module responsibilities and hardware boundaries.
- [Runtime modes](runtime.md): command-line modes, continuous operation, restart behavior, and timers.
- [Configuration](configuration.md): hardware configuration, chromatography setup, SSV positions, and Omega PID values.
- [Data files](data-files.md): chromatogram, engineering, metadata, cylinder, operator, and sample-flow files.
- [UI design](ui-design.md): visual conventions for the PyQt control panel.
- [Development workflow](development.md): local setup, smoke checks, display simulation, and public documentation rules.

## Main Source Modules

| Module | Responsibility |
| --- | --- |
| `run_ie3_loop.sh` | Normal autonomous entry point used by the desktop launcher |
| `ie3.py` | Main instrument controller; direct modes are for maintenance, diagnostics, and development |
| `display.py` | PyQt control panel and chromatogram display |
| `gc8890.py` | Agilent 8890 SOAP/HTTP driver and detector data handling |
| `valco.py` | Valco GSV/SSV serial valve driver |
| `omega.py` | Omega Platinum temperature controller driver |
| `lj.py` | LabJack T7 digital and analog I/O |
| `sync2boulder.py` | SFTP packaging and upload |
| `setup.py` | Chromatography timing, pressure, and sequence settings |
| `ie3_config.py` | Instrument-specific paths and hardware addresses |
