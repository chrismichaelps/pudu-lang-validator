---
type: handoff
status: ACTIVE
---

# Validator Bootstrap

## FMCF roles

**DNA Engineer:** specified the Pudu rule model and package contracts. **Shadow:** maintains the README and focused checks. **Forensic Guardian:** audits source parity, links, and changelog after validation.

## Ownership

The bootstrap owns `src/PuduLangValidator/**`, `src/Main.pudu`, their mirrors, package metadata, and package tests. The sibling Pudu compiler repository is read-only reference material.

## Exact next action

Confirm issue #26 restores a passing GitHub Actions check, then prepare release 0.1.0 from updated `dev`.

## Referenced by

[[handoffs/_MOC]]
