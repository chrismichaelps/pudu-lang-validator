---
type: module
path: src/PuduLangValidator/Rules/Case.pudu
---

# Case

## Purpose and Interface

`forCase` validates a selected case of a Pudu sum type with a case-specific validator. The selector answers `Option[V]`; `None` means the source is another variant. Several case rules may be added to the same validator in declaration order.

## Algorithm and edge cases

Only a matching case runs. Child failures retain their fields, optionally prefixed with a supplied path. Rule-set selection is inherited. The type checker ensures dispatch covers declared Pudu variants.

## Negative logic

No reflection, casts, or fallback validator for unknown subclasses.

## Depth

MEDIUM — typed dispatch across sum-type cases.

## Grill Log

- **Q:** Infer a variant at runtime? **A:** Require an explicit selector. **Rationale:** Pudu sums are statically checked and exhaustive. **Rejected:** string tags and casts.

## Referenced by

[[src/PuduLangValidator/Rules/_MOC]]
