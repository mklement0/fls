[![npm version](https://badge.fury.io/js/fls.svg)](http://badge.fury.io/js/fls)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [fls &mdash; Introduction](#fls-&mdash-introduction)
- [Installation](#installation)
  - [From the [npm registry](https://www.npmjs.com/package/fls)](#from-the-npm-registryhttpswwwnpmjscompackagefls)
  - [Manual installation](#manual-installation)
- [Examples](#examples)
- [Usage](#usage)
- [License](#license)
  - [Acknowledgements](#acknowledgements)
  - [npm dependencies](#npm-dependencies)
- [Changelog](#changelog)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# fls &mdash; Introduction

A **type-filtering wrapper** for the standard **`ls`** Unix utility (<u>f</u>iltering <u>ls</u>).

The general idea is to **enhance `ls` with the ability to filter items by filesystem** type by specifying a filter expression as the first argument.  
A filter expression is composed of one or more optionally negatable [Bash's file-test operators](http://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html), such as `f` for files, `d` for directories, and `L` for symlinks.

Behind the scenes `ls` is ultimately invoked, so all of its options are supported.  
Specifying a filter is optional, so `fls` can generally be used in lieu of `ls`, with a few restrictions detailed in the [usage chapter](#usage).

`fls` lends itself to creating aliases; for instance, the following alias lists all executable (`x`) files (`f`):

* `alias lsbin='fls fx'`

See [examples](#examples) or complete [usage information](#usage).


# Installation

**Supported platforms**

* When installing from the **npm registry**: **Linux** and **OSX**
* When installing **manually**: any **Unix-like** platform with **Bash** 

## From the [npm registry](https://www.npmjs.com/package/fls)

With [Node.js](http://nodejs.org/) or [io.js](https://iojs.org/) installed, install as follows:

    [sudo] npm install fls -g

**Note**:

* Whether you need `sudo` depends on how you installed Node.js / io.js and whether you've [changed permissions later](https://docs.npmjs.com/getting-started/fixing-npm-permissions); if you get an `EACCES` error, try again with `sudo`.
* The `-g` ensures [_global_ installation](https://docs.npmjs.com/getting-started/installing-npm-packages-globally) and is needed to put `fls` in your system's `$PATH`.

## Manual installation

* Download [this `bash` script](https://raw.githubusercontent.com/mklement0/fls/stable/bin/fls) as `fls`.
* Make it executable with `chmod +x fls`.
* Move it or symlink it to a folder in your `$PATH`, such as `/usr/local/bin` (OSX) or `/usr/bin` (Linux).

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

# Usage

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```
$ fls --help

SYNOPSIS
  fls [filters] [options-for-ls] [dir]
  fls [filters] [options-for-ls] fileOrDir ...

DESCRIPTION
  A type-filtering wrapper for the ls utility.

  NOTE: For simplicity, this utility is limited to filtering items from a 
  *single directory*:
   - Either by specifying a single directory (the current one by default)
     whose *content* to filter.
   - Or, as the result of globbing on the command line, by specifying multiple
     items *all from the same parent directory* to *filter themselves*.
  Also, to allow use of a single utility in both filtering and non-filtering
  scenarios, specifying a filter is *optional* and not specifying one makes
  fls behave like ls, within the constraints noted above.

  options-for-ls
    Options to pass through to ls, such as -l to list in long format.
    Note that *option -d is implictly always used*; that is, subdirectories
    are always filtered and printed as themselves, and their content is 
    neither examined nor printed.

  dir
    A *single* directory whose items (files and subdirectories) to filter;
    defaults to . (the current directory).
  
  fileOrDir ...
    A list of files and/or subdirectories *from a single parent directory*
    to which to apply filtering *directly*.
    Typically, this list will come from a pathname expansion (globbing)
    on the command line.
    CAVEAT: If a glob happens to expand to a *single directory*, this utility
    will instead target that directory's *content*, as if a single directory
    had been explicitly specified - it cannot tell the difference.

  filters
    A string of one or more filter characters, optionally grouped by negation.
    AND logic is implicitly applied to multiple filters; i.e., matching items
    must meet ALL criteria.
    A ^ preceding one or more characters negates their logic; Only one ^
    is allowed.
    Using just a - (hyphen) explicitly indicates that *no* filtering should
    be applied at all.

    Note that the filters argument, if specified, must always come *first*,
    even before any options for ls. This reduces ambiguity and facilitates
    definition of aliases (see below).
    To avoid ambiguity when no options are specified at all, use - as the
    first argument.

    Filter characters correspond to *Bash's file-test operators*; common ones
    are listed below; for the full list, see CONDITIONAL EXPRESSIONS in
    `man bash`.

    f, d
      Matches a file / directory; note that for symlinks the type of their
      *target* is matched.
    
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
  fls        # same as ls
  fls -lt ~  # same as ls -lt ~
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

* **v0.0.2** (2015-06-03):
  * [fix] Simple, but fundamental bug on Linux fixed (my apologies): no longer tries to use `command` with `exec`, which fails on Linux, because `command` is only a builtin (and was never needed to begin with).

* **v0.0.1** (2015-06-03):
  * Initial release.
