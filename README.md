[![npm version](https://img.shields.io/npm/v/fls.svg)](https://npmjs.com/package/fls) [![license](https://img.shields.io/npm/l/fls.svg)](https://github.com/mklement0/fls/blob/master/LICENSE.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [fls &mdash; Introduction](#fls-mdash-introduction)
- [Examples](#examples)
- [Installation](#installation)
  - [Installation from the npm registry](#installation-from-the-npm-registry)
  - [Manual installation](#manual-installation)
- [Usage](#usage)
- [License](#license)
  - [Acknowledgements](#acknowledgements)
  - [npm dependencies](#npm-dependencies)
- [Changelog](#changelog)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# fls &mdash; Introduction

`fls` &mdash; **f**iltering **ls** &mdash; is a **type-filtering wrapper** for the standard **`ls`** Unix utility.

The general idea is to **enhance `ls` with the ability to filter items by filesystem type** by specifying a filter expression as the first argument.  
A filter expression is composed of **one or more optionally negatable filter characters based on the type-identifying chars. supported by [`find`'s `-type` primary](http://www.gnu.org/software/findutils/manual/html_node/find_html/Type.html#Type) and [Bash's file-test operators](http://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)**,  
such as `f` for files, `d` for directories.

Behind the scenes `ls` is ultimately invoked, so all of its options - except `-R` / `--recursive`, `--ignore`, `-f` and `-U` - are supported.  
Specifying a filter is optional, so `fls` can generally be used in lieu of `ls`, with a few restrictions detailed in the [manual](doc/fls.md).

The following example lists only subdirectories in the current directory, in long format:

    fls d -l

`fls` also lends itself to creating aliases; for instance, the following alias lists all executable (`x`) files (`f`):

* `alias lsbin='fls fx'`

This alias can serve as a general `ls` replacement with fixed options, to which you may optionally pass filters as the first operand:

* `alias lsx='fls -FAhl'`

See examples below, read concise [usage information](#usage) further below, or read the [manual](doc/fls.md).

# Examples

```shell
# List all files (not directories) in the current dir.
fls f

# List all subdirs. of the user's home dir.
fls d ~

# List all symlinks to hidden files in long format in the user's home dir.
fls lf -l ~/.*

# List all executable files matching c* in /usr/local/bin
fls xf /usr/local/bin/c*

# List all empty (zero bytes) files in the current dir.
fls fe

# List all empty directories in the current dir, most recent one first.
fls de -t

# Use without filters:
fls           # same as ls
fls -lt ~     # same as ls -lt ~
fls -lt -- pg # same as lf -lt pg; -- unambiguously marks 'pg' as file operand
```

# Installation

**Supported platforms**

* When installing from the **npm registry**: **Linux** and **OSX**
* When installing **manually**: any **Unix-like** platform with **Bash** 

## Installation from the npm registry

With [Node.js](http://nodejs.org/) or [io.js](https://iojs.org/) installed, install [the package](https://www.npmjs.com/package/fls) as follows:

    [sudo] npm install fls -g

**Note**:

* Whether you need `sudo` depends on how you installed Node.js / io.js and whether you've [changed permissions later](https://docs.npmjs.com/getting-started/fixing-npm-permissions); if you get an `EACCES` error, try again with `sudo`.
* The `-g` ensures [_global_ installation](https://docs.npmjs.com/getting-started/installing-npm-packages-globally) and is needed to put `fls` in your system's `$PATH`.

## Manual installation

* Download [this `bash` script](https://raw.githubusercontent.com/mklement0/fls/stable/bin/fls) as `fls`.
* Make it executable with `chmod +x fls`.
* Move it or symlink it to a folder in your `$PATH`, such as `/usr/local/bin` (OSX) or `/usr/bin` (Linux).


# Usage

Find concise usage information below; for complete documentation, read the [manual online](doc/fls.md), or, once installed, run `man fls` (`fls --man` if installed manually).

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```nohighlight
$ fls --help


A type-filtering wrapper around the standard ls utility.

    fls [<filter>] [<options-for-ls>] [<dir>]
    fls [<filter>] [<options-for-ls>] <fileOrDir>...

<filter> is a string of filter characters; commonly used are:

    f       file or symlink to file
    d       dir or symlink to dir
    l       symlink
    x       executable file / searchable dir. (by you)
    e       empty file (zero bytes) or empty dir. (no files or subdirs.)

Filters are combined with logical AND, and filters placed after ^ are negated.  
E.g., fls fx^l lists executable files that aren't symlinks.

Standard options: --help, --man, --version, --home
```

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

Copyright (c) 2015-2020 Michael Klement <mklement0@gmail.com> (http://same2u.net), released under the [MIT license](https://spdx.org/licenses/MIT#licenseText).

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have optional suffixes denoting the type of dependency; the *absence* of a suffix denotes a required *run-time* dependency: `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

* [doctoc (D)](https://github.com/thlorenz/doctoc)
* [json (D)](https://github.com/trentm/json)
* [marked (D)](https://marked.js.org)
* [marked-man (D)](https://github.com/kapouer/marked-man)
* [replace (D)](https://github.com/ALMaclaine/replace)
* [semver (D)](https://github.com/npm/node-semver)
* [tap (D)](http://www.node-tap.org/)
* [urchin (D)](https://git.sdf.org/tlevine/urchin)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

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
