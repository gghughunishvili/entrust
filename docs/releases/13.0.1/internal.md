# Entrust 13.0.1 — internal release notes

**Released:** 2026-09-30 · **Tag:** `13.0.1` (`c6c7f25`) · **Release:** [GitHub](https://github.com/gghughunishvili/entrust/releases/tag/13.0.1) · **PRs:** [#25](https://github.com/gghughunishvili/entrust/pull/25), [#26](https://github.com/gghughunishvili/entrust/pull/26)

## What changed and why

Documentation only. No code, constraint or test change.

- **Unclosed Markdown fence** in the Laravel 11 middleware step (introduced in #18). One missing
  code-fence marker inverted every fenced block after it, so from "Configuration" onward GitHub rendered
  prose as code and code as prose. This shipped in `11.0.0`, `12.0.0` and `13.0.0`.
- **Middleware snippet clarified.** It read as a standalone statement, which invited a second
  `withMiddleware()` call in `bootstrap/app.php`. It now says to add the body to the closure the
  skeleton already contains, and notes the Laravel 13 signature `function (Middleware $middleware): void`.
- **Version index** now points `^13.0` at `13.0.1`, and records that the usage API is identical
  across `11.x`–`13.x`.

## Decisions

- **No backport to `11.0.x` / `12.0.x`.** The defect is formatting, not content: every
  instruction on those pages is still correct. Re-releasing two tags for a Markdown typo costs
  more than it returns. The version index points readers at the `13.x` page instead.
- **Fence guard recorded as a rule.** Verify fence count is even and no fenced block contains a
  heading before tagging a docs change.

## Data and migration impact

None.

## Verification

- CI: 11 jobs green on PR #25 and PR #26.
- Structural checks on `master`: fence count 80 (even); zero fenced blocks contain a heading.
- Every `blob/<tag>/README.md` and `releases/tag/<tag>` link in the version index returned
  HTTP 200 after the tag was pushed.
- The middleware instructions were followed verbatim in a fresh `laravel/laravel:^13.0` app.

## Known gaps

- `11.0.0`, `12.0.0` and `13.0.0` tags keep the fence defect. Readers are pointed at `13.0.1`.

## Follow-ups

- None required.
