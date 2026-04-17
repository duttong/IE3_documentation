# UI Design

The IE3 desktop interface is a compact PyQt control panel for a scientific instrument. The design should stay calm, readable, and dense enough for live operations without hiding important status.

## Core Pattern

- light blue-gray application background
- white content cards with subtle borders
- teal primary accent
- green for ready, open, active, or running states
- yellow for editable, caution, pending, or manual-intervention states
- tables for operational values and logs
- metric tiles for short live values

Use color as status, not decoration.

## Main Tabs

Current tabs:

| Tab | Purpose |
| --- | --- |
| `Info` | Live status, GC parameters, solenoids, flask sampling, and current sequence state |
| `SSV Info` | Flow statistics, flow balancing controls, and SSV reference |
| `Cylinders` | Pressure entry, usage projections, and tank inventory |
| `Log` | Operator notes and event history |

## Layout Rules

- Keep the main window readable at the instrument display resolution.
- Use a clear header at the top of each tab.
- Group related controls into cards.
- Avoid nesting cards inside cards.
- Give hardware-control buttons stable widths so labels do not shift the layout.
- Keep tables readable and avoid hiding important columns.

## State Labels

Hardware state labels should be explicit:

- use `Sample Open` and `Sample Closed`, not only color
- use `Start Flask Sampling` and `Flask Sampling in Progress`
- show both the numeric SSV position and the configured label when available

## Operator Safety

Controls that interrupt the sequence or move hardware should confirm before acting. The UI should log operator-visible transitions such as sequence stop/restart and flask sampling state changes.

## Display Simulation

Develop UI changes with the display simulation before using instrument hardware:

```bash
python3 display.py
python3 display.py --cylinder-log logs/cylinder_pressures.csv
```
