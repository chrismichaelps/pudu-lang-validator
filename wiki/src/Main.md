---
type: module
path: src/Main.pudu
---

# Main

## Purpose and Interface

The CLI prints package version or a concise usage message. Exit status is zero for supported requests and nonzero for unknown arguments. Validation itself is a library API; the CLI does not parse a model-specific schema.

## Algorithm and edge cases

Read arguments once, match the first command, and avoid network or filesystem effects.

## Negative logic

No reflection-driven generic object validation and no hidden remote service.

## Depth

SHALLOW — stable package entry point.

## Grill Log

- **Q:** Should the CLI accept arbitrary JSON with no schema? **A:** No. **Rationale:** validation rules need typed caller definitions. **Rejected:** weakly typed pseudo-rules.

## Referenced by

[[src/_MOC]]
