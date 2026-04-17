# Development Workflow

## Environment

Python 3.11 is the intended runtime. PyQt6 is the supported GUI stack.

Recommended conda environment:

```bash
conda create -n ie3-py311 -c conda-forge python=3.11 pyqt=6 matplotlib pandas numpy pyyaml beautifulsoup4 lxml pyserial requests python-dateutil
conda activate ie3-py311
python3 -m pip install -r requirements.txt
```

`labjack-ljm` may need to be installed separately on the target machine.

## Smoke Checks

Compile the Python files:

```bash
python3 -m py_compile *.py
```

Check status without starting a run:

```bash
python3 ie3.py --status
```

Run the display simulation:

```bash
python3 display.py
python3 display.py --cylinder-log logs/cylinder_pressures.csv
```

## Development Pattern

For operational changes on the instrument:

1. Let the current sequence continue running.
2. Make and review the code or configuration change.
3. Request a controlled restart:

```bash
ie3.py --restart-at-end
```

4. Confirm the next process starts cleanly.
5. Expect the first injection after restart to be flagged `SKIP`.

Normal autonomous operation should be started through `run_ie3_loop.sh`, usually by using the desktop `IE3 Run` launcher. Direct `ie3.py` modes are still useful for development checks, diagnostics, and explicit commands such as `--status`, `--stop`, and `--restart-at-end`.

## Public Documentation Rules

When documenting source behavior in this public repository:

- describe behavior and field meanings clearly
- avoid private credentials, host access details, and unpublished analysis
- avoid absolute paths from a developer laptop
- prefer relative links inside this repository
- update technician and software docs together when a feature affects both groups
