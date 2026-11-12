# Lesson 8.3 — Command-line interfaces and project delivery

**Course:** Python 0–100, Edition 2  
**Module:** [08 — Files, Persistence, and System Interfaces](README.md)  
**Prerequisite:** Lesson 8.2  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Build an argparse entry point.
2. Expose testable exit behavior.
3. Document a reproducible standard-library project.

### A CLI is an adapter

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

### Make stdout useful

Return report data on stdout and diagnostics on stderr. Use 0 for success, 2 for invalid input or usage, and 1 for operational failures such as an unwritable database. Argparse handles usage failures itself; translate anticipated service errors at the command boundary. Tests can call `main([...])` directly or use `subprocess.run` with an argument list, captured output, and a timeout. Do not use `shell=True` to assemble user-provided paths.

### The project is more than a working directory

A standard-library application can run from its project root with `python -m mother`. Keep `mother/__init__.py`, `mother/__main__.py`, tests, fixtures, and a README. It need not be installed with pip. If distributing an installable package later, add build metadata and verify installation separately; do not claim root-directory execution proves packaging works.

Record the Python version, commands, input schema, and absence of third-party dependencies. `.venv`, generated databases, caches, and local reports should stay out of source control. A virtual environment helps isolate dependencies, but is not an OS sandbox. Module 10 will use explicit database and input paths in every command.

## Self-review

Why should importing `mother_cli.cli` not parse process arguments? Which command proves a package's entry point, and which claim does it leave untested?

## Practice

Complete the [exercise specification](exercises/lesson03/lesson03.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson03-answers.md` beside it. Then complete the [module assessment](assessment.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/library/argparse.html)
