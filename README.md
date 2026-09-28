<p align="center">
  <img src="public/pudu-lang-short.png" alt="Pudu" width="120">
</p>

<p align="center">
  <a href="https://www.pudu-lang.org/">Pudu</a> |
  <a href="https://www.pudu-lang.org/docs">Documentation</a> |
  <a href="wiki/00-INDEX.md">Design vault</a> |
  <a href="https://github.com/chrismichaelps/pudu-lang-validator/wiki">API docs</a> |
  <a href="CONTRIBUTING.md">Contributing</a>
</p>

# pudu-lang-validator

A validation package written in Pudu. Define rules for a record, run them against a value, and get back an ordered list of failures. Each failure includes a property path, message, code, severity, and application state.

The package is named `@chrismichaelps/pudu-lang-validator`. Its modules live under `PuduLangValidator`.

## Example

```pudu
module Example

import PuduLangValidator.Rule as Rule
import PuduLangValidator.Validator as Validator
import PuduLangValidator.Rules.Text as TextRule
import PuduLangValidator.Rules.Number as NumberRule

type Customer = { name: Str, age: Int }

fn customerValidator() -> Validator.Validator[Customer] {
  let name = Rule.build(TextRule.notEmpty(Rule.ruleFor("name", |customer: Customer| customer.name)))
  let age = Rule.build(NumberRule.greaterThanOrEqualTo(Rule.ruleFor("age", |customer: Customer| customer.age), 18))
  Validator.add(Validator.add(Validator.create(), name), age)
}

fn main() -> Int {
  let validator = customerValidator()
  let outcome = Validator.validate(&validator, Customer{name: "", age: 16})
  if outcome.isValid { 0 } else { 1 }
}
```

`Validator.validate` returns a `ValidationResult`. In this example, both rules fail, so `outcome.isValid` is `false` and `outcome.errors` has two entries in rule order. You can change a message or code on an individual check before calling `Rule.build`.

## What it includes

| Area | Modules |
| --- | --- |
| Rules and execution | `Rule`, `Validator`, `Async` |
| Text, number, and collection checks | `Rules.Text`, `Rules.Number`, `Rules.Decimal`, `Rules.Presence`, `Rules.Collection` |
| Equality, ordering, and nested values | `Rules.Comparison`, `Rules.Ordering`, `Rules.Child`, `Rules.Case` |
| Results and application support | `Result`, `Advanced`, `Localization`, `Testing`, `Http` |

Rules can run in named sets or over selected properties. For a collection, `Validator.includeProperties(Validator.defaults(), ["orders[].sku"])` selects `sku` from every order; failures still name their original indices. Child validators keep the full path, such as `orders[2].sku`.

Checks can use custom messages with `{PropertyName}`, `{PropertyPath}`, and `{PropertyValue}`. Built-in checks also provide arguments for bounds and measured values. Use `Rule.withArgument` or `Rule.withArgumentFrom` to add your own. `Rule.withSeverityFrom` and `Rule.withStateFrom` compute failure metadata from the source and selected value.

Cross-property checks such as `Comparison.equalToProperty` and `Ordering.greaterThanProperty` take a typed selector and an explicit path for the other property. `Decimal.precisionScale` reserves `precision - scale` digits for the whole-number part. `Text.matches` returns a typed error if its pattern cannot be compiled; a valid pattern can appear in a failure message as `{RegularExpression}`.

`Async` runs awaited checks alongside rules lifted from `Rule`. Call `Async.validateAsync` at that boundary. Both validator types support a prevalidation decision before ordinary rules run. The `Testing` module offers result queries for paths, messages, codes, severity, and state.

The [source mirrors](wiki/src/_MOC.md) record each module's behavior and design decisions.

## Installing

After the package is published, add it to a Pudu project:

```sh
pudu install @chrismichaelps/pudu-lang-validator
```

Import the modules you need under `PuduLangValidator`, as in the example above.

### Build from source

Install [Pudu 0.1.2](https://www.pudu-lang.org/download), then:

```sh
git clone https://github.com/chrismichaelps/pudu-lang-validator
cd pudu-lang-validator
pudu test test
pudu build src/Main.pudu -o pudu-lang-validator
./pudu-lang-validator --version
```

## Developing

```sh
pudu check $(rg --files src test -g '*.pudu')
pudu fmt --check src test
pudu lint src test
pudu test test
```

The [wiki vault](wiki/00-INDEX.md) has a design page for each package module.

## License

[Apache License 2.0](LICENSE).
