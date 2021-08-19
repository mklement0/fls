# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **[v0.4.0](https://github.com/mklement0/fls/compare/v0.3.3...v0.4.0)** (2021-08-19):
  * An attempt to use `ls`'s `-R` / `--recursive` option now causes an error; it was never meaningfully supported.

* **[v0.3.3](https://github.com/mklement0/fls/compare/v0.3.2...v0.3.3)** (2020-01-26):
  * [dev] Updated the (design-time only) npm packages.

* **[v0.3.2](https://github.com/mklement0/fls/compare/v0.3.1...v0.3.2)** (2018-07-26):
  * [doc] Read-me formatting fixed.

* **[v0.3.1](https://github.com/mklement0/fls/compare/v0.3.0...v0.3.1)** (2018-07-25):
  * [dev] `npm`-package security vulnerabilities fixed - note that these vulnerabilities never affected _runtime_ operation.

* **[v0.3.0](https://github.com/mklement0/fls/compare/v0.2.3...v0.3.0)** (2015-09-17):
  * [BREAKING CHANGES] Filter characters streamlined to be (a) all-lowercase (`l` now accepted in addition to `L` and `h`),
    `s` in addition to `S`; (b) new filter `e` for testing emptiness added, which supersedes the previous `s` filter with _opposite_ semantics.
    `s` now means test for a socket, and what was previously `s` (non-emptiness test) can now be expressed more intuitively as
    `^e` (negated emptiness test), and, conversely, a simple `e` tests for emptiness rather than the obsolete double-negative `^s`.

* **[v0.2.3](https://github.com/mklement0/fls/compare/v0.2.2...v0.2.3)** (2015-09-16):
  * [doc] man page improvements.

* **[v0.2.2](https://github.com/mklement0/fls/compare/v0.2.1...v0.2.2)** (2015-09-16):
  * [doc] `fls` now has a man page, and `-h` outputs concise usage information only.
  * [fix] Filenames with backslashes are now handled correctly.

* **[v0.2.1](https://github.com/mklement0/fls/compare/v0.2.0...v0.2.1)** (2015-09-15):
  * [dev] Makefile improvements; various small behind-the-scenes fixes.

* **[v0.2.0](https://github.com/mklement0/fls/compare/v0.1.5...v0.2.0)** (2015-07-26):
  * **[breaking changes]**
     * Behavior aligned with `ls` so as to facilitate use of `fls`-based aliases as general `ls` replacements with optional on-demand filtering.
     * The filter argument may now be placed either before or after options; only _before_ options is it unambiguously a filter; alternatively,
       following the first operand with `--` also unambiguously marks it as a filter.  
       Options may now be intermingled with operands, even
       on platforms whose `ls` implementation doesn't support it.
     * Conversely, use `--` in lieu of a filter to explicitly requests that _no_ filtering be performed - the previously used `-` no longer works.
     * If the first operand is not unambiguously specified as a filter and it is not a valid filter, it is treated as a file/dir. operand.
     * `-d`, previously only used behind the scenes for _multiple_ operands, can now be used explicitly to request that a _single_ operand
       that is a _directory_ be targeted as _itself_, as opposed to its contents.
  * [fix] Filter validation improved to correctly detect mutually exclusive types and duplicate filters.
  * [dev] Improved tests.

* **[v0.1.5](https://github.com/mklement0/fls/compare/v0.1.4...v0.1.5)** (2015-07-18):
  * [fix] Symlinks passed as file operands are now correctly detected, even if their targets do not exist.
  * [dev] Test for above fix added, Makefile improvements.

* **[v0.1.4](https://github.com/mklement0/fls/compare/v0.1.3...v0.1.4)** (2015-06-09):
  * [doc] Attempt to fix problem with read-me formatting in the npm registry.
  * [dev] Makefile updated.

* **[v0.1.3](https://github.com/mklement0/fls/compare/v0.1.2...v0.1.3)** (2015-06-03):
  * [doc] Read-me fixed and improved.
  * [dev] Makefile updated.

* **[v0.1.2](https://github.com/mklement0/fls/compare/v0.1.1...v0.1.2)** (2015-06-03):
  * [doc] Read-me improved.

* **[v0.1.1](https://github.com/mklement0/fls/compare/v0.1.0...v0.1.1)** (2015-06-03):
  * [doc] Read-me improved.
  * [dev] Makefile updated.

* **v0.1.0** (2015-06-03):
  * [dev] Version bumped to 0.1.0 to better reflect the level of maturity.
  * [doc] TOC problem fixed.

* **v0.0.2** (2015-06-03):
  * [fix] Simple, but fundamental bug on Linux fixed (my apologies): no longer tries to use `command` with `exec`, which fails on Linux, because `command` is only a builtin (and was never needed to begin with).

* **v0.0.1** (2015-06-03):
  * Initial release.
