# Entrust 12.0.0 — internal release notes

**Released:** 2026-09-30 · **Tag:** `12.0.0` (`e39985b`) · **Release:** [GitHub](https://github.com/gghughunishvili/entrust/releases/tag/12.0.0) · **PR:** [#19](https://github.com/gghughunishvili/entrust/pull/19)

## What changed and why

Widens the supported window to Laravel 11 and 12.

- `illuminate/{console,support,cache,database}` → `^11.0|^12.0`.
- PHPUnit `^10.5.38|^11.0|^12.0` accepted as a dev dependency.
- CI matrix extended to Laravel 12.
- No source changes. Nothing this package touches moved between Laravel 11 and 12.

## Decisions

- **Additive, not a new major of the code.** The major version still tracks Laravel's major,
  so `12.x` is the line for Laravel 12, and `^12.0` keeps Laravel 11 users working.
- **`php` stays `^8.2`.** Both majors require 8.2, so there is no reason to raise it.

## Data and migration impact

None. Upgrading from `^11.0` is a constraint change only.

## Verification

- CI: 8 jobs green on PR #19 (Laravel 11 and 12 across PHP 8.2–8.4, both resolutions where
  applicable).
- Local Docker: PHP 8.2 `prefer-lowest` (Laravel 12.0.0, PHPUnit 10.5) and `prefer-stable`
  (Laravel 12.69.3, PHPUnit 11.5); PHP 8.4 `prefer-stable` (PHPUnit 12.5). No deprecations.

## Known gaps

- Same as 11.0.0: Lumen unsupported; the `12.0.0` README carries the unclosed code fence
  fixed in `13.0.1`.

## Follow-ups

- Optional `12.0.1` documentation patch, if the `12.x` tag should carry the fence fix.
