# Water Trap Standalone Tool

`water_traps.py` reads and controls the two dedicated Omega CNi controllers used for the Peltier water traps. It runs standalone from the main IE3 application, using the same `omega.py` driver, and also exposes an importable reader class used by `ie3.py` for engineering-file integration. See [configuration](configuration.md#water_trapsyaml) for the `water_traps.yaml` fields this tool reads, and [water traps](../technicians/water-traps.md) for the `H2O Traps` tab in the main IE3 GUI.

## Common Commands

Read current temperatures once:

```bash
python3 water_traps.py
```

Read setpoints:

```bash
python3 water_traps.py --setpoints
```

Read temperatures and setpoints together:

```bash
python3 water_traps.py --once --setpoints
```

Set a trap setpoint by label:

```bash
python3 water_traps.py --set water_trap_1 10
```

Set a trap setpoint by Omega address:

```bash
python3 water_traps.py --set 2 10
```

Set both trap setpoints:

```bash
python3 water_traps.py --set water_trap_1 10 --set water_trap_2 10
```

Setpoints must stay above freezing; the tool rejects values outside `0-100 °C`.

Continuously log trap temperatures:

```bash
python3 water_traps.py --watch
```

Continuously log and print readings:

```bash
python3 water_traps.py --watch --stdout
```

Continuously log temperatures and setpoints:

```bash
python3 water_traps.py --watch --setpoints --stdout
```

## Runtime Overrides

The CLI can override config values without editing `water_traps.yaml`:

```bash
python3 water_traps.py --port /dev/ttyUSB2
python3 water_traps.py --baud 9600
python3 water_traps.py --watch --interval 30
python3 water_traps.py --watch --log /tmp/water_traps.csv
python3 water_traps.py --config /path/to/water_traps.yaml
```

Do not point this tool at the same serial port already opened by another running process (for example, `ie3.py`).

## CSV Logging

`--watch` mode appends rows to the configured `log_file` and does not overwrite it on restart. The first column is `time_utc`, followed by one column per reading:

```text
time_utc,water_trap_1_temp,water_trap_2_temp
2026-05-05T18:00:00.000000+00:00,4.10,10.00
```

When `--setpoints` is used with `--watch`, setpoint columns are added:

```text
time_utc,water_trap_1_temp,water_trap_2_temp,water_trap_1_sp,water_trap_2_sp
```

Missing or invalid readings use `-999.0`, matching the convention in `omega.py`.

## Importable API

`water_traps.py` can be imported by IE3 or other scripts:

```python
from water_traps import WaterTrapReader

reader = WaterTrapReader.from_yaml("water_traps.yaml")
try:
    temps = reader.read_once()
    temps_and_setpoints = reader.read_once(include_setpoints=True)
    readback = reader.set_setpoint("water_trap_1", 10)
finally:
    reader.close()
```

`read_once()` returns CSV-ready keys such as:

```python
{
    "water_trap_1_temp": 4.1,
    "water_trap_2_temp": 10.0,
}
```

With `include_setpoints=True`, it also includes:

```python
{
    "water_trap_1_sp": 4.0,
    "water_trap_2_sp": 10.0,
}
```

`set_setpoint()` raises `ValueError` for an invalid trap reference or a setpoint outside `0-100 °C`.

## Notes

- Use one `WaterTrapReader` per serial bus.
- Trap addresses may overlap with the main IE3 Omega addresses because the water traps are on a different serial bus.
