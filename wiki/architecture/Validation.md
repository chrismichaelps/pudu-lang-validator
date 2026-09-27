---
type: architecture
status: active
---

# Validation

## Purpose

A reusable, typed validation library for Pudu records. Rules are values. A property selector keeps the source record type and property type explicit; a built rule erases only the property type, retaining the source type. Validation accumulates failures in declaration order.

## Interface

`Rule.ruleFor` starts a property rule. `Rule.must` adds a predicate. The latest predicate can receive a message, code, severity, state, and condition. `Rule.build` freezes a builder. `Validator.create`, `add`, `include`, `validate`, and `validateWith` compose and run rules. `Text`, `Number`, and `Collection` offer named predicates over their respective property types.

## Compatibility boundaries

The API uses Pudu module functions and immutable builders. `Option[T]` expresses optional inputs. Errors are returned as values. HTTP and UI integrations consume structured results at their own boundaries. Asynchronous checks need explicit Pudu task boundaries and are not silently run by synchronous validation.

## Invariants

- A predicate receives the root and selected property once per check; selection runs once per rule.
- A false predicate emits one failure with stable path, code, and metadata.
- Rule cascade stops later checks in that rule; validator cascade stops later rules.
- Default selection runs unnamed rules. Named selection runs matching rules and can include unnamed rules explicitly. All selection runs every rule.
- Property selection compares the declared path and nested prefixes.
- Validation does not mutate its validator or input.
- Context data is passed through root, child, and collection validation without mutation.

## Grill Log

- **Q:** Should rule building be mutable? **A:** Use Pudu functions and immutable values. **Rationale:** rules remain reusable and predictable across validations. **Rejected:** a dynamic stringly typed rule bag.
- **Q:** Throw on invalid input? **A:** Return structured results. **Rationale:** validation failure is an expected domain outcome. **Rejected:** panic or host exception emulation.
- **Q:** How are nested paths represented? **A:** Joined dotted names and bracketed collection indices. **Rationale:** stable, readable failure paths. **Rejected:** parsing selectors to infer paths.
- **Q:** Copy default English messages? **A:** Write original messages. **Rationale:** behavior can align without reproducing prose. **Rejected:** copying documentation or resource strings.

## Referenced by

[[architecture/_MOC]] · [[src/PuduLangValidator/Rule]] · [[src/PuduLangValidator/Validator]]
