# Public Documentation Boundaries

The source repository is private, but this documentation repository is public. Use it to explain the system without publishing sensitive operational access details.

## Good Public Content

- user-facing operating procedures
- general hardware roles and software architecture
- file format descriptions
- data-quality flags and parsing guidance
- troubleshooting steps for local operators
- configuration field meanings
- non-sensitive examples

## Do Not Publish

- private keys, passwords, tokens, or credentials
- internal-only remote login details
- unpublished data products or analysis results
- personal notes or private issue history
- machine-specific developer paths
- instructions that would let an unauthorized person access NOAA systems

## Review Checklist

Before committing public documentation:

- Links are relative and work from GitHub.
- Screenshots do not show credentials, private hostnames, or unrelated personal information.
- Command examples do not include secrets.
- Source-derived behavior is current with the private code.
- Technician pages and software pages agree on user-visible behavior.

For IE3 login credentials, public docs may direct technicians to contact Geoff Dutton at `geoff.dutton@noaa.gov`, but should not include the credentials themselves.
