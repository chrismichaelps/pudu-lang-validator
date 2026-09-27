---
type: module
path: src/PuduLangValidator/Rules/Decimal.pudu
---

# Decimal

## Purpose and Interface

`precisionScale` checks total digits and digits after the decimal point on Pudu `Decimal`, with an `ignoreTrailingZeros` option. The rule is built from exact decimal text so binary floating point never changes the count.

## Algorithm and edge cases

Ignore a leading sign and a decimal point; count remaining digits. When requested, remove fractional trailing zeroes before counting. A zero still has one significant digit. `1.2300` has scale four or two according to the flag. Reversed or negative configuration bounds fail the check.

## Negative logic

No floating point conversion, rounding, or copied framework message.

## Depth

MEDIUM — decimal representation and significant digit accounting.

## Grill Log

- **Q:** Count trailing zeroes by default? **A:** Yes. **Rationale:** the literal's scale is intentional and the flag must change it explicitly. **Rejected:** implicit trimming.

## Referenced by

[[src/PuduLangValidator/Rules/_MOC]]
