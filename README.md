# aserta

a handy unit testing framework, in shell script; *originally forked from [assert.sh](https://github.com/lehmannro/assert.sh)*

[![version: 2.0](https://img.shields.io/badge/version-2.0-3D9970.svg?style=flat-square)]()
[![language: bash](https://img.shields.io/badge/language-bash-447799.svg?style=flat-square)]()
[![Build Status](https://img.shields.io/travis/andamira/aserta/master.svg)](https://travis-ci.org/andamira/aserta)
[![Code Climate](https://img.shields.io/codeclimate/github/andamira/aserta.svg)](https://codeclimate.com/github/andamira/aserta)

---

### Table of Contents

- [Features](#features-)
- [Example](#example-)
- [Install](#install-)
- [Reference](#reference-)
  - [Functions](#functions-)
    - [Return Status](#return-status-)
    - [Expected Output](#expected-output-)
    - [Flow Control](#flow-control-)
  - [Options](#options-)

---


## Features [▴](#table-of-contents "Back to TOC")

- minimal setup & lightweight interface
- time benchmarks with real-time display of test progress
- run all tests, stop on first failure, or collect numbers only
- automatically set the exit status of the test script
- test grouping in individual suites
- skip individual tests


## Example [▴](#table-of-contents "Back to TOC")

Write the following snippet into a new file named `my-tests`, for example:

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


## Install [▴](#table-of-contents "Back to TOC")


- Manually

  ```sh
  $ wget https://raw.github.com/andamira/aserta/master/aserta && chmod +x aserta
  ```

- With [bpkg](http://www.bpkg.io/)

  ```sh
  $ bpkg install andamira/aserta # -g
  ```

- With [basher](https://github.com/basherpm/basher)

  ```sh
  $ basher install andamira/aserta
  ```


## Reference [▴](#table-of-contents "Back to TOC")


### Functions [▴](#table-of-contents "Back to TOC")


#### Return Status [▴](#table-of-contents "Back to TOC")

- **`assert_raises`** `<command> [exitcode] [STDIN]`

  Verify the command terminated with the expected status code.
  The default `[exitcode]` is assumed to be 0.

- **`assert_success`** `<command> [STDIN]`

  Verify the command terminated successfully.
  This is equivalent to `assert <command>` when no exit code is specified.

- **`assert_failure`** `<command> [STDIN]`

  Verify the command terminated in failure.


#### Expected Output [▴](#table-of-contents "Back to TOC")

- **`assert`** `<command> [STDOUT] [STDIN]`

  Verify the command output *equals* the output string.

  STDOUT supports all  control sequences `echo -e` interprets, like `\n` for a newline.

   The default STDOUT is assumed to be empty.

- **`assert_contains`** `<command> <expected part of STDOUT>`

  Verify the command output *contains* the expected string.

- **`assert_startswith`** `<command> <expected start to STDOUT>`

  Verify the command output *starts* with the expected string.

- **`assert_endswith`** `<command> <expected end to STDOUT>`

  Verify the command output *ends* with the expected string.

- **`assert_matches`** `<command> <pattern>`

  Verify the command output *matches* the extended REGEXP pattern.


#### Flow Control [▴](#table-of-contents "Back to TOC")

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



### Options [▴](#table-of-contents "Back to TOC")

See `aserta --help` for command line options on test runners.

  command line option   |   ENV variable  | description
----------------------- | --------------- | -------------
`-c`, `--continue`      | `$CONTINUE`     | Don't modify exit code depending on suite status.
`-d`, `--discover-only` | `$DISCOVERONLY` | Collect test suites and number of tests only.
`-i`, `--invariant`     | `$INVARIANT`    | Don't measure runtime. Useful for parsing output.
`-v`, `--verbose`       | `$DEBUG`        | Real-time output for every individual test.
`-x`, `--stop`          | `$STOP`         | Stop running tests after the first failure.


