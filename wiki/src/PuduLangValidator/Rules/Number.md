---
type: module
path: src/PuduLangValidator/Rules/Number.pudu
---

# Number

## Purpose and Interface

Adds `greaterThan`, `greaterThanOrEqualTo`, `lessThan`, `lessThanOrEqualTo`, `inclusiveBetween`, `exclusiveBetween`, `equal`, `notEqual`, `notEmpty`, `empty`, and `isInEnum` to integer property rules. Cross-property comparison remains available through `Rule.mustWith`. `isInEnum` takes an explicit array of valid discriminants because Pudu sum types cannot hold an invalid variant.

## Algorithm and edge cases

Numeric bounds are rendered into failure messages when a check fails.

Bounds are compared with normal Pudu integer operators. A reversed interval simply fails every value. No arithmetic difference is computed, so extreme integer bounds cannot overflow.

## Negative logic

No implicit numeric conversions or culture-sensitive parsing.

## Depth

SHALLOW — named predicates with stable codes.

## Grill Log

- **Q:** Normalize reversed intervals? **A:** Preserve caller order. **Rationale:** silently swapping them hides a configuration mistake. **Rejected:** automatic bound swap.

## Referenced by

[[src/PuduLangValidator/Rules/_MOC]]
