# Data Files

IE3 writes chromatograms, engineering data, metadata, operator logs, cylinder logs, and sample-flow statistics.

## Output Layout

The production chromatogram root is configured in `ie3_config.py`. Each run writes into a UTC day directory and run-specific output set.

Important file types:

| File | Purpose |
| --- | --- |
| `*.itx` | Igor Text File chromatograms |
| `eng_*.csv` | engineering data stream |
| `meta_*.json` | run metadata |
| `ie3_*.log` | runtime log |
| `logs/cylinder_pressures.csv` | technician-entered cylinder pressure history |
| `logs/log_entries.csv` | operator event log |
| `logs/ie3_sampleflows.json` | recent SSV flow statistics |
| `logs/tank_inventory.csv` | tank inventory used by the Cylinders tab |

## ITX Chromatogram Files

Each chromatogram is written as an Igor Text File. One file contains detector channel waves for a single injection.

Every channel wave carries an `X note` line with semicolon-delimited metadata:

```text
X note chr1_01411, " 1; 0; HH:MM:SS; MM-DD-YYYY; selpos; sta; aux...; flag;"
```

| Field | Example | Description |
| --- | --- | --- |
| Format version | `1` | ITX note format version |
| Reserved | `0` | Reserved field |
| Injection time | `14:11:07` | UTC injection time |
| Injection date | `03-25-2026` | UTC injection date |
| SSV position | `3` | SSV position at sample flush stop |
| Station | `smo` | Instrument/station code |
| Auxiliary values | `1011.14 1011.26 1008.51` | Values configured in `setup.itx_aux_times` |
| Quality flag | `SKIP` or `KEEP` | Startup/restart data-quality flag |

When parsing ITX notes, split on semicolons and read the last non-empty field as the quality flag.

## Engineering CSV

Engineering CSV files are written by `save_ie3_df()` in `ie3.py`. New files include a header row on the first write.

Engineering data includes valve positions, LabJack analog and digital values, Omega values, GC status, and IE3 runtime state.

## Metadata JSON

Metadata includes the run sequence, run type, and SSV port mapping used for the run. This lets downstream users interpret SSV positions even if the public configuration later changes.

## Logs

Technician-facing logs are synced with chromatogram data:

- `logs/cylinder_pressures.csv`
- `logs/log_entries.csv`

These logs are part of the operational record and should be used during data review.
