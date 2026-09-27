# Security policy

Report suspected vulnerabilities privately through the [GitHub advisory form](https://github.com/chrismichaelps/pudu-lang-validator/security/advisories/new) or by email to <chrisperezsantiago1@gmail.com> with `SECURITY` in the subject. Include the package version, Pudu version, platform, and a minimal reproducer.

Validation rules may process untrusted values. Reports about unexpected crashes, resource exhaustion, incorrect acceptance, or disclosure of input through failures are in scope. Regex checks use Pudu's bounded matcher; a pattern that reaches its work limit fails the rule. Applications decide which input values and custom state may appear in user-visible messages.

Compiler or standard-library vulnerabilities belong in the [Pudu repository](https://github.com/chrismichaelps/pudu-lang/security).

## Supported versions

| Version | Supported |
| --- | --- |
| 0.1.x | Yes |
