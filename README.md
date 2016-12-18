# assert.sh

is a small test-driven development suite for Bash, *(forked from [lehmannro/assert.sh](https://github.com/lehmannro/assert.sh))*

[![language: bash](https://img.shields.io/badge/language-bash-447799.svg?style=flat-square)]()
[![license: LGPLv3](https://img.shields.io/badge/license-LGPLv3-447799.svg?style=flat-square)](https://github.com/joseluis/assert.sh/blob/master/LICENSE)
[![Build Status](https://img.shields.io/travis/joseluis/assert.sh/master.svg)](https://travis-ci.org/joseluis/assert.sh)

---


## Features

- lightweight interface: ``assert`` and ``assert_raises``
- minimal setup -- source ``assert.sh`` and you're done
- test grouping in individual suites
- time benchmarks with real-time display of test progress
- run all tests, stop on first failure, or collect numbers only
- automatically set the exit status of the test script
- skip individual tests


## Example

  . assert.sh

  # `echo test` is expected to write "test" on stdout
  assert "echo test" "test"
  # `seq 3` is expected to print "1", "2" and "3" on different lines
  assert "seq 3" "1\n2\n3"
  # exit code of `true` is expected to be 0
  assert_raises "true"
  # exit code of `false` is expected to be 1
  assert_raises "false" 1
  # end of test suite
  assert_end examples

If you had written the above snippet into ``tests.sh`` you could invoke it
without any extra hassle::

  $ ./tests.sh
  all 4 examples tests passed in 0.014s.

Now, we will add a failing test case to our suite::

  # expect `exit 127` to terminate with code 128
  assert_raises "exit 127" 128

Remember to insert test cases before `assert_end` (or write another
`assert_end` to the end of your file). Otherwise test statistics will be
omitted.

When run, the output is::

  test #5 "exit 127" failed:
          program terminated with code 127 instead of 128
  1 of 5 examples tests failed in 0.019s.

The overall status code is 1 (except if you modified the exit code manually)::

  $ bash tests.sh
  ...
  $ echo $?
  1


## Reference

- `assert <command> [stdout] [stdin]`

  Check for an expected output when running your command. `stdout` supports all
  control sequences ``echo -e`` interprets, eg. ``\n`` for a newline. The
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

  Skip the following test case if `command` exits successfully.  (``skip``
  disclaimer applies.)  Use this if you want to run a test only if some
  precondition is met, eg. the test needs root privileges or network access.


### Command line options

See `assert.sh --help` for command line options on test runners.

  -v, --verbose    Generate real-time output for every individual test run.
  -x, --stop       Stop running tests after the first failure.
                   (Default: run all tests.)
  -i, --invariant  Do not measure runtime for suites. Useful mainly to parse
                   test output.
  -d, --discover   Collect test suites and number of tests only; don't run any
                   tests.
  -c, --continue   Do not modify exit code depending on overall suite status.
  -h               Show brief usage information and exit.
  --help           Show usage manual and exit.


### Environment variables

variable        | corresponding option
--------------- | -------------------
`$DEBUG`        | `--verbose`
`$STOP`         | `--stop`
`$INVARIANT`    | `--invariant`
`$DISCOVERONLY` | `--discover-only`
`$CONTINUE`     | `--continue`

