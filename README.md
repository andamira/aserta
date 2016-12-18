# assert.sh

is a small test-driven development suite for Bash, *(forked from [lehmannro/assert.sh](https://github.com/lehmannro/assert.sh))*

[![version: 1.2.X](https://img.shields.io/badge/version-1.2.X-3D9970.svg?style=flat-square)]()
[![language: bash](https://img.shields.io/badge/language-bash-447799.svg?style=flat-square)]()
[![license: LGPLv3](https://img.shields.io/badge/license-LGPLv3-447799.svg?style=flat-square)](https://github.com/joseluis/assert.sh/blob/master/LICENSE)
[![Build Status](https://img.shields.io/travis/joseluis/assert.sh/master.svg)](https://travis-ci.org/joseluis/assert.sh)
[![Code Climate](https://img.shields.io/codeclimate/github/joseluis/assert.sh.svg)](https://codeclimate.com/github/joseluis/assert.sh)

---

### Table of Contents

- [Features](#features)
- [Example](#example)
- [Reference](#reference)
  - [Functions](#functions)
  - [Options & variables](#options--variables)

---


## Features

- lightweight interface: `assert` and `assert_raises``
- minimal setup -- source `assert.sh` and you're done
- test grouping in individual suites
- time benchmarks with real-time display of test progress
- run all tests, stop on first failure, or collect numbers only
- automatically set the exit status of the test script
- skip individual tests


## Example

```sh
source assert.sh          # source this library

assert "echo test" "test" # `echo test` is expected to write `test` on stdout
assert "seq 3" "1\n2\n3"  # `seq 3` to print `1`, `2` & `3` on different lines
assert_raises "true"      # exit code of `true` is expected to be 0
assert_raises "false" 1   # exit code of `false` is expected to be 1

assert_end "examples"     # end of the test suite
```

If you had written the above snippet into `tests.sh`, and made it executable,
you could invoke it without any extra hassle:

```sh
$ ./tests.sh
all 4 examples tests passed in 0.014s.
```

Now, we will add a failing test case to our suite:

```sh
assert_raises "exit 127" 128    # expect `exit 127` to terminate with code 128
```

Remember to insert test cases before `assert_end` (or write another
`assert_end` to the end of your file). Otherwise test statistics will be
omitted.

When run, the output is:

```sh
test #5 "exit 127" failed:
        program terminated with code 127 instead of 128
1 of 5 examples tests failed in 0.019s.
```

The overall status code is 1 (except if you modified the exit code manually):

```sh
$ bash tests.sh
...
$ echo $?
1
```


## Reference



### Functions

- `assert <command> [stdout] [stdin]`

  Check for an expected output when running your command. `stdout` supports all
  control sequences `echo -e` interprets, eg. `\n` for a newline. The
  default `stdout` is assumed to be empty.

- `assert_raises <command> [exitcode] [stdin]`

  Verify `command` terminated with the expected status code. The default
  `exitcode` is assumed to be 0.

- `assert_end [suite]`

  Finalize a test suite and print statistics.

- `skip`

  Unconditionally skip the following test case.  The skipped test case is
  *exempt* from any test diagnostics (ie., not accounted for in the total
  number of tests.)

- `skip_if <command>`

  Skip the following test case if `command` exits successfully.  (`skip`
  disclaimer applies.)  Use this if you want to run a test only if some
  precondition is met, eg. the test needs root privileges or network access.

- Extra wrappers for convenience:

  - `assert_success <command> [stdin]`

    Verify `command` terminated successfully.

  - `assert_failure<command> [stdin]`

    Verify `command` terminated with failure.

  - `assert_contains <command> <expected output...>`


  - `assert_matches <command> <expected output...>`


  - `assert_startswith <command> <expected start to stdout>`


  - `assert_endswith <command> <expected end to stdout>`


  - `_assert_with_grep <grep modifiers> <command> <expected output...>`



### Options & Variables

See `assert.sh --help` for command line options on test runners.

env variable    | corresponding CLI option | information
--------------- | ------------------------ | ------------
`$DEBUG`        | `-v`, `--verbose`        | Real-time output for every individual test.
`$STOP`         | `-x`, `--stop`           | Stop running tests after the first failure.
`$INVARIANT`    | `-i`, `--invariant`      | Don't measure runtime. Useful for parsing output.
`$DISCOVERONLY` | `-d`, `--discover-only`  | Collect test suites and number of tests only.
`$CONTINUE`     | `-c`, `--continue`       | Don't modify exit code depending on suite status.

