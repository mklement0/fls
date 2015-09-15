[![npm version](https://img.shields.io/npm/v/fls.svg)](https://npmjs.com/package/fls) [![license](https://img.shields.io/npm/l/fls.svg)](https://github.com/mklement0/fls/blob/master/LICENSE.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [fls &mdash; Introduction](#fls-&mdash-introduction)
- [Examples](#examples)
- [Installation](#installation)
  - [From the npm registry](#from-the-npm-registry)
  - [Manual installation](#manual-installation)
- [Usage](#usage)
- [License](#license)
  - [Acknowledgements](#acknowledgements)
  - [npm dependencies](#npm-dependencies)
- [Changelog](#changelog)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# fls &mdash; Introduction

`fls` &mdash; ***f***iltering ***ls*** &mdash; is a **type-filtering wrapper** for the standard **`ls`** Unix utility.

The general idea is to **enhance `ls` with the ability to filter items by filesystem type** by specifying a filter expression as the first argument.  
A filter expression is composed of **one or more optionally negatable filter characters based on [Bash's file-test operators](http://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html)**, such as `f` for files, `d` for directories, and `L` for symlinks.

Behind the scenes `ls` is ultimately invoked, so all of its options are supported.  
Specifying a filter is optional, so `fls` can generally be used in lieu of `ls`, with a few restrictions detailed in the [usage chapter](#usage).

The following example lists only subdirectories in the current directory, in long format:

    fls d -l

`fls` also lends itself to creating aliases; for instance, the following alias lists all executable (`x`) files (`f`):

* `alias lsbin='fls fx'`

A general `ls` replacement with fixed options, to which you may optionally pass filters as the first operand:

* `alias lsx='fls -FAhl'`

See examples below and complete [usage information](#usage) further below.

# Examples

```shell
# List all files (not directories) in the current dir.
fls f

# List all subdirs. of the user's home dir.
fls d ~

# List all symlinks to hidden files in long format in the user's home dir.
fls Lf -l ~/.*

# List all executable files matching c* in /usr/local/bin
fls xf /usr/local/bin/c*

# List all empty (zero-byte) files in the current dir.
fls f^s

# List all empty directories in the current dir, most recent one first.
fls d^s -t

# Use without filters:
fls        # same as ls
fls -lt ~  # same as ls -lt ~
```

# Installation

**Supported platforms**

* When installing from the **npm registry**: **Linux** and **OSX**
* When installing **manually**: any **Unix-like** platform with **Bash** 

## From the npm registry

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

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```nohighlight
$ fls --help

SYNOPSIS
  fls [filter] [options-for-ls] [dir]
  fls [filter] [options-for-ls] fileOrDir...

DESCRIPTION
  A type-filtering wrapper for the ls utility.

  NOTE: For simplicity, this utility is limited to filtering items from a 
  *single directory*:
   - Either by specifying a single directory (the current one by default)
     whose *content* to filter.
   - Or, as the result of globbing on the command line, by specifying multiple
     items *all from the same parent directory* to filter *themselves*.
     Unlike with ls, matching items are always printed by filename only, with
     a path component, if any, removed.
  Also, to allow use of a single utility in both filtering and non-filtering
  scenarios, specifying a filter is *optional* and not specifying one makes
  fls behave like ls, within the constraints noted.

  options-for-ls
    Options to pass through to ls, such as -l to list in long format.

  dir
    A *single* directory whose items (files and subdirectories) to filter;
    defaults to . (the current directory).
  
  fileOrDir...
    A list of files and/or subdirectories *from a single parent directory*
    to which to apply filtering *directly*.
    Typically, this list will come from a pathname expansion (globbing)
    on the command line.
    Note that, unlike with ls, option -d is implictly for multiple operands; 
    that is, (sub)directories among the operands are always filtered and
    printed as themselves, and their content is neither examined nor printed.
    CAVEAT: If a glob happens to expand to a *single directory*, this utility
    will instead target that directory's *content*, as if a single directory
    had been explicitly specified - it cannot tell the difference.
    When in doubt, use -d explicitly.

  filter
    A string of one or more filter characters, optionally grouped by negation.
    AND logic is implicitly applied to multiple filters; i.e., matching items
    must meet ALL criteria.
    A ^ preceding one or more characters negates their logic; only one ^
    is allowed.
    Specifying just '--' explicitly indicates that *no* filtering should be
    performed at all.

    For convenience and to facilitate definition of aliases, the filter may be
    placed before or after the options to pass to ls, if any.
    A first operand that is not a valid filter is considered a file operand
    instead. If what you intended as a filter is treated as a (non-existent)
    file instead, the implicication is that the filter is invalid; to see the
    specific reason, specify the filter unambiguously as such, by:
     - either: placing it before options; e.g.: fls d -l [...]
     - or: following it with '--'; e.g.: fls -l d -- [...]
    Conversely, to explicitly request unfiltered output, use '--'
    as the first operand; e.g.: fls -- [...]

    Filter characters correspond to *Bash's file-test operators*; common ones
    are listed below; for the full list, see CONDITIONAL EXPRESSIONS in
    `man bash`.

    f, d
      Matches a file / directory; note that for symlinks the type of their
      *target* is matched.
      Caveat: This means that if a symlink points to a non-existing target,
      neither filter will match it; only L by itself will output such symlinks.
    
    x
      Matches an executable file or searchable directory; add f or d to
      distinguish.

    L or h
      Matches a symlink. Add f or d to distinguish between symlinks to
      files and those to directories.

    s
      Matches a nonempty file (nonzero-byte file) or nonempty directory
      Add f or d to distinguish. Note: bash's -s test operator only
      operates meaningfully on files, not directories, but this utility
      extends the "nonempty" semantics to refer to directories that contain
      at least one item (whether hidden or not).

    r, w
      Matches a file or directory that the current user can read, write.

  To filter *hidden* files or directories, use glob .* - this will return only
  the hidden items, to which you can then apply further filtering; e.g.,
    fls f .*  # show hidden files
    fls d .*  # show hidden subdirs.

  Since remembering the filter characters can be a challenge, you can define
  *aliases*; e.g.:
    alias lsd='fls d'     # list directories
    alias lsexe='fls xf'  # list executables
    alias lsln='fls L'    # list symlinks

  The exit code is 0, as long as all file operands exist and can be examined.
  Thus, a filter that matches nothing simply produces no output, without 
  indicating an error condition.

EXAMPLES
    # List all files in the current dir.
  fls f
    # List all files in the current dir in long format, including hidden ones.
  fls f -lA
    # List all hidden files in the current dir.
  fls f .*
    # List all subdirs. of /    
  fls d /
    # List all symlinks to files in the current dir.
  fls Lf
    # List all executable files matching c* in /usr/local/bin
  fls xf /usr/local/bin/c*
    # List all empty (zero-byte) files in the current dir.
  fls f^s
    # List all empty directories in the current dir.
  fls d^s
    # Use without filters:
  fls           # same as ls
  fls -lt ~     # same as ls -lt ~
  fls -lt -- ~  # ditto, explicitly requesting unfiltered output
```

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

Copyright (c) 2015 Michael Klement <mklement0@gmail.com> (http://same2u.net), released under the [MIT license](https://spdx.org/licenses/MIT#licenseText).

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have optional suffixes denoting the type of dependency; the *absence* of a suffix denotes a required *run-time* dependency: `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

* [doctoc (D)](https://github.com/thlorenz/doctoc)
* [json (D)](https://github.com/trentm/json)
* [replace (D)](https://github.com/harthur/replace)
* [semver (D)](https://github.com/npm/node-semver#readme)
* [tap (D)](https://github.com/isaacs/node-tap)
* [urchin (D)](https://github.com/tlevine/urchin)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

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
