---
type: module
path: src/PuduLangValidator/Advanced.pudu
---

# Advanced

## Purpose and Interface

`custom` accepts a root-aware function that may return several failures. `customWithContext` also receives immutable root context data. `dependent` runs a second rule only when a first rule succeeds. `when` and `unless` gate an entire built rule.

## Algorithm and edge cases

Custom failures retain the caller's order and metadata; the rule name controls property selection. Dependent failure paths are retained as supplied. A dependent rule is part of its prerequisite and runs when that prerequisite succeeds, regardless of its own tag. A skipped condition reports no failures.

## Negative logic

No exception from validation failure and no implicit child traversal.

## Depth

MEDIUM — explicit control flow over already built rules.

## Grill Log

- **Q:** Should dependent rules run after one parent failure? **A:** No. **Rationale:** dependents are meaningful only when their prerequisite holds. **Rejected:** unconditional sequencing.
- **Q:** Can custom code add several failures? **A:** Yes. **Rationale:** cross-field rules may identify several properties. **Rejected:** one-failure restriction.

## Referenced by

[[src/PuduLangValidator/_MOC]]
