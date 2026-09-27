---
type: handoff
status: ACTIVE
---

# Validator Bootstrap

## FMCF roles

**DNA Engineer:** specified the Pudu rule model and module mirrors. **Shadow:** implements the declared modules and focused checks. **Forensic Guardian:** audits source parity, links, and changelog after validation.

## Ownership

The bootstrap owns `src/PuduLangValidator/**`, `src/Main.pudu`, their mirrors, package metadata, and package tests. The sibling Pudu compiler repository is read-only reference material.

## Exact next action

Compose nested asynchronous validators on issue #5, test child selection and failure paths, then review source parity before integration into `dev`.

## Referenced by

[[handoffs/_MOC]]
