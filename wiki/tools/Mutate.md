---
type: module
path: tools/Mutate.pudu
---

# Mutate (tool)

## Purpose and Interface

Mutation testing: apply one operator change at a time to source files, run the checks and the suites, and report survivors.

`pudu run tools/Mutate.pudu [--file <path> | --domain] [--every <n>] [--threshold <percent>] [--dry-run]`,
with `PUDU_BIN` naming the compiler (default `pudu`).

## Algorithm and edge cases

Mutants are operator swaps at code positions outside strings, comments, and imports; comparisons must be spaced so type brackets are not mutated.

A mutant that fails `pudu check` is invalid; one whose suites still pass survived.

`--domain` limits the run to the pure predicate layer under `src/PuduLangValidator/Rules`; `--every n` samples; `--threshold` fails the run below a score.

## Negative logic

No background mutation without a verdict. Every mutated file is restored before the next mutant, whatever the verdict.

A mutant whose suites run past 60 seconds counts as killed.

The harness runs on a committed tree only: a run stopped mid-mutant leaves that one change,
which `git diff` shows and `git checkout` removes.

## Depth

MEDIUM — one operator change per run with full suite adjudication.

## Grill Log

- **Q:** Why mutate the predicate layer in pull requests? **A:** It holds the comparisons where a single-point change is most likely to go unnoticed, and it runs in minutes. **Rationale:** boundary mistakes hide in plain operators. **Rejected:** the full tree on every pull request.
- **Q:** What happens to a mutant that cannot change behaviour? **A:** The code is simplified until the mutant is invalid or killed. **Rationale:** an unkillable mutant marks a redundant expression. **Rejected:** excusing survivors.

## Referenced by

[[src/_MOC]]
