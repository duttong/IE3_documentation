# Software Guide

IE3 is a flat Python application. `ie3.py` coordinates hardware drivers, timing, engineering data, chromatogram capture, and the PyQt user interface.

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
| `ie3.py` | Main instrument controller and runtime entry point |
| `display.py` | PyQt control panel and chromatogram display |
| `gc8890.py` | Agilent 8890 SOAP/HTTP driver and detector data handling |
| `valco.py` | Valco GSV/SSV serial valve driver |
| `omega.py` | Omega Platinum temperature controller driver |
| `lj.py` | LabJack T7 digital and analog I/O |
| `sync2boulder.py` | SFTP packaging and upload |
| `setup.py` | Chromatography timing, pressure, and sequence settings |
| `ie3_config.py` | Instrument-specific paths and hardware addresses |
