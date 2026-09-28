---
type: module
path: src/PuduLangValidator/Rules/Decimal.pudu
---

# Decimal

## Purpose and Interface

`precisionScale` checks total digits and digits after the decimal point on Pudu `Decimal`, with an `ignoreTrailingZeros` option. The rule is built from exact decimal text so binary floating point never changes the count.

## Algorithm and edge cases

Precision checks attach expected precision and scale plus the measured digit count and actual scale as message arguments. The check captures the measured shape once on failure, so the predicate and rendered values agree.

Ignore a leading sign and a decimal point; count remaining digits. When requested, remove fractional trailing zeroes before counting. A zero still has one significant digit. `1.2300` has scale four or two according to the flag. The whole-number part may contain no more than `precision - scale` significant digits, even when the value has fewer than `scale` fractional digits. Reversed or negative configuration bounds fail the check.

## Negative logic

No floating point conversion, rounding, or copied framework message.

## Depth

MEDIUM — decimal representation and significant digit accounting.

## Grill Log

- **Q:** Count trailing zeroes by default? **A:** Yes. **Rationale:** the literal's scale is intentional and the flag must change it explicitly. **Rejected:** implicit trimming.
- **Q:** Can unused fractional places increase the whole-number budget? **A:** No. **Rationale:** the declared scale reserves those positions consistently. **Rejected:** accepting values solely because their observed total digits fit precision.

## Referenced by

[[src/PuduLangValidator/Rules/_MOC]]
