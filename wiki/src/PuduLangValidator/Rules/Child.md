---
type: module
path: src/PuduLangValidator/Rules/Child.pudu
---

# Child

## Purpose and Interface

`setValidator` applies a child validator to a nested value. `setOptionalValidator` skips `None`. `forEachValidator` applies a child validator to each array element.

## Algorithm and edge cases

Child path translation uses [[src/PuduLangValidator/Selection]] to preserve wildcard indices across nested arrays.

Failures from children are copied in child order with a prefix: `address.postcode` or `orders[2].total`. Empty child property names resolve to the prefix itself. Collection indices are original indices. Child validators inherit selected rule sets, default-rule inclusion, and the property selection with their own prefix removed. Collection children at unselected indices are skipped.

## Negative logic

No reflection, inheritance dispatch, or implicit child traversal.

## Depth

MEDIUM — preserves nested failure locations across typed validator boundaries.

## Grill Log

- **Q:** Validate absent optional child? **A:** Skip it. **Rationale:** absence is governed by an explicit presence rule. **Rejected:** fabricating child errors.
- **Q:** Should a child keep its original failure path? **A:** Prefix it. **Rationale:** callers need a location in the root model. **Rejected:** losing context.

## Referenced by

[[src/PuduLangValidator/Rules/_MOC]]
