---
type: module
path: src/PuduLangValidator/Testing.pudu
---

# Testing

## Purpose and Interface

`hasErrorFor`, `hasNoErrorFor`, `hasCode`, and `onlyErrorsFor` inspect structured results without parsing messages. `forProperty` starts a `FailureQuery` over an exact path. `withMessage`, `withCode`, `withSeverity`, and `withState` retain matching failures; `withoutMessage`, `withoutCode`, `withoutSeverity`, and `withoutState` retain their inverse. `hasAny`, `hasNone`, and `only` turn a query into an assertion for `Std.Test.that`.

`testValidate` and `testValidateAsync` run real validators and return ordinary structured results. Their option-bearing variants preserve property and rule-set selection.

## Algorithm and edge cases

An empty result has no error for any path. `onlyErrorsFor` and query `only` require at least one matching failure, so an accidentally valid result cannot satisfy an expected failure assertion.

Queries retain the complete original failure array and narrow a separate match array in encounter order. `only` succeeds when every original failure survives all filters. Repeated failures at the same path remain distinct. Inverse filters compare one failure field at a time; they do not rewrite or remove failures from the validation result.

## Negative logic

No mocking framework or global test registry.

## Depth

SHALLOW — pure predicates over results.

## Grill Log

- **Q:** Return a custom assertion object? **A:** Use a small query value and return `Bool` for `Std.Test.that`. **Rationale:** chaining metadata filters needs a stable selection while the language already has test reporting. **Rejected:** duplicate test runner.
- **Q:** Does `only` accept no matches? **A:** No. **Rationale:** an expected failure assertion must observe at least one failure. **Rejected:** vacuous success.
- **Q:** Do filters use exact matching? **A:** Yes, including full property paths. **Rationale:** test outcomes stay deterministic for nested collections. **Rejected:** implicit wildcard matching.

## Referenced by

[[src/PuduLangValidator/_MOC]]
