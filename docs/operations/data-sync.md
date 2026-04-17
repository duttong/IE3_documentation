# Data Sync

IE3 data sync packages local chromatogram run directories and uploads them to the configured remote incoming area by SFTP.

## What Sync Sends

The sync process sends:

- one tarball per selected chromatogram run directory
- `logs/cylinder_pressures.csv`
- `logs/log_entries.csv`

Run directories with two or fewer `.itx` files are skipped as trivial during normal sync.

Remote files are not deleted by the sync script.

## Common Commands

Default daily sync:

```bash
sync2boulder.py
```

Sync the last three days:

```bash
sync2boulder.py --last-days 3
```

Resend one specific run directory:

```bash
sync2boulder.py --dir-name 20260324
```

Verbose output:

```bash
sync2boulder.py -v
```

## Scheduled Sync

The current cron example runs once per day at `14:20 UTC`, which corresponds to `07:20 MST`.

The wrapper script activates the IE3 conda environment and runs `sync2boulder.py --last-days 1`.

## Safety Notes

Do not publish SSH keys, private host aliases, or remote account details in public docs. Keep public examples generic unless the value is already intentionally public operational documentation.
