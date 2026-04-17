# Configuration

IE3 uses several small configuration files. Keep public documentation focused on what the fields mean. Do not publish private credentials or access procedures.

## `ie3_config.py`

`ie3_config.py` contains instrument-specific hardware and path configuration:

- GC network URL
- ITX data path
- instrument code written into ITX files
- GSV and SSV addresses
- SSV stop port
- auxiliary pressure channel mappings
- Omega controller addresses
- LabJack digital and analog channel assignments

The GC-facing network interface on the IE3 computer must be on the same subnet as the GC.

## `setup.py`

`setup.py` contains chromatography and runtime settings:

- detector temperature and makeup settings
- auxiliary pressure setpoints
- warmup, test, and run sequences
- chromatogram duration
- cleanup duration and temperature threshold
- sample flush start and stop timing
- backflush timing
- solenoid timing
- ITX auxiliary data timing
- initial plotting limits
- flask sampling SSV override settings

Current key values:

| Setting | Value |
| --- | --- |
| Chromatogram duration | `450 s` |
| Cleanup duration | `15 min` |
| Sample flush start | `150 s` |
| Sample flush stop | `440 s` |
| Air1 SSV position | `3` |
| Air2 SSV position | `7` |
| Flask sampling timeout | `180 min` |

## `ssv_positions.yaml`

`ssv_positions.yaml` maps SSV positions to cylinders and air inlets. The UI uses it for the `SSV Reference`, `Cylinders`, and flow balancing tables.

Current public summary:

| Position | Meaning |
| ---: | --- |
| `1` | high standard |
| `3` | `air1` |
| `5` | reference |
| `7` | `air2` |
| `9` | low standard |
| even positions | stop ports |

## `omega_config.yaml`

`omega_config.yaml` stores PID values for Omega Platinum controllers. Update only the numeric PID values, and only when the controller tuning has intentionally changed.
