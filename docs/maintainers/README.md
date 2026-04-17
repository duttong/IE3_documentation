# Maintainer Guide

This page is for future people responsible for keeping IE3 running, understandable, and documented.

## Repositories

| Repository | Visibility | Purpose |
| --- | --- | --- |
| `IE3_acquisition` | private | source code, instrument configuration, launchers, and operational scripts |
| `IE3_documentation` | public | public operator, software, operations, and maintainer documentation |

The private source repository is the source of truth for exact runtime behavior. This public repository should explain behavior at a level that helps operators and developers without exposing private access details.

## When to Update Docs

Update documentation whenever a change affects:

- startup, shutdown, restart, or sync procedures
- GUI labels, tabs, or operator workflow
- run sequence behavior
- SSV port assignments
- cylinder or operator log behavior
- output file names, formats, metadata, or data-quality flags
- configuration fields or default values
- troubleshooting guidance

## Handoff Checklist

For a maintainer transition, review:

- how to start and stop IE3
- how controlled restart works
- where output files are written
- how daily sync works
- how to read `SKIP` and `KEEP` in ITX notes
- how SSV positions map to standards and air inlets
- how cylinder pressure logging and tank inventory interact
- what should and should not be published in public docs

## Documentation Workflow

1. Make behavior changes in the private source repository.
2. Confirm the behavior from source or instrument testing.
3. Update the public docs in this repository.
4. Check that public docs do not contain credentials or private access details.
5. Keep old top-level entry points working when reorganizing pages.

## Ownership Notes

Prefer clear public explanations over copying large source snippets. Small command examples and field tables are useful. Long private implementation details are better left in code comments or private developer notes unless they are necessary for future operation.
