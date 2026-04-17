# Glossary

| Term | Meaning |
| --- | --- |
| IE3 | Continuous atmospheric GC measurement system documented here |
| GC | Agilent 8890 gas chromatograph |
| GSV | Gas Sample Valve, a two-position Valco valve used per channel |
| SSV | Stream Selection Valve, the multi-position Valco valve that selects tanks and air inlets |
| Stop port | An SSV port that blocks or stops sample flow; current even-numbered positions are stop ports |
| ITX | Igor Text File chromatogram format |
| Engineering CSV | CSV stream of instrument status, hardware readings, and runtime values |
| Omega | Omega Platinum temperature controller |
| LabJack | LabJack T7 I/O device used for digital outputs and analog readings |
| Flask sampling | Temporary mode that redirects `air1` sequence positions to `air2` |
| SKIP | ITX quality flag for the first injection after software start or controlled restart |
| KEEP | ITX quality flag for normal usable injections |
| Controlled restart | Restart requested with `ie3.py --restart-at-end`, after the current sequence finishes |
