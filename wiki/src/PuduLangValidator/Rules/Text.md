---
type: module
path: src/PuduLangValidator/Rules/Text.pudu
---

# Text

## Purpose and Interface

Adds `notEmpty`, `empty`, `length`, `minimumLength`, `maximumLength`, `matches`, `emailAddress`, `creditCard`, and `enumName` checks to a `RuleBuilder[T,Str]`. Named checks attach stable codes and original English messages. `matches` compiles the pattern once when building the rule; invalid patterns are reported by a typed `Result` instead of becoming a validation failure.

## Algorithm and edge cases

Length bounds are rendered into failure messages when a check fails.

Length checks attach `MinLength`, `MaxLength`, and `TotalLength` message arguments as applicable, including when the caller supplies a custom message.

Whitespace-only text is empty. Length counts Pudu Unicode scalar values. Bounds are inclusive. The email check requires one internal `@`; it does not claim deliverability. Card numbers permit digits with spaces and hyphens, require 12–19 digits, and pass a Luhn check; issuer or account status is outside scope. Enum names compare against an explicit list, optionally ignoring case. Regex matching uses `Std.Regex`; a budget exhaustion is a failed predicate.

## Negative logic

No RFC email parser, DNS check, copied message strings, or hidden regex compilation per validated item.

## Depth

MEDIUM — typed text predicates and one fallible build boundary.

## Grill Log

- **Q:** Reject invalid regex at validation time? **A:** Reject at rule construction. **Rationale:** configuration mistakes are distinct from bad input. **Rejected:** treating a malformed pattern as a property failure.
- **Q:** Does length imply nonempty? **A:** No; only the requested interval is applied. **Rationale:** composition stays explicit. **Rejected:** implicit presence check.

## Referenced by

[[src/PuduLangValidator/Rules/_MOC]]
