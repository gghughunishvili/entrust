# Entrust 13.0.0 — internal release notes

**Released:** 2026-09-30 · **Tag:** `13.0.0` (`13df62c`) · **Release:** [GitHub](https://github.com/gghughunishvili/entrust/releases/tag/13.0.0) · **PRs:** [#20](https://github.com/gghughunishvili/entrust/pull/20), [#24](https://github.com/gghughunishvili/entrust/pull/24)

## What changed and why

Widens the supported window to Laravel 11, 12 and 13, and adds a per-version documentation
index to the README.

- `illuminate/{console,support,cache,database}` → `^11.0|^12.0|^13.0`.
- CI matrix covers all three majors; Laravel 13 excluded on PHP 8.2 (Laravel 13 requires 8.3).
- `prefer-lowest` pinned per major to that major's minimum PHP (11 and 12 on 8.2, 13 on 8.3).
- README compatibility table became a documentation index: every released tag from `2.0.0`
  upward, with constraints read from each tag's own `composer.json`, a maintenance status, and
  links to the README frozen at that tag and to its release page.

## Decisions

- **`php` stays `^8.2`, not `^8.3`.** Raising it would lock Laravel 11 and 12 users on PHP 8.2
  out of upgrades they can make. Laravel 13's own constraint already enforces 8.3 for anyone who
  resolves it, so the package does not need to repeat it.
- **Per-version docs link to the README at each tag, not to a generated docs site.** Tags are
  immutable, so a link to `blob/<tag>/README.md` is a true snapshot. A hosted docs site would
  need a maintenance pipeline and would drift.
- **Laravel 13 landed through a stacked PR whose base was a feature branch.** PR #20 merged into
  `feature/laravel-12` instead of `master`. It was caught and corrected by PR #24, which merged
  the work into `master` before tagging. Recorded so the same trap is avoided next time.

## Data and migration impact

None. Upgrading from `^12.0` or `^11.0` is a constraint change only.

## Verification

- CI: 11 jobs green on PR #20; 11 jobs green on PR #24.
- Local Docker: PHP 8.3 `prefer-lowest` (Laravel 13.0.0, PHPUnit 10.5) and `prefer-stable`
  (Laravel 13.34.0, PHPUnit 12.5); PHP 8.4 `prefer-stable`. No deprecations.
- Published-package check: `gghughunishvili/entrust:^13.0` installed from Packagist into a fresh
  `laravel/laravel:^13.0` app (Laravel 13.34). Discovery, config publish, migration, role,
  permission, ability, scope and pivot-cleanup assertions all passed. The three middleware
  checks returned 200/403/200 after aliases were registered in the Laravel 13 `bootstrap/app.php` closure.
- Links in the version index: `blob/13.0.0/README.md` returned 404 before its tag was pushed and
  200 afterward. The final index was checked against `master` and every link returned 200.

## Known gaps

- The `13.0.0` README carries an unclosed Markdown code fence that inverts most of the page
  below "Configuration". **Fixed in `13.0.1`.** The `13.0.0` tag was not re-released; the version
  index points `^13.0` at `13.0.1`.
- Lumen unsupported.

## Follow-ups

- Keep release notes in `docs/releases/<version>/` for every release from now on. The 11.0.0 to
  13.0.1 notes were added retroactively.
