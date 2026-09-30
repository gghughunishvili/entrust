# Changelog

All notable changes to this package are documented here. The major version tracks the
Laravel major release it targets, so Entrust `11.x` targets Laravel `11.x`.

## [11.0.0]

### Added

- Support for **Laravel 11** (`illuminate/console`, `illuminate/support` and
  `illuminate/cache` at `^11.0`).
- A `entrust-config` publish tag, so the config can be published with
  `php artisan vendor:publish --tag=entrust-config`.
- A GitHub Actions matrix that runs the suite on PHP 8.2, 8.3 and 8.4, against both the
  lowest and the latest resolvable dependency set.
- `CHANGELOG.md` and a compatibility table in the README.

### Changed

- **Minimum PHP is now 8.2**, matching Laravel 11.
- The generated migration is now an anonymous class (`return new class extends Migration`),
  the form Laravel has generated since 8.x. Regenerating no longer collides on the
  `EntrustSetupTables` class name.
- The generated migration no longer wraps its schema changes in an explicit
  `DB::beginTransaction()` / `DB::commit()` pair; DDL is not transactional on MySQL and the
  wrapper only masked failures.
- The generated migration imports `Schema` explicitly and uses `unsignedBigInteger()`.
- Test suite moved to PHPUnit 10.5/11 — data providers are static and declared with the
  `#[DataProvider]` attribute, and `phpunit.xml` uses the current schema with
  `failOnWarning`, `failOnDeprecation` and `failOnNotice` enabled.
- README documents Laravel 11: package discovery, middleware aliases in `bootstrap/app.php`
  (`app/Http/Kernel.php` no longer exists), and `App\Models` namespaces.

### Fixed

- The CI workflow, which had been red since #11. It was an unmodified copy of the Laravel
  *application* template: it copied a `.env.example` this package does not have and
  created a SQLite database the suite never opens, on `actions/checkout@v1`. Replaced by a
  PHP × Laravel matrix.
- README now documents the Lumen alias-order trap behind #5: Lumen's `withAliases()` keys
  the array by facade class, the reverse of Laravel's `config/app.php`.
- `explode(): Passing null to parameter #2` deprecation in the `EntrustRole`,
  `EntrustPermission` and `EntrustAbility` middleware on PHP 8.1+.
- Implicitly nullable parameter deprecation on PHP 8.4 in the test suite.

### Removed

- Support for Laravel 6 through 10. Stay on Entrust `^10.0` for those.
- Support for **Lumen**, which never reached Laravel 11 and has been discontinued
  upstream. The `lumen` keyword is gone from `composer.json`; Lumen users stay on `^10.0`
  (or `3.0` for Lumen 7). Resolves the long-standing question in #5.
- `composer.lock` is no longer committed; a library should resolve against the host
  application's constraints.

### Deprecated

- `Entrust::routeNeedsRole()`, `Entrust::routeNeedsPermission()` and
  `Entrust::routeNeedsRoleOrPermission()`. They call `Route::filter()` / `Route::when()`,
  which Laravel removed in 5.2, so they throw on Laravel 11. Use the middleware instead.

## [10.0.0] and earlier

See the [commit history](https://github.com/gghughunishvili/entrust/commits/master).

[11.0.0]: https://github.com/gghughunishvili/entrust/releases/tag/11.0.0
[10.0.0]: https://github.com/gghughunishvili/entrust/releases/tag/10.0.0
