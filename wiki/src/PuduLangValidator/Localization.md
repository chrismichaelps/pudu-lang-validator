---
type: module
path: src/PuduLangValidator/Localization.pudu
---

# Localization

## Purpose and Interface

`Catalog` stores original application messages by `(locale, code)`. `empty`, `withMessage`, and `messageFor` build and query it. `localized` attaches a message lookup to the latest typed rule check, taking the locale from the source model and a caller-supplied fallback. Rule message placeholders are rendered by the core rule engine after lookup.

## Algorithm and edge cases

Locale keys are lowercase. Lookup tries the exact locale, then a language subtag before `-`, then the supplied fallback. An empty catalog always uses the fallback. Catalog values are written by the application.

## Negative logic

No process-global language manager or implicit culture from the host OS.

## Depth

MEDIUM — explicit locale lookup and failure message integration.

## Grill Log

- **Q:** Use a global current culture? **A:** Pass locale through the source. **Rationale:** concurrent validations must not change one another's language. **Rejected:** mutable global culture.
- **Q:** Copy framework translations? **A:** No; applications supply their own messages. **Rationale:** keeps copyright and product voice clear. **Rejected:** imported resource catalogs.

## Referenced by

[[src/PuduLangValidator/_MOC]]
