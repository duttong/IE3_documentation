# Runtime Modes

## Command-Line Modes

```bash
ie3.py                 # normal operation
ie3.py --status        # check status without starting a full run
ie3.py --prerun        # apply prerun settings
ie3.py --safe          # apply safe settings
ie3.py --warmup        # run warmup sequence
ie3.py --test          # run test sequence
ie3.py --cleanup       # cleanup before run sequence
ie3.py --idle          # idle mode
ie3.py --stop          # ask a running process to stop gracefully
ie3.py --restart-at-end # request controlled restart after the current sequence
ie3.py --debug         # DEBUG logging for the session
```

## Continuous Operation

Normal operation uses the run sequence from `setup.py`. Each injection lasts `450` seconds in the current configuration. A full run sequence is about `10.1` hours.

At the end of a sequence, IE3:

1. saves flow statistics
2. stops timers
3. writes the final chromatogram
4. gzips CSV and ITX files in the completed run directory
5. renews GC ownership before starting the next sequence

A sequence that crosses UTC midnight finishes in its current output set. The next sequence starts a fresh output set, including a new UTC day directory if needed.

## Controlled Restart

`ie3.py --restart-at-end` sends a restart request to the running process. The process finishes the current sequence and exits with the dedicated restart code. `run_ie3_loop.sh` sees that code and starts a fresh `ie3.py`.

This supports code or configuration updates without cutting off a sequence in the middle.

## SKIP and KEEP

The first injection after a fresh process start or controlled restart is flagged `SKIP`. Later injections are flagged `KEEP`.

The flag appears in each ITX wave note and should be respected by downstream processing.

Day-boundary rollover inside continuous operation does not set `SKIP`.
