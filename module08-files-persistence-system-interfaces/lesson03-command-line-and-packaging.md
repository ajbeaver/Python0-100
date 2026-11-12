# Lesson 8.3 — Command-line interfaces and project delivery

**Course:** Python 0–100, Edition 2  
**Module:** [08 — Files, Persistence, and System Interfaces](README.md)  
**Prerequisite:** Lesson 8.2  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Build an argparse entry point.
2. Expose testable exit behavior.
3. Document a reproducible standard-library project.

## 8.3.1 — A CLI is an adapter

The command line translates text options into a service call. It should not contain a second copy of classification rules. Keep parser construction and `main(argv=None)` in a CLI module, then have `__main__.py` call `raise SystemExit(main())` behind a main guard.

```python
import argparse

def build_parser():
    parser = argparse.ArgumentParser(description="Review local observations")
    parser.add_argument("--threshold", type=float, default=80.0)
    parser.add_argument("input")
    return parser
```

`type=float` converts syntax; domain validation must still reject NaN, infinity, and out-of-range thresholds. `--help` should show usage and exit successfully without reading input or creating output. Missing required arguments are usage errors.

## 8.3.2 — Follow an argument into the service

With the example parser, `parse_args(["sample.json", "--threshold", "81"])` returns a namespace whose `input` attribute is text and whose `threshold` is the float 81.0. Passing a list makes parsing testable without changing global process arguments. With no explicit list, argparse uses the current process's arguments.

The CLI should next validate the effective threshold and call a service with that valid number and an explicit path. Constructing a parser and parsing options should not open the database. This ordering explains why `--help` and a bad threshold can finish before resource creation.

## 8.3.3 — Make stdout useful

Return report data on stdout and diagnostics on stderr. Use 0 for success, 2 for invalid input or usage, and 1 for operational failures such as an unwritable database. Argparse handles usage failures itself; translate anticipated service errors at the command boundary. Tests can call `main([...])` directly or use `subprocess.run` with an argument list, captured output, and a timeout. Do not use `shell=True` to assemble user-provided paths.

## 8.3.4 — Exit status is another return channel

`main` can return an integer describing success or failure. `raise SystemExit(main())` translates that return into the process's exit status. Printing `"error"` alone does not make a process fail; returning 0 after printing it tells a script that the operation succeeded.

Stdout and stderr are separate streams. JSON on stdout remains parseable when diagnostics go to stderr. A subprocess test should assert all three observable outputs: stdout content, stderr content, and return code. It should also inspect any relevant filesystem effects, because the right status does not prove a failed operation left no rows behind.

## 8.3.5 — Launch commands preserve argument boundaries

A Python subprocess call can use a list such as `[sys.executable, "-m", "mother_cli", "check", str(path)]`. Each element is one argument, even when a path contains spaces. `sys.executable` selects the interpreter running the test. An interpolated shell command introduces another parser, quoting rules, and possible shell interpretation without helping this task.

Before handing off the project, reproduce commands from the documented root. Package execution, a virtual environment, and an installable distribution are separate concepts. This course requires the first two; adding a packaging toolchain before a distribution requirement appears would add setup work without improving the current acceptance test.

## 8.3.6 — The project is more than a working directory

A standard-library application can run from its project root with `python -m mother`. Keep `mother/__init__.py`, `mother/__main__.py`, tests, fixtures, and a README. It need not be installed with pip. If distributing an installable package later, add build metadata and verify installation separately; do not claim root-directory execution proves packaging works.

Record the Python version, commands, input schema, and absence of third-party dependencies. `.venv`, generated databases, caches, and local reports should stay out of source control. A virtual environment helps isolate dependencies, but is not an OS sandbox. Module 10 will use explicit database and input paths in every command.

## Self-review

Why should importing `mother_cli.cli` not parse process arguments? Which command proves a package's entry point, and which claim does it leave untested?

## Practice

Complete the [exercise specification](exercises/lesson03/lesson03.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson03-answers.md` beside it. Then complete the [module assessment](assessment.md).

## Further reading

- [Python reference — argparse](https://docs.python.org/3/library/argparse.html)
