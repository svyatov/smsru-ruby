# Contributing

Thanks for your interest in improving `smsru-ruby`! Please be respectful and
follow the [Code of Conduct](CODE_OF_CONDUCT.md).

## Maintenance

Leonid Svyatov ([@svyatov](https://github.com/svyatov)) reviews and merges every
pull request and publishes every release. No succession is arranged. If he stops
maintaining the gem, nobody else holds commit or publishing rights. The project
then stalls until he hands those rights over or someone forks it.

## Development setup

```sh
git clone https://github.com/svyatov/smsru-ruby.git
cd smsru-ruby
bin/setup
```

## Running tests and the linter

```sh
bundle exec rake          # RuboCop + tests (the default task)
bundle exec rake test     # tests only
bundle exec rubocop       # linter only
```

End-to-end tests replay recorded API responses. If you change request/response
handling, re-record the cassettes (see the README) and commit them:

```sh
SMSRU_API_ID=your_api_id bundle exec rake vcr:record
COVERAGE=true bundle exec rake   # expect 100% coverage
```

## Code style

- Ruby 3.3+, two-space indentation, 120-character lines.
- RuboCop (with `rubocop-minitest`) must pass; run `bundle exec rubocop -A` to
  auto-correct.
- Add no runtime dependencies. The gem uses the standard library only.

## Commit messages

This project uses [Conventional Commits](https://www.conventionalcommits.org/):
`type(scope): description`, e.g. `feat(stoplist): add bulk import`.
Common types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `perf`, `ci`, `build`.

## Pull request process

1. Fork and create a feature branch.
2. Add tests for your change and keep coverage at 100%.
3. Update `README.md` and `CHANGELOG.md` (under `Unreleased`) as needed.
4. Ensure `bundle exec rake` passes.
5. Open a PR describing the change and the motivation.

Questions? Open an [issue](https://github.com/svyatov/smsru-ruby/issues).
