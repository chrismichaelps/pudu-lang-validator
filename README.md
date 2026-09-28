<p align="center">
  <img src="public/pudu-lang-short.png" alt="Pudu" width="120">
</p>

<p align="center">
  <a href="https://www.pudu-lang.org/">Pudu</a> |
  <a href="https://www.pudu-lang.org/docs">Documentation</a> |
  <a href="wiki/00-INDEX.md">Design vault</a> |
  <a href="CONTRIBUTING.md">Contributing</a>
</p>

# pudu-lang-validator

Typed validation rules written entirely in Pudu 0.1.2. Build reusable rules with Pudu functions, `Option`, `Result`, and explicit async tasks. Validation returns structured failures in declaration order.

## A validator in Pudu

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

`ValidationResult` gives `isValid` and ordered `errors`. Each failure carries a property path, message, code, severity, and custom state. Rule-level and validator-level cascade, rule sets, property selection, child validators, collection indices, custom rules, and async checks are explicit package APIs.

## Installing

Requires [Pudu 0.1.2](https://www.pudu-lang.org/download) on `PATH`.

```sh
git clone https://github.com/chrismichaelps/pudu-lang-validator
cd pudu-lang-validator
pudu test test
pudu build src/Main.pudu -o pudu-lang-validator
./pudu-lang-validator --version
```

The manifest identifies this package as `@chrismichaelps/pudu-lang-validator`. Import modules under `PuduLangValidator` when the package is available in your project.

## APIs

| Area | Modules |
| --- | --- |
| Rule construction and metadata | `PuduLangValidator.Rule`, `PuduLangValidator.Validator` |
| Results and typed failures | `PuduLangValidator.Result` |
| Built-in checks | `PuduLangValidator.Rules.Text`, `.Number`, `.Decimal`, `.Presence`, `.Comparison`, `.Ordering`, `.Collection`, `.Case` |
| Nested and custom rules | `PuduLangValidator.Rules.Child`, `PuduLangValidator.Advanced` |
| Asynchronous checks | `PuduLangValidator.Async` |
| Messages and integrations | `PuduLangValidator.Localization`, `.Testing`, `.Http` |

Rule sets, property selection, cascade behavior, conditions, nested paths, collection filters, context data, severity, codes, and custom state are covered by the package tests. The [source mirrors](wiki/src/_MOC.md) describe each module's contract.

Use `Validator.includeProperties(Validator.defaults(), ["orders[].sku"])` to validate one field across every item in a collection. Concrete indices, including nested indices, remain in failure paths.

Messages support `{PropertyName}`, `{PropertyPath}`, and `{PropertyValue}`. Built-in rules expose their bounds and measured values as named arguments; `Rule.withArgument` and `Rule.withArgumentFrom` let application rules provide their own.

`Rule.withSeverityFrom` and `Rule.withStateFrom` derive failure metadata from the source and selected value. Async rules offer root-based variants with the same names.

`Comparison.equalToProperty` and `Ordering.greaterThanProperty` compare a selected value with another typed property on the same source. Their companion functions cover inequality and inclusive or exclusive ordering; each takes an explicit comparison path for failure messages.

`Testing.testValidate` and `Testing.testValidateAsync` run real validators for tests. Start a result query with `Testing.forProperty(&result, "name")`, narrow it with `Testing.withCode` or other metadata filters, then use `Testing.hasAny`, `Testing.hasNone`, or `Testing.only` with `Std.Test.that`.

## Developing

```sh
pudu check $(rg --files src test -g '*.pudu')
pudu fmt --check src test
pudu lint src test
pudu test test
```

The [wiki vault](wiki/00-INDEX.md) holds a design page for every implementation module and the choices behind the public API.

## License

[Apache License 2.0](LICENSE).
