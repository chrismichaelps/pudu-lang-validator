---
type: module
path: src/PuduLangValidator/Rules/Collection.pudu
---

# Collection

## Purpose and Interface

Adds `notEmpty`, `empty`, and `countBetween` checks for `Array[V]`. `forEach` builds a rule from a named array selector and an element predicate, reporting indexed paths. Filtering is expressed by a predicate passed to `forEachWhere`. `forEachWhereIndexed` accepts a custom indexer that names each included item in the failure path. `ruleForEach` starts a reusable `ElementBuilder` whose `must`, `withMessage`, `withCode`, `whereItems`, `withIndex`, and `stopOnFirst` operations compose multiple checks per element before `buildEach` freezes the rule.

## Algorithm and edge cases

Element selection uses [[src/PuduLangValidator/Selection]] to match explicit and empty bracket indices at path boundaries.

Count bounds are rendered into failure messages when a check fails.

`countBetween` attaches `From`, `To`, and computed `TotalCount` arguments for caller messages.

Each element is visited in ascending index order. Filtering preserves original indices. A simple element predicate yields one failure; an element builder may yield several in check order, unless stopped after the first failure. Selected indexed paths skip other elements. The element message may use `{CollectionIndex}` and `{PropertyPath}`. A custom indexer supplies the bracket content, while `{CollectionIndex}` stays the numeric source position. An empty array produces no element failures.

## Negative logic

No enumerable reflection or mutation during iteration.

## Depth

MEDIUM — array checks and indexed failure construction.

## Grill Log

- **Q:** Renumber filtered elements? **A:** Keep source indices. **Rationale:** paths should locate the actual item. **Rejected:** dense filtered numbering.
- **Q:** Implicit child validation? **A:** Require an explicit element predicate or child rule. **Rationale:** type and execution order stay visible. **Rejected:** reflection.

## Referenced by

[[src/PuduLangValidator/Rules/_MOC]]
