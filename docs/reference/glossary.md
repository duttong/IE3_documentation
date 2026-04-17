# Glossary

| Term | Meaning |
| --- | --- |
| IE3 | Continuous atmospheric GC measurement system documented here |
| CATS | [Chromatograph for Atmospheric Trace Species](https://gml.noaa.gov/hats/insitu/cats/), the in-situ halocarbons instrument system IE3 is replacing |
| GC | Agilent 8890 gas chromatograph |
| GSV | Gas Sample Valve, a two-position Valco valve used per channel |
| SSV | Stream Selection Valve, the multi-position Valco valve that selects tanks and air inlets |
| Stop port | An SSV port that blocks or stops sample flow; current even-numbered positions are stop ports |
| ITX | Igor Text File chromatogram format |
| Engineering CSV | CSV stream of instrument status, hardware readings, and runtime values |
| Omega | Omega Platinum temperature controller |
| LabJack | LabJack T7 I/O device used for digital outputs and analog readings |
| Nafion dryer | Dryer that uses dry N2 counterflow to remove water through a Nafion membrane |
| Magnesium perchlorate dryer | Final sample drying trap upstream of the GC |
| Pump board | Pump and back-pressure-regulator assembly that pulls ambient air from tower lines |
| PCB | Printed circuit board that conditions sensor signals and switches solenoid/fan power |
| Flask sampling | Temporary mode that redirects `air1` sequence positions to `air2` |
| SKIP | ITX quality flag for the first injection after software start or controlled restart |
| KEEP | ITX quality flag for normal usable injections |
| Controlled restart | Restart requested with `ie3.py --restart-at-end`, after the current sequence finishes |
