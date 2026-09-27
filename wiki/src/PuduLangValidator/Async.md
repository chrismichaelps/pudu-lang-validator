---
type: module
path: src/PuduLangValidator/Async.pudu
---

# Async

## Purpose and Interface

`AsyncRule[T]` holds an asynchronous root check. `mustAsync` checks a typed selected property. `forEachAsync` and `forEachWhereAsync` validate array elements with optional asynchronous filtering. `fromSync` lifts an ordinary built rule. `inRuleSet`, `withSeverity`, `withState`, and `when` decorate a built async rule. `AsyncValidator[T]` combines these rules; `validateAsync` uses default selection and `validateAsyncWith` applies explicit property and rule-set options. The synchronous validator does not accept async rules.

`mustAsyncWithContext` receives immutable root data with its source and property. `validateAsyncWithData` supplies that data to async rules and lifted synchronous rules. Existing entry points pass an empty map.
`forEachWhereAsyncWithContext` passes the same data to both its filter and its element predicate; the simpler array functions adapt to it.

`setValidatorAsync`, `setOptionalValidatorAsync`, and `forEachValidatorAsync` compose nested asynchronous validators, preserving context, selected rule sets, selected property paths, and source indices. `whenAsync` awaits a context-aware condition before running a rule. `customAsync` lets an awaited callback return zero or more structured failures.

## Algorithm and edge cases

Async root and child rules use [[Selection]] for the same path relations as synchronous validation.

Selection runs once for an async rule. A false predicate emits one failure with the supplied path, message, and code. Array checks and filters are awaited in source order and retain original indices; unselected indices are skipped. Rules are awaited sequentially so failure order matches declaration order. `stopOnFirst` ends after the first failing rule. A rule's awaited work is cold until validation begins.

A scalar async rule skips itself when only a descendant path is selected. Decorators preserve the context data unchanged; lifted rules receive the same data as native async rules.

Child rules advertise their children's rule sets to the parent selection. Nested property names become child-relative before validation and are prefixed once afterward. Optional absence yields no child failures; a separate presence rule can reject it. Collection children retain original indices, and only requested indices are awaited. A false asynchronous condition performs no validation work.

Selecting an ancestor of a dotted collection path reaches its elements, while an unrelated prefix does not.

## Negative logic

No automatic blocking execution of an async predicate from `validate`; no background task hidden from the caller.

## Depth

MEDIUM — explicit task boundary with deterministic ordering.

## Grill Log

- **Q:** Run async checks from the synchronous API? **A:** No. **Rationale:** a caller must choose an async boundary. **Rejected:** implicit blocking.
- **Q:** Run checks concurrently? **A:** Await in rule order. **Rationale:** failures and side effects remain predictable. **Rejected:** completion-order failures.
- **Q:** Should context be mutable during async validation? **A:** Pass an immutable map through the run boundary. **Rationale:** nested and lifted rules observe one consistent request context. **Rejected:** per-rule global state.
- **Q:** How should nested async work retain order? **A:** Await each child and array element sequentially. **Rationale:** failure order remains tied to source order. **Rejected:** merge by completion time.

## Referenced by

[[src/PuduLangValidator/_MOC]]
