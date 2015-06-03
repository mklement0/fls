# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **v0.0.2** (2015-06-03):
  * [fix] Simple, but fundamental bug on Linux fixed (my apologies): no longer tries to use `command` with `exec`, which fails on Linux, because `command` is only a builtin (and was never needed to begin with).

* **v0.0.1** (2015-06-03):
  * Initial release.
