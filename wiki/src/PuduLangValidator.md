---
type: module
path: src/PuduLangValidator.pudu
---

# PuduLangValidator

## Purpose and Interface

The package root identifies the installed version and gives callers a simple entry point. Rule construction remains in the named modules, where the type of each operation is visible.

## Algorithm and edge cases

`version` returns the manifest version as text. It performs no initialization.

## Negative logic

No hidden global registry, mutable default rules, or implicit import re-exports.

## Depth

SHALLOW — package identity.

## Grill Log

- **Q:** Re-export every rule from the root? **A:** Keep explicit module imports. **Rationale:** ambiguous names such as `notEmpty` are resolved by module. **Rejected:** an untyped umbrella surface.

## Referenced by

[[src/PuduLangValidator/_MOC]]
