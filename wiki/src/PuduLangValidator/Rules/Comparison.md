---
type: module
path: src/PuduLangValidator/Rules/Comparison.pudu
---

# Comparison

## Purpose and Interface

`equal`, `notEqual`, and `oneOf` work on equality-capable properties. Cross-property comparison uses a root-aware selector or `Rule.mustWith`.

## Algorithm and edge cases

Equality checks attach `ComparisonValue` for caller message templates.

Comparison uses Pudu `Eq` and exact values, with no culture or implicit conversion. `oneOf` checks the provided array in order; an empty candidate array never matches.

## Negative logic

No string collation or comparer object is guessed.

## Depth

SHALLOW — generic typed equality.

## Grill Log

- **Q:** Add a configurable comparer now? **A:** Use `Rule.mustWith` for caller-defined equivalence. **Rationale:** a caller's predicate states its semantics clearly. **Rejected:** hidden global comparer.

## Referenced by

[[src/PuduLangValidator/Rules/_MOC]]
