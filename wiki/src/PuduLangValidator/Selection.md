---
type: module
path: src/PuduLangValidator/Selection.pudu
---

# Selection

## Purpose and Interface

Matches explicit property paths at dot and bracket boundaries. `covers` asks whether a requested path selects a concrete rule path. `overlaps` also admits a concrete ancestor needed to reach a selected child. `relative` removes a concrete child prefix from a request and keeps remaining empty-index patterns for deeper collections.

## Algorithm and edge cases

The request may contain `[]` in place of one bracketed index. A wildcard consumes exactly one nonempty bracket value, including caller-defined index keys. The matcher compares Unicode scalar values in order and stops at the first mismatch. It never treats a plain text prefix as a path boundary: `item` does not select `items`. Multiple wildcards are handled independently, so nested arrays remain addressable. An empty relative path selects the whole child.

## Negative logic

No regular-expression compilation, reflection, or implicit property-name extraction.

## Depth

DEEP — one path relation keeps root, child, element, and async selection consistent.

## Grill Log

- **Q:** Should `[]` match a missing index? **A:** No; consume one nonempty bracket value. **Rationale:** paths must locate a real element. **Rejected:** optional index segments.
- **Q:** Is any string prefix an ancestor? **A:** No; require dot or bracket boundaries. **Rationale:** unrelated property names must remain separate. **Rejected:** raw prefix matching.
- **Q:** Should a custom index key match `[]`? **A:** Yes; match any nonempty bracket content. **Rationale:** index renderers already define the path key. **Rejected:** numeric-only matching.

## Referenced by

[[src/PuduLangValidator/_MOC]] · [[src/PuduLangValidator/Validator]] · [[src/PuduLangValidator/Async]]
