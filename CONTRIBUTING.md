# Contributing to pudu-lang-validator

Read [the vault](wiki/00-INDEX.md), the [validation design](wiki/architecture/Validation.md), and the [Pudu grammar](wiki/grammar/pudu.md) before changing code. Write and resolve a source file's mirrored wiki page before its implementation.

## Branches

- `main` holds released versions.
- `dev` integrates reviewed changes.
- Feature work uses `feature/<issue>-<slug>` from `dev`, then targets `dev` in a pull request.

The first local commit establishes the repository before a remote issue tracker exists. Later behavior commits name their issue: `feat(rules): add collection filters refs #12`.

## Code

Package modules live under `PuduLangValidator`. Keep every source file below 500 lines. Public types and modules use a `/** @Namespace.Entity.Role — intent */` anchor; public functions have short `///` comments useful in LSP hover. Explain behavior and boundaries in the wiki, not in long source comments.

## Checks

```sh
pudu check $(rg --files src test tools -g '*.pudu')
pudu fmt --check src test tools
pudu lint src test tools
pudu test test
pudu build src/Main.pudu -o /tmp/pudu-lang-validator
```

Review success, failure, edge, and output cases for the changed behavior. Check the implementation against its mirror and record the logic change in `wiki/CHANGELOG.md`.

Changes to predicates also run mutation testing on the files they touch, and no new mutant may survive
without a written reason:

```sh
pudu run tools/Mutate.pudu --file src/PuduLangValidator/Rules/Text.pudu
```

## Pull requests

Target `dev`, link the ready issue, report focused and full check results, and obtain a reviewer who did not implement the change. The reviewer checks both public behavior and wiki parity.
