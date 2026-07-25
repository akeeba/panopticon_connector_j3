# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build Commands

The build system uses Apache Phing 3.x. The shared build infrastructure lives in `../buildfiles/phing/common.xml` (outside this repo).

```bash
# Full build (default target): version setup, packaging, update XML generation
phing
```

The `version.php` file under `plugins/system/panopticon/` is **generated at build time** via token replacement (`##VERSION##`, `##DATE##`). It is gitignored — do not create or edit it manually.

## Architecture

### Key Patterns

- **Controllers** are callables: implement `__invoke(\JInput $input): object`. Routes are registered in `panopticon.php::getRouter()`.
- **JSON:API responses** use `AbstractController::asSingleItem()` and `asItemsList()` helpers.
- **All PHP files** start with `defined('_JEXEC') || die;` as a Joomla security guard.
- **Runtime bounds**: minimum supported PHP is `7.2.5`; the codebase is intended to remain parse-compatible through PHP `8.1.x`. Deprecation notices on PHP `8.1+` are acceptable for this project, but syntax requiring PHP `8.2+` is not.
- **Polyfills** in `polyfills.php` provide PHP 8 string functions (`str_contains`, `str_starts_with`, `str_ends_with`) for PHP 7.x compatibility.

## Code Conventions

- Allman brace style (braces on own line)
- Copyright header: `@package panopticon`, `@copyright Copyright (c)2023-2026 Nikolaos Dionysopoulos / Akeeba Ltd`, `@license` AGPL-3.0
- Use Joomla's `JInput` for request input, `JDatabaseQuery` for SQL (parameterized queries)
- Errors are `RuntimeException` with HTTP status codes; the plugin serialises the exception chain into a JSON:API `errors` array
- No test suite exists in this repository
