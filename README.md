# aserta

a handy unit testing framework; *originally forked from [assert.sh](https://github.com/lehmannro/assert.sh)*

[![language: bash](https://img.shields.io/badge/language-bash-447799.svg?style=flat-square "made in Bash")]()
[![version: stable](https://img.shields.io/github/tag/andamira/aserta.svg?label=stable+version&style=flat-square "stable version")](https://github.com/andamira/aserta/commits/stable)
[![Build Status: stable](https://img.shields.io/travis/andamira/aserta/stable.svg?label=stable "stable build status")](https://travis-ci.org/andamira/aserta/branches)
[![Build Status: master](https://img.shields.io/travis/andamira/aserta/master.svg?label=master "master build status")](https://travis-ci.org/andamira/aserta/)
[![Code Climate](https://img.shields.io/codeclimate/github/andamira/aserta.svg)](https://codeclimate.com/github/andamira/aserta)

---

### Table of Contents

- [Features](#features-)
- [Examples](#examples-)
  - [Sourcing the script](#sourcing-the-script-)
  - [Running a single test](#running-a-single-test-)
- [Install](#install-)
  - [Mac OS X](#mac-os-x-)
- [Reference](#reference-)
  - [Functions](#functions-)
    - [Return Status](#return-status-)
    - [Expected Output](#expected-output-)
    - [String comparison](#string-comparison-)
    - [Flow Control](#flow-control-)
  - [Options](#options-)
- [License](#license-)

---


## Features [↑](#table-of-contents "Back to TOC")

- minimal setup & lightweight interface
- time benchmarks with real-time display of test progress
- run all tests, stop on first failure, or collect numbers only
- automatically set the exit status of the test script
- test grouping in individual suites
- skip individual tests


## Examples [↑](#table-of-contents "Back to TOC")

### Sourcing the script [↑](#table-of-contents "Back to TOC")

Write the following snippet into a new file named `my-tests`,
in the same directory as the `aserta` script is located.

```sh
#!/usr/bin/env bash

source aserta

# Command exit codes
assert_success    "true"
assert_failure    "false"
assert_raises     "unknown_cmd" 127

# Expected output
assert            "echo test"    "test"
assert            "seq 3"        "1\n2\n3"
assert_contains   "echo foobar"  "oba"
assert_startswith "echo foobar"  "foo"
assert_endswith   "echo foobar"  "bar"
assert_matches    "echo foobar"  "^f.*r$"

assert_end "example"
```

Run it to check that it passes:

```sh
$ bash my-tests
all 9 example tests passed in 0.090s.
```

Now make sure the test fails by changing, for example, the parameter of the first test, from `true` to `false`:

```sh
assert_success "false"
```

Run it again to check how it fails now:

```
$ bash my-tests
test #1 "false" failed:
        program terminated with code 1 instead of 0
1 of 9 example tests failed in 0.089s.
```

The default error status code is 1:

```sh
$ echo $?
1
```

### Running a single test [↑](#table-of-contents "Back to TOC")

It is possible to run a single test without sourcing the script,
just by passing the desired test function as an argument to the script
just after the script options, and then the test function arguments. E.g.:

```
$ aserta --verbose assert_raises unknown-command 127
.
```

This way of running tests has several disadvantages:

- Only one test can be run on each call.
- Can't show test suite summary statistics.
- Doesn't support: `skip`, `skip_if` and `assert_end`.

To run a test function without sourcing the script,
first make sure the script is executable:

```
$ chmod +x aserta
```

And remember to provide the option -v (--verbose) or the option -x
(--stop) if you want to see the result of the test.
Otherwise it wont have any effect.

```
$ ./aserta -v assert_success true
.

$ ./aserta -v assert_success false
X
```

```
$ ./aserta -x assert_success true

$ ./aserta -x assert_success false
test #1 "false" failed:
        program terminated with code 1 instead of 0
```


## Install [↑](#table-of-contents "Back to TOC")

- Manually

  The latest stable version:

  ```sh
  $ wget git.io/aserta.stable -O aserta && chmod +x aserta
  ```

  The bleeding edge version (`master`):

  ```sh
  $ wget git.io/aserta && chmod +x aserta
  ```

- With [bpkg](http://www.bpkg.io/)

  ```sh
  $ bpkg install andamira/aserta # -g
  ```

- With [basher](https://github.com/basherpm/basher)

  ```sh
  $ basher install andamira/aserta
  ```

### Mac OS X [↑](#table-of-contents "Back to TOC")

The script should work as it is in a Mac OS X system.

But it is encouraged to install the GNU version of coreutils and grep
in order to achieve greater speeds in tests with regular expressions,
and to be able to measure time in milliseconds, instead of full seconds.

A very easy way to install them is by using [Brew](http://brew.sh/):

```
$ brew tap homebrew/dupes
$ brew install coreutils grep
```


## Reference [↑](#table-of-contents "Back to TOC")


### Functions [↑](#table-of-contents "Back to TOC")


#### Return Status [↑](#table-of-contents "Back to TOC")

- **`assert_raises`** `<command> [exitcode] [STDIN]`

  Verify the command terminated with the expected status code.
  The default `[exitcode]` is assumed to be 0.

- **`assert_success`** `<command> [STDIN]`

  Verify the command terminated successfully.
  This is equivalent to `assert <command>` when no exit code is specified.

- **`assert_failure`** `<command> [STDIN]`

  Verify the command terminated in failure.


#### Expected Output [↑](#table-of-contents "Back to TOC")

- **`assert`** `<command> [STDOUT] [STDIN]`

  Verify the command output *equals* the output string.

  STDOUT supports all  control sequences `echo -e` interprets, like `\n` for a newline.

   The default STDOUT is assumed to be empty.

- **`assert_startswith`** `<command> <expected start of STDOUT>`

  Verify the command output *starts* with the expected string.

- **`assert_endswith`** `<command> <expected end of STDOUT>`

  Verify the command output *ends* with the expected string.

- **`assert_contains`** `<command> <expected part of STDOUT>`

  Verify the command output *contains* the expected string.

- **`assert_NOTcontains`** `<command> <not expected part of STDOUT>`

  Verify the command output does *not contain* the expected string.

- **`assert_matches`** `<command> <expected matching pattern>`

  Verify the command output *matches* the extended REGEXP pattern.


#### String Comparison [↑](#table-of-contents "Back to TOC")

- **`assert_str_equals`** `<string> <expected string>`

  Verify the first string *equals* the second string.

- **`assert_str_NOTequals`** `<string> <not expected string>`

  Verify the first string does *not equal* the second string.

- **`assert_str_startswith`** `<string> <expected start of string>`

  Verify the first string *starts* with the second string.

- **`assert_str_endswith`** `<string> <expected end of string>`

  Verify the first string *ends* with the second string.

- **`assert_str_contains`** `<string> <expected part of string content>`

  Verify the first string *contains* the second string.

- **`assert_str_NOTcontains`** `<string> <not expected part of string content>`

  Verify the first string does *not contain* the second string.

- **`assert_str_matches`** `<string> <expected matching pattern>`

  Verify the first string *matches* the extended REGEXP pattern.


#### Flow Control [↑](#table-of-contents "Back to TOC")

- **`assert_end`** `[suite]`

  Finalize the current test suite and print statistics.

- **`skip`**

  Unconditionally skip the following test case.

  The skipped test case is *exempt* from any test diagnostics,
  and not accounted for in the total number of tests.

- **`skip_if`** `<command>`

  Skip the following test case if the command exits successfully.

  Use this if you want to run a test only if some precondition is met.
  E.g. if the test needs root privileges or network access.


### Options [↑](#table-of-contents "Back to TOC")

See `aserta --help` for command line options on test runners.

command line option |   ENV variable  | description
------------------- | --------------- | -------------
`-c`, `--continue`  | `$CONTINUE`     | Don't modify exit code depending on suite status.
`-d`, `--discover`  | `$DISCOVERONLY` | Collect test suites and number of tests only.
`-i`, `--invariant` | `$INVARIANT`    | Don't measure runtime. Useful for parsing output.
`-v`, `--verbose`   | `$DEBUG`        | Real-time output for every individual test.
`-x`, `--stop`      | `$STOP`         | Stop running tests after the first failure.


### License [↑](#table-of-contents "Back to TOC")

- The work previous to the fork of `assert.sh` was released under the LGPLv3 license.
  - View [COPYING](https://github.com/andamira/aserta/blob/master/COPYING)
   and [COPYING.LESSER](https://github.com/andamira/aserta/blob/master/COPYING.LESSER)
- The new work in the current project `aserta`, is released under the MIT license.
  - View [LICENSE.md](https://github.com/andamira/aserta/blob/master/LICENSE.md)

