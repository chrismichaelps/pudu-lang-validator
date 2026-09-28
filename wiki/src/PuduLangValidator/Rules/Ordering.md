---
type: module
path: src/PuduLangValidator/Rules/Ordering.pudu
---

# Ordering

## Purpose and Interface

Generic `greaterThan`, `greaterThanOrEqualTo`, `lessThan`, `lessThanOrEqualTo`, `inclusiveBetween`, and `exclusiveBetween` checks accept any property type implementing Pudu `Ord`. `greaterThanProperty`, `greaterThanOrEqualToProperty`, `lessThanProperty`, and `lessThanOrEqualToProperty` read a typed comparison property from the same source.

## Algorithm and edge cases

Single bounds attach `ComparisonValue`; intervals attach `From` and `To` for caller message templates. Cross-property bounds also attach the caller-supplied `ComparisonProperty` path. The failed predicate captures the selected bound for the message, so it is read once per check.

Use `Ord.before` without subtraction, so large values cannot overflow a difference. Inclusive bounds admit endpoints; exclusive bounds reject them. A reversed interval simply matches no values.

## Negative logic

No implicit numeric conversion or culture-based string ordering.

## Depth

SHALLOW — reusable typed order checks.

## Grill Log

- **Q:** Use `<` on unbounded generic types? **A:** Call the declared `Ord` relation. **Rationale:** it is the language's explicit ordering contract. **Rejected:** coercing to text or numbers.
- **Q:** Infer the bound's path? **A:** Require an explicit path alongside its typed selector. **Rationale:** a function value has no recoverable source-field name. **Rejected:** runtime reflection.

## Referenced by

[[src/PuduLangValidator/Rules/_MOC]]
