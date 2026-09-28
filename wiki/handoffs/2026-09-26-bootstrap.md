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

Audit remaining validation context and child-selection behavior, then open a focused issue before editing its source mirror.

## Referenced by

[[handoffs/_MOC]]
