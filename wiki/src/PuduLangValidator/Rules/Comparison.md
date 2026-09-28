---
type: module
path: src/PuduLangValidator/Rules/Comparison.pudu
---

# Comparison

## Purpose and Interface

`equal`, `notEqual`, and `oneOf` work on equality-capable properties. `equalToProperty` and `notEqualToProperty` accept a typed selector for another value on the same source and its explicit path, so a caller can compose cross-property rules without a hand-written predicate.

## Algorithm and edge cases

Equality checks attach `ComparisonValue` for caller message templates. Cross-property checks also attach `ComparisonProperty`, and evaluate the comparison selector once against the source being validated. The failed predicate captures that value for the message.

Comparison uses Pudu `Eq` and exact values, with no culture or implicit conversion. `oneOf` checks the provided array in order; an empty candidate array never matches.

## Negative logic

No string collation or comparer object is guessed.

## Depth

SHALLOW — generic typed equality.

## Grill Log

- **Q:** Add a configurable comparer now? **A:** Use `Rule.mustWith` for caller-defined equivalence. **Rationale:** a caller's predicate states its semantics clearly. **Rejected:** hidden global comparer.
- **Q:** Infer the comparison path from a selector? **A:** Require a path argument. **Rationale:** Pudu functions do not carry source-field names. **Rejected:** parsing source syntax or naming a closure.

## Referenced by

[[src/PuduLangValidator/Rules/_MOC]]
