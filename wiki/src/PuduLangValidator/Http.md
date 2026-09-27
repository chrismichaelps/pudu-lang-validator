---
type: module
path: src/PuduLangValidator/Http.pudu
---

# Http

## Purpose and Interface

`toReport` converts a validator result to `Std.Validate.Report`. `problem` maps it through `Std.App.Problem`; `respond` returns an HTTP 422 response for Pudu web handlers.

## Algorithm and edge cases

Each failure becomes one `Std.Validate.Failure`, keeping property path and message. A successful result produces an empty report, but callers should invoke `respond` only when `isValid` is false. Severity and custom state are not serialized to the public HTTP response.

## Negative logic

No framework middleware, automatic form binding, or echo of attempted values.

## Depth

SHALLOW — one explicit standard-library adapter.

## Grill Log

- **Q:** Invent a second HTTP error format? **A:** Use `Std.App.Problem`. **Rationale:** Pudu applications already have one problem contract. **Rejected:** package-specific JSON schema.
- **Q:** Expose custom state in the response? **A:** No. **Rationale:** applications may store private context there. **Rejected:** serializing internal metadata.

## Referenced by

[[src/PuduLangValidator/_MOC]]
