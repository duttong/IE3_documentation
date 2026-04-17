# IE3 Documentation

Public documentation for IE3, a continuous atmospheric GC measurement system.

This repository is intended for technicians, software developers, and future project maintainers. The private source repository remains the source of truth for code, credentials, host-specific settings, and deployment-only details. Public documentation here should explain how the system is used, maintained, and understood without exposing private access information.

**IE3** is an acronym for the **In-situ Electron Capture Detector 3rd-generation** instrument. The IE3 systems are replacing the old [CATS](https://gml.noaa.gov/hats/insitu/cats/) instruments. American Samoa (SMO) is the first NOAA observatory to install an IE3 instrument. 

## Start Here

- [Technician Guide](docs/technicians/README.md): normal operation, daily checks, flow balancing, logging, and common recovery steps.
- [Hardware Guide](docs/hardware/README.md): instrument hardware overview, gas path, electronics, PCB, valves, and slide photos.
- [Software Guide](docs/software/README.md): software layout, runtime behavior, configuration, data files, and development notes.
- [Operations Guide](docs/operations/README.md): startup, shutdown, controlled restart, data sync, and routine operating schedule.
- [Maintainer Guide](docs/maintainers/README.md): handoff notes, documentation upkeep, and project stewardship.
- [Reference](docs/reference/glossary.md): shared terms and project vocabulary.

## Documentation Map

| Audience | Use these pages |
| --- | --- |
| Technicians | [Hardware guide](docs/hardware/README.md), [daily checks](docs/technicians/daily-checks.md), [power down and power up](docs/technicians/power-cycle.md), [flow balancing](docs/technicians/flow-balancing.md), [cylinder pressures](docs/technicians/cylinder-pressures.md), [operator log](docs/technicians/operator-log.md), [troubleshooting](docs/technicians/troubleshooting.md) |
| Software developers | [Architecture](docs/software/architecture.md), [runtime modes](docs/software/runtime.md), [configuration](docs/software/configuration.md), [data files](docs/software/data-files.md), [UI design](docs/software/ui-design.md), [development workflow](docs/software/development.md) |
| Maintainers | [Maintainer guide](docs/maintainers/README.md), [hardware guide](docs/hardware/README.md), [operations](docs/operations/README.md), [startup and shutdown](docs/operations/startup-shutdown.md), [data sync](docs/operations/data-sync.md), [documentation boundaries](docs/reference/public-docs-boundaries.md) |

## Source Repositories

- Private source code: `IE3_acquisition`
- Public documentation: `IE3_documentation`

When adding public docs, avoid publishing:

- passwords, private keys, usernames tied to remote access, or internal-only hostnames beyond generic examples
- unpublished calibration values or analysis results that are not already meant for public release
- exact operational access procedures that would let someone connect to NOAA systems
- private issue history, personal notes, or machine-specific paths unless they are necessary public operational context

## Keeping Docs Current

Update these docs when the software changes any user-visible behavior, output file format, configuration field, operating procedure, or troubleshooting recommendation. Prefer small, topic-focused pages over one large guide.
