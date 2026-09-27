---
type: module
path: src/PuduLangValidator/Rules/Ordering.pudu
---

# Ordering

## Purpose and Interface

Generic `greaterThan`, `greaterThanOrEqualTo`, `lessThan`, `lessThanOrEqualTo`, `inclusiveBetween`, and `exclusiveBetween` checks accept any property type implementing Pudu `Ord`. Fixed and cross-property comparisons remain explicit through a bound or root-aware `Rule.mustWith`.

## Algorithm and edge cases

Use `Ord.before` without subtraction, so large values cannot overflow a difference. Inclusive bounds admit endpoints; exclusive bounds reject them. A reversed interval simply matches no values.

## Negative logic

No implicit numeric conversion or culture-based string ordering.

## Depth

SHALLOW — reusable typed order checks.

## Grill Log

- **Q:** Use `<` on unbounded generic types? **A:** Call the declared `Ord` relation. **Rationale:** it is the language's explicit ordering contract. **Rejected:** coercing to text or numbers.

## Referenced by

[[src/PuduLangValidator/Rules/_MOC]]
