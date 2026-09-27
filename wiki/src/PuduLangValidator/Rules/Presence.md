---
type: module
path: src/PuduLangValidator/Rules/Presence.pudu
---

# Presence

## Purpose and Interface

`notNull` and `isNull` constrain `Option[V]` fields. `notEmpty` constrains optional text, rejecting `None` and whitespace-only `Some`.

## Algorithm and edge cases

Only the `Some`/`None` tag is examined for null checks. Absence does not pass `notEmpty`.

## Negative logic

No implicit null in a non-optional Pudu value and no default-value coercion.

## Depth

SHALLOW — Pudu `Option` makes absence explicit.

## Grill Log

- **Q:** Is an empty string null? **A:** No. **Rationale:** absence and empty content are different source states. **Rejected:** conflation.

## Referenced by

[[src/PuduLangValidator/Rules/_MOC]]
