# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

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
