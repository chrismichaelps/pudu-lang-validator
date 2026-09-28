---
type: module
path: src/PuduLangValidator/Result.pudu
---

# Result

## Purpose and Interface

Owns `Severity`, `Failure`, and `ValidationResult`. A failure records path, message, code, severity, and custom state as text. `fromFailures` derives validity from an empty failure array; `messages` joins messages in encounter order; `forProperty` filters by exact path; `toDictionary` groups messages by path; `toResult` turns a valid result into a typed success and an invalid result into its failure array.

## Algorithm and edge cases

No failure exists for a successful check. The empty result is valid and renders to empty text. The module never sorts failures or guesses a display name from a path.

## Negative logic

No exception, localization registry, or mutable error bag. A string state is deliberately portable in this first package version.

## Depth

MEDIUM — one stable, structured result contract with straightforward projections.

## Grill Log

- **Q:** Should validity be stored independently? **A:** `fromFailures` derives it from errors; direct record construction must maintain that convention. **Rationale:** Pudu records keep projections simple. **Rejected:** separate mutable error bag.
- **Q:** Should errors be grouped by property? **A:** Preserve rule order; grouping can be a later projection. **Rationale:** order conveys which check failed first. **Rejected:** map storage.

## Referenced by

[[src/PuduLangValidator/_MOC]] · [[src/PuduLangValidator/Rule]] · [[src/PuduLangValidator/Validator]]
