# Exercise 4.2 — Build a Small Package

**Module:** 04 — Modules, Packages, and Imports  
**Lesson:** 4.2 — Packages, Import Paths, and Project Layout  
**Difficulty:** Foundational–Intermediate  
**Estimated time:** 45–60 minutes

## Objective

Create a Python package that runs reliably by its package name from the exercise project root.

## Part A — Written questions

Record answers in `lesson02-answers.md`.

1. What distinguishes an ordinary package from a standalone module?
2. Why does `python3 -m observer` differ from `python3 observer/__main__.py`?
3. What does the dot mean in `from . import evaluation` when used within `observer`?
4. Why isn't it appropriate to modify `sys.path` just to make a broken exercise import succeed?

## Part B — Implementation

Create the following **empty files** and directories yourself:

```text
lesson02/
└── observer/
    ├── __init__.py
    ├── __main__.py
    ├── readings.py
    └── evaluation.py
```

### Starting data

```python
readings = [60, 77, 84, 95]
threshold = 80
```

### Requirements

1. `readings.py` must provide the sample readings to other modules without printing them on import.
2. `evaluation.py` must expose a function that returns a Boolean for a reading and threshold.
3. `__main__.py` must import from the package and display a readable per-reading classification.
4. `__init__.py` must remain empty.
5. From the `lesson02/` directory, `python3 -m observer` must launch your program.
6. Importing `observer.evaluation` must not start the report.

### Constraints

- Standard-library features only, no third-party dependencies.
- No path modification with `sys.path`.
- No writing outside the exercise directory.
- Use package imports, not filesystem-relative paths masquerading as import names.

## Part C — Design reasoning

Explain why the package entry point should coordinate modules instead of having `evaluation.py` import `__main__.py` to print its results.

## Submission

- Four source files within `observer/`
- `lesson02-answers.md`

## Completion checklist

- [ ] Package launches using `python3 -m observer` from the specified root.
- [ ] Imports don't accidentally start printing.
- [ ] The source files have distinct responsibilities.
- [ ] No manual import-path patches were required.

**Status:** Not submitted
