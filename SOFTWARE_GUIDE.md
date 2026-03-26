# Software Guide
## March 26, 2026

## Overview

The IE3 software is organized as a flat Python application. The low-level hardware communication is handled by small driver modules, and `ie3.py` coordinates those drivers into a complete run sequence, engineering data stream, and user interface.

## Network Layout

The IE3 computer uses two network cards.

- One network interface is used for the site or general network connection.
- The second network interface is connected directly to the Agilent 8890 GC.

The GC network address is configured in [ie3_config.py](/Users/gdutton/programming/IE3_acquisition/ie3_config.py) as:

```python
gcurl = 'http://10.1.2.101'
```

The GC-facing network card on the IE3 computer must be configured on the same subnet as the GC, for example `10.1.2.x`.

## Driver Modules

### `lj.py`

`lj.py` is the LabJack T7 driver. It opens the first detected LabJack, reads analog and digital channels, writes digital outputs, and provides the basic I/O used for solenoids, pressures, flows, and temperatures.

### `valco.py`

`valco.py` handles serial communication with the Valco valves (version 3.0). It supports both the 2-position Gas Sample Valves (`GSV`) and the multi-position Stream Selection Valve (`SSV`). It is responsible for reading valve position and sending movement commands such as `load`, `inject`, and `go=<position>`.

All valve objects share one `serial.Serial` connection (passed via `ser=` at construction) and a class-level `threading.Lock`. This ensures that commands from concurrent timer threads — position reads (`cp`) and critical movement commands (`go`, `goa`, `gob`) — never interleave on the shared RS-485 bus. The `cp()` method accepts a `blocking=False` argument; background polling calls in `update_valves_cp()` use this so that a critical movement command waiting for the bus is never delayed by a position read.

### `omega.py`

`omega.py` is the serial driver for the Omega Platinum temperature controllers. It reads live temperatures, reads setpoints, writes setpoints, and can also apply PID values from `omega_config.yaml`.

### `gc8890.py`

`gc8890.py` is the Agilent 8890 GC driver. It communicates with the GC over SOAP/HTTP and is organized into five sections:

- **Connection & License Management** — opens a TCP session, obtains a license key from the GC (`GetConnectionContext`), and requests software ownership (`RequestLicense`). The license key and TCP session are always kept paired; renewal via `renew_license_and_ownership()` opens a fresh session atomically under a lock so concurrent timer threads cannot race and create mismatched session/license pairs.
- **SOAP Communication Infrastructure** — low-level helpers (`header`, `_cmd_head`, `command`) that all GC requests are built on.
- **Read-only GC Queries** — `GetStatusA`, `GetMethod`, `GetID`, and related status parsing. These do not require ownership.
- **Owned GC Commands** — `SetMethod`, `SubscribeToData`, and related calls that require ownership. Auxiliary pressure setpoints are applied here.
- **Detector Data & Chromatogram Handling** — per-channel socket receivers, block decoding, chromatogram boundary management, and `.itx` file export.

## Main Runtime: `ie3.py`

`ie3.py` is the instrument controller. It creates the `Instrument_IE3` object, starts all hardware drivers, applies run modes, sequences the valves and pressures over time, writes chromatogram and engineering files, and launches the Qt interface in `display.py`.

Operationally, `ie3.py` is the program to start and stop:

- `ie3.py` starts normal operation
- `ie3.py --idle` starts idle mode
- `ie3.py --status` checks status without a full run
- `ie3.py --stop` sends a graceful stop signal to a running instance
- `ie3.py --restart-at-end` tells a running instance to finish the current sequence and then restart

If you are connected over SSH and want the Qt windows to appear on the instrument's
local monitor instead of the forwarded X11 session, start the software with:

```bash
nohup env DISPLAY=:0 XAUTHORITY=$HOME/.Xauthority ie3.py >/tmp/ie3.out 2>&1 &
```

For normal unattended operation, the preferred launcher is [run_ie3_loop.sh](/Users/gdutton/programming/IE3_acquisition/run_ie3_loop.sh). It runs `ie3.py` and only restarts it when the program exits with the dedicated restart-at-end exit code after a completed sequence. The desktop `IE3 Run` launcher now uses this wrapper.

The intended workflow is:

- start the software through `run_ie3_loop.sh`
- make code changes while the current sequence continues to run
- run `ie3.py --restart-at-end` to arm a controlled restart
- the current sequence finishes normally
- `ie3.py` exits with the planned restart code
- `run_ie3_loop.sh` launches a fresh `ie3.py` instance using the updated code

During continuous operation, output files now rotate between runs. A run that crosses UTC midnight is allowed to finish normally in its current `.itx`, engineering `.csv`, and `.log` files. The next run then starts in a fresh output set, including a new day directory if the UTC date changed.

The control panel in `display.py` now includes:

- `Info`: live instrument status, GC parameters, Flask Sampling control
- `Log`: operator event log saved to `logs/log_entries.csv` and synced to Boulder
- `Cylinders`: manual cylinder pressure log saved to `logs/cylinder_pressures.csv` and synced to Boulder

## Data Sync: `sync2boulder.py`

`sync2boulder.py` uploads IE3 data to the remote SFTP `incoming` directory. It packs each local chromatogram run directory (under `/export/home/ie3/chroms/`) as a `YYYYMMDD.tgz` tarball and uploads it. In addition to the run tarballs it always syncs two extra files:

- `logs/cylinder_pressures.csv` — cylinder pressure log
- `logs/log_entries.csv` — operator event log

Run directories with two or fewer `.itx` files are skipped as trivial. Remote files are never deleted.

```bash
sync2boulder.py                    # sync yesterday and today (default)
sync2boulder.py --last-days 3      # sync the last 3 days
sync2boulder.py --dir-name 20260324  # resend one specific day
sync2boulder.py -v                 # verbose SFTP output
```

## Configuration Files

### `ie3_config.py`

`ie3_config.py` contains instrument-specific hardware and site configuration. This is where the software looks up:

- the GC network address
- data storage paths
- valve addresses
- LabJack digital and analog channel assignments
- Omega controller addresses
- auxiliary pressure channel mappings

This file should be edited when the physical hardware mapping or site-specific paths change.

### `setup.py`

`setup.py` contains the operational and chromatography configuration. This is where the software defines:

- chromatogram duration
- sample flush timing
- backflush timing
- detector temperature and makeup setpoints
- auxiliary pressure setpoints
- solenoid timing
- cleanup behavior
- the run sequence
- `air1` / `air2` SSV assignments used by the Flask Sampling override
- the Flask Sampling timeout

The run sequence in `setup.py` is a Stream Selection Valve (`SSV`) sequence. In practice, this is the ordered list of SSV positions that the instrument will visit during a run. Each value in the sequence corresponds to an SSV port, which determines which tank or air inlet is routed into the system for that chromatogram.

## ITX Chromatogram File Format

Each chromatogram is written as an Igor Text File (`.itx`). One file contains all detector channels for a single injection.

### Wave note fields

Every channel wave carries an `X note` line with semicolon-delimited metadata:

```
X note chr1_01411, " 1; 0; HH:MM:SS; MM-DD-YYYY; selpos; sta; aux...; flag;"
```

| Field | Example | Description |
|-------|---------|-------------|
| `1` | `1` | Format version |
| `0` | `0` | Reserved |
| `HH:MM:SS` | `14:11:07` | Injection time (UTC) |
| `MM-DD-YYYY` | `03-25-2026` | Injection date (UTC) |
| `selpos` | `3` | SSV position at sample flush stop |
| `sta` | `smo` | Station/instrument code |
| `aux...` | `1011.14 1011.26 1008.51` | Space-separated auxiliary floats (e.g. pressures) defined in `setup.itx_aux_times` |
| `flag` | `SKIP` or `KEEP` | Data quality flag (see below) |

### SKIP / KEEP flag

The last semicolon-delimited field before the closing `"` is a quality flag:

- **`KEEP`** — normal, usable data.
- **`SKIP`** — first injection after an `ie3.py` start or `--restart-at-end` restart. The sample loop may not be fully conditioned and this chromatogram should not be used in processing.

The flag is set in `ie3.py` via `self.skip_first_injection`, which is `True` only when the Python process starts fresh. Day-boundary rollovers handled inside `continuous()` do **not** set this flag — their first injection is `KEEP`.

When parsing `.itx` files, split the note string on `; ` and read the last non-empty field as the quality flag.

### Other Config Files

- [ssv_positions.yaml](/Users/gdutton/programming/IE3_acquisition/ssv_positions.yaml): SSV port descriptions
- [omega_config.yaml](/Users/gdutton/programming/IE3_acquisition/omega_config.yaml): Omega PID values
