---
type: module
path: src/PuduLangValidator/Validator.pudu
---

# Validator

## Purpose and Interface

`Validator[T]` stores built rules, a preflight function, and validator cascade. `create`, `add`, and `include` construct it. `withPreValidation` supplies an early gate that can return a complete failure list before ordinary rules run. `validate` runs default rules. `validateWith` receives `Options` containing selected property names, rule sets, and whether default rules run. `validateWithData` also passes immutable root context data to each rule. `accepts` exposes the same selection predicate for asynchronous rules. `stopOnFirst` stops after the first rule with failures.

## Algorithm and edge cases

Rule reachability uses [[Selection]] for exact paths, ancestors, descendants, and empty bracket index patterns.

Ancestor paths are admitted for child traversal. Ordinary property rules skip when the selected path names only a descendant.

Run preflight first; `Some(failures)` returns immediately, including when the array is empty. `None` continues. Then run rules in insertion order. Default validation runs only untagged rules. A named-rule-set selection runs tagged rules and may also include untagged rules explicitly. A wildcard includes every named set. Child container rules advertise their children's tags so they run when a selected child set is present. Property selection admits exact names, descendants at a dot or bracket boundary, and ancestors needed to reach selected children. An empty selection array means all properties. Inclusion appends rules and preserves their own tags. A validator with no rules is valid.

## Negative logic

No implicit dependency container, global registry, or exception-based validation failure.

## Depth

MEDIUM — one execution path over type-erased property rules.

## Grill Log

- **Q:** Is inclusion an execution wrapper? **A:** Append the included rules. **Rationale:** ordering and selection remain visible. **Rejected:** nested validator state.
- **Q:** How does a selected parent property reach children? **A:** Match exact path or segment prefix. **Rationale:** collection and nested paths remain addressable. **Rejected:** raw string prefix without delimiter.

## Referenced by

[[src/PuduLangValidator/_MOC]] · [[architecture/Validation]]
