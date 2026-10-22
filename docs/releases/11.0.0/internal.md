# Entrust 11.0.0 — internal release notes

**Released:** 2026-09-30 · **Tag:** `11.0.0` (`26f2b6d`) · **Release:** [GitHub](https://github.com/gghughunishvili/entrust/releases/tag/11.0.0) · **PR:** [#18](https://github.com/gghughunishvili/entrust/pull/18)

## What changed and why

Targets Laravel 11 and starts the `11.x` line. The major version now tracks Laravel's own,
so `11.x` means "for Laravel 11.x".

- Constraints: `illuminate/{console,support,cache}` → `^11.0`; `php` → `^8.2`.
- Dropped the Lumen keyword and every Lumen claim. Lumen never reached Laravel 11 and is
  discontinued upstream.
- Generated migration is an anonymous class (`return new class extends Migration`). The old
  named class `EntrustSetupTables` collided when `entrust:migration` ran twice.
- Explicit `DB::beginTransaction()`/`commit()` removed from the generated migration. DDL is
  not transactional on MySQL, so the wrapper implied atomicity it did not provide.
- Config publishes under the `entrust-config` tag.
- Middleware `explode()` calls cast their input to `string`. PHP 8.1+ emitted a deprecation
  on every middleware call when a route passed no roles.
- `composer.lock` untracked. A library should resolve against its host application.
- `minimum-stability: dev` removed; it pulled dev builds of dev dependencies into installs.

## Decisions

- **Supported window = Laravel 11 onward; Laravel 6–10 left on `10.0.0`.** Alternative was
  additive (keep `php >=7.2`, PHPUnit 8). Rejected: PHPUnit 8 and a Laravel 11 suite cannot be
  in one dependency set, and an additive release would have kept the untested `php >=7.2`
  claim alive.
- **CI: matrix instead of one job.** Old workflow was a copy of the Laravel *application*
  template (copied a non-existent `.env.example`, created a SQLite DB the suite never opens).
  Replaced by PHP 8.2/8.3/8.4 × Laravel × `prefer-lowest|prefer-stable`.
- **`prefer-lowest` only on each major's own minimum PHP.** An `x.0.0` framework release
  predates later PHP versions and emits its own deprecations there, which masks package
  regressions.
- **PHPUnit 10.5/11 with `failOnWarning`, `failOnDeprecation`, `failOnNotice`.** Chosen so
  future deprecations fail the build rather than scroll past.

## Data and migration impact

- Existing installs: none to the database schema. The generated migration is for new installs
  and for users regenerating it.
- Upgrading from `10.x`: `vendor:publish` tag changed (`--tag=entrust-config`); middleware
  aliases move from `app/Http/Kernel.php` to `bootstrap/app.php`; the three
  `routeNeedsRole`/`routeNeedsPermission`/`routeNeedsRoleOrPermission` helpers are deprecated
  because they depend on `Route::filter()`, which Laravel removed in 5.2.

## Verification

- Suite: 51 tests / 100 assertions green on every matrix cell (PR #18, 4 jobs).
- Local Docker: PHP 8.2 `prefer-lowest` (Laravel 11.0.0, PHPUnit 10.5) and `prefer-stable`;
  PHP 8.4 `prefer-stable` (Laravel 11.57, PHPUnit 11.5). No deprecations with `failOnDeprecation`.
- Published-package check: `gghughunishvili/entrust:^11.0` installed from Packagist into a fresh
  `laravel/laravel:^11.0` app. Discovery, config publish, migration generate + migrate,
  role/permission/ability assertions, pivot cleanup on delete, and the README middleware
  snippet (`role:owner` 200, `role:admin` 403, `permission:edit-post` 200).

## Known gaps

- Lumen unsupported. Lumen 6/7 users stay on `^10.0` / `3.0`.
- The Laravel 11 release shipped with an unclosed Markdown code fence in its README
  (fixed in `13.0.1`; `11.0.0` tag was not re-released).
- Packagist's CI badge and version badges depend on third-party badge hosts.

## Follow-ups

- Optional `11.0.1` documentation patch if the fence fix should be on the `11.x` tag itself.
