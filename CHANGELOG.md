# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

The public API that Semantic Versioning covers is every constant and method
under `SmsRu` that YARD documents as public: the `SmsRu` client and its `auth`,
`callbacks`, `call_check`, `my`, and `stoplist` sub-resources, the result and
event `Data` classes, the `SmsRu::Statuses` constants and predicates, the error
hierarchy under `SmsRu::Error`, and `SmsRu::Webhook.parse`. Anything marked
`@api private`, including `SmsRu::Coerce` and the sub-resource constructors, may
change in any release. The minimum supported Ruby version is part of the public
API: raising it takes a MAJOR release.

## [Unreleased]

### Added

- This changelog now declares which constants and methods Semantic Versioning
  covers, so a version range says something checkable about what may change.

## [2.0.1] - 2026-07-31

### Added

- Releases are now built and published by GitHub Actions over OIDC, and every
  published gem carries a Sigstore provenance attestation. Verify one with
  `https://rubygems.org/api/v1/attestations/smsru-ruby-VERSION.json`.

### Changed

- Renamed `LICENSE.txt` to `LICENSE`. The license text is untouched and the gem
  is still MIT.
- Rewrote the README and widened the gemspec summary and description to describe
  the full API surface.

## [2.0.0] - 2026-07-13

### Changed

- **Breaking:** Renamed the gem from `smsru_ruby` to `smsru-ruby` for
  consistency with the `-ruby` suffix convention. Update your `Gemfile`
  (`gem "smsru-ruby"`) and requires (`require "smsru-ruby"`). The Ruby API is
  unchanged. The top-level class is still `SmsRu`.

## [1.0.0] - 2026-06-26

First public release. A Ruby port of the official SMS.ru PHP library covering the
same API, reworked to be idiomatic Ruby. How it differs from the original:

- **Idiomatic, namespaced API** instead of flat `get_*`/`add_*` methods: account
  reads under `client.my` (`#balance`, `#limit`, `#free_limit`, `#senders`),
  credential check via `client.auth.ok?`, plus `client.stoplist`,
  `client.callbacks`, and `client.callcheck` sub-resources. Keyword arguments for
  every optional send parameter, plus a per-client `from` default.
- **Typed, immutable `Data` results** that separate *operation outcome* from
  *delivery state*: `#ok?` plus `#error_code`/`#error_text` on rejected
  `Sms`/`CostItem` entries; `#delivered?`/`#pending?`/`#failed?`/`#found?` and
  named `SmsRu::Statuses` constants for the delivery `status_code` on `Status`
  and webhook events; `#ok?`/`#ok`/`#failed` collection helpers on `SendResult`
  and `Cost`; plus `#confirmed?` and `#available_today`. No raw decoded JSON or
  magic numbers.
- **Typed error hierarchy** under `SmsRu::Error` (`AuthError`,
  `InsufficientFundsError`, `ResponseError`, `ConnectionError`). Errors are
  raised rather than returned as status codes you have to inspect.
- **First-class inbound webhooks**: `SmsRu::Webhook.parse` decodes the callback
  POST into typed events (`SmsRu::Events::SmsStatus`, `CallcheckStatus`, `Test`,
  `Unknown`), and `SmsRu::Webhook.valid?` verifies the signature.
- **Zero runtime dependencies** (Ruby stdlib only, no curl), TLS verified by
  default, with configurable `timeout`, `retries`, global `test` mode, and an
  optional `logger`.
- **Ships RBS type signatures** (`sig/`) checked at 100% coverage under Steep's
  strict profile and verified against the test suite at runtime (`rbs test`);
  SMS.ru's loosely-typed JSON is normalized to the declared types at the parse
  boundary, so result objects never surface raw wire values.

[Unreleased]: https://github.com/svyatov/smsru-ruby/compare/v2.0.1...HEAD
[2.0.1]: https://github.com/svyatov/smsru-ruby/compare/v2.0.0...v2.0.1
[2.0.0]: https://github.com/svyatov/smsru-ruby/compare/v1.0.0...v2.0.0
[1.0.0]: https://github.com/svyatov/smsru-ruby/releases/tag/v1.0.0
