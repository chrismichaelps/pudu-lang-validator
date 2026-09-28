---
type: module
path: src/PuduLangValidator/Rule.pudu
---

# Rule

## Purpose and Interface

`RuleBuilder[T,V]` captures a property name, display name, selector, ordered checks, optional rule sets, and cascade mode. `ruleFor` starts a builder. `must`, `mustWith`, and `mustWithContext` append predicates over the property, root, and optional context data. `withMessage`, `withMessageFrom`, `withCode`, `withSeverity`, and `withState` change the most recent check. `withName` changes display only; `overridePropertyName` changes the failure path. `when` and `unless` gate all checks in the chain; `whenCurrent` and `unlessCurrent` gate the latest check. `stopOnFirst` changes the rule cascade. `inRuleSet` tags the rule. `build` freezes it as `Rule[T]`, whose run function returns failures.

`withArgument` and `withArgumentFrom` add named message values to the latest check. A repeated name replaces its earlier value. Argument factories receive the source and property only when that check fails.

`mustWithArguments` adds a check that returns `None` on success or `Some(arguments)` on failure. These arguments are captured by the predicate and reused for the emitted message. An explicit `withArgument` or `withArgumentFrom` on that check takes precedence when names overlap.

`withSeverityFrom` and `withStateFrom` compute metadata from the source and property for the latest check. Static setters clear an earlier callback; a later callback replaces an earlier static value.

## Algorithm and edge cases

Scalar property selection uses [[Selection]] so empty bracket patterns match concrete indices without admitting a descendant-only request.

Message arguments render alongside built-in property placeholders. A custom message may use named arguments supplied by its check; arguments on another check do not leak into it. Argument names representing built-in property placeholders are reserved.

Rendering scans the chosen template once. Replacement text is appended literally and is not reparsed for more placeholders. An argument factory runs only if its name occurs in the chosen template.

The check returns its failure decision and any captured argument values together. A source selector needed for both comparison and message formatting can therefore be evaluated once per check. A false condition skips the check and all of its argument work.

Metadata callbacks run only for emitted failures, after conditions and the predicate. Each check owns its own callbacks, so later checks cannot change earlier failures.

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
- **Q:** When are dynamic message arguments computed? **A:** Only after the attached check fails. **Rationale:** successful validation avoids formatting work. **Rejected:** eager argument evaluation.
- **Q:** How can a predicate reuse a value in its failure message? **A:** Return it as a captured argument with the failure decision. **Rationale:** the compared value and rendered value must agree even for a changing selector. **Rejected:** invoking the selector again while rendering.
- **Q:** Which metadata setter wins? **A:** The latest static or computed setter on one check. **Rationale:** fluent updates have a predictable order. **Rejected:** combining conflicting severities or states.

## Referenced by

[[src/PuduLangValidator/_MOC]] · [[architecture/Validation]] · [[src/PuduLangValidator/Validator]]
