---
type: module
path: src/PuduLangValidator/Testing.pudu
---

# Testing

## Purpose and Interface

`hasErrorFor`, `hasNoErrorFor`, `hasCode`, and `onlyErrorsFor` inspect structured results without parsing messages. They complement `Std.Test` assertions and keep validator tests focused on paths and codes.

## Algorithm and edge cases

An empty result has no error for any path and vacuously only errors for no paths. `onlyErrorsFor` also requires at least one error, so an accidentally valid result cannot satisfy an expected failure assertion.

## Negative logic

No mocking framework or global test registry.

## Depth

SHALLOW — pure predicates over results.

## Grill Log

- **Q:** Return a custom assertion object? **A:** Return `Bool` for `Std.Test.that`. **Rationale:** the language already has test reporting. **Rejected:** duplicate test runner.

## Referenced by

[[src/PuduLangValidator/_MOC]]
