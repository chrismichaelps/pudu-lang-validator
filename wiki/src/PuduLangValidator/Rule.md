---
type: module
path: src/PuduLangValidator/Rule.pudu
---

# Rule

## Purpose and Interface

`RuleBuilder[T,V]` captures a property name, display name, selector, ordered checks, optional rule sets, and cascade mode. `ruleFor` starts a builder. `must`, `mustWith`, and `mustWithContext` append predicates over the property, root, and optional context data. `withMessage`, `withMessageFrom`, `withCode`, `withSeverity`, and `withState` change the most recent check. `withName` changes display only; `overridePropertyName` changes the failure path. `when` and `unless` gate all checks in the chain; `whenCurrent` and `unlessCurrent` gate the latest check. `stopOnFirst` changes the rule cascade. `inRuleSet` tags the rule. `build` freezes it as `Rule[T]`, whose run function returns failures.

## Algorithm and edge cases

Scalar property rules skip when selection targets only one of their descendants; child rules can still traverse to that path.

Selection runs once per rule. Checks run in declaration order; false predicates emit one failure. Conditions compose with existing conditions by conjunction. `stopOnFirst` stops after the first emitted failure. Metadata calls on an empty builder are no-ops; this keeps constructors total. The failure path is explicit, not inferred from the selector. Message templates replace `{PropertyName}`, `{PropertyPath}`, and `{PropertyValue}` after a check fails. A message factory runs only for a failed check, allowing a caller to select text by locale or source data. A built rule receives selected rule sets, default-rule inclusion, selected property paths, and immutable root context data so child and collection rules can inherit them.

## Negative logic

No reflection, selector inspection, automatic name extraction, or global cascade state.

## Depth

DEEP — carries the typed property boundary, ordered check semantics, and metadata update law.

## Grill Log

- **Q:** Can every check have its own property type? **A:** Keep checks of one builder homogeneous and erase V only at build. **Rationale:** avoids dynamic casts. **Rejected:** boxed values.
- **Q:** Should `when` affect all earlier checks? **A:** Yes; `whenCurrent` narrows it. **Rationale:** the two scopes must be explicit and testable. **Rejected:** silently treating `when` as current-only.
- **Q:** If no check exists, does metadata error? **A:** Return the same builder. **Rationale:** preserves a total functional API. **Rejected:** panic.

## Referenced by

[[src/PuduLangValidator/_MOC]] · [[architecture/Validation]] · [[src/PuduLangValidator/Validator]]
