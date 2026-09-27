---
type: module
path: src/PuduLangValidator/Async.pudu
---

# Async

## Purpose and Interface

`AsyncRule[T]` holds an asynchronous root check. `mustAsync` checks a typed selected property. `forEachAsync` and `forEachWhereAsync` validate array elements with optional asynchronous filtering. `fromSync` lifts an ordinary built rule. `inRuleSet`, `withSeverity`, `withState`, and `when` decorate a built async rule. `AsyncValidator[T]` combines these rules; `validateAsync` uses default selection and `validateAsyncWith` applies explicit property and rule-set options. The synchronous validator does not accept async rules.

## Algorithm and edge cases

Selection runs once for an async rule. A false predicate emits one failure with the supplied path, message, and code. Array checks and filters are awaited in source order and retain original indices; unselected indices are skipped. Rules are awaited sequentially so failure order matches declaration order. `stopOnFirst` ends after the first failing rule. A rule's awaited work is cold until validation begins.

## Negative logic

No automatic blocking execution of an async predicate from `validate`; no background task hidden from the caller.

## Depth

MEDIUM — explicit task boundary with deterministic ordering.

## Grill Log

- **Q:** Run async checks from the synchronous API? **A:** No. **Rationale:** a caller must choose an async boundary. **Rejected:** implicit blocking.
- **Q:** Run checks concurrently? **A:** Await in rule order. **Rationale:** failures and side effects remain predictable. **Rejected:** completion-order failures.

## Referenced by

[[src/PuduLangValidator/_MOC]]
