---
type: architecture
---

# Delivery

`main` is the release branch and `dev` is the integration branch. Feature work starts from a ready issue on `feature/<issue>-<slug>`. A contributor other than the implementation author reviews public behavior and wiki parity. Commits use `type(scope): imperative summary refs #<issue>`. Check, format, lint, test, and a compiled CLI smoke run before review.

## Grill Log

- **Q:** Can a bootstrap have no remote issue? **A:** Keep it locally reviewable and create the issue before a feature PR. **Rationale:** branch naming must refer to a real issue. **Rejected:** inventing an issue number.

## Referenced by

[[architecture/_MOC]]
