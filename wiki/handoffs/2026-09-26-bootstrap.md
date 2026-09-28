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

Audit assertion helpers against the current result API, then open a focused issue for the first missing behavior before editing its mirrored module page.

## Referenced by

[[handoffs/_MOC]]
