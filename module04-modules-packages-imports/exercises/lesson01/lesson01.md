# Exercise 4.1 — Importing a Helper Module

**Module:** 04 — Modules, Packages, and Imports  
**Lesson:** 4.1 — Modules, Imports, and Execution  
**Difficulty:** Foundational  
**Estimated time:** 30–45 minutes

## Objective

Separate calculation and command-line presentation into two Python files, and predict the effect of importing them.

## Part A — Written predictions

Record answers in `lesson01-answers.md` **before running** the examples.

1. If a module contains a function definition but no function call, does importing it run the function body? Why?
2. If a module contains a top-level `print()`, when can that print execute?
3. How does `import evaluation` differ from `from evaluation import is_high`?
4. Why can `if __name__ == "__main__":` protect a script's execution behavior during ordinary imports?

## Part B — Implementation

Create **empty** files `evaluation.py` and `app.py` yourself, in the same exercise directory.

### Starting data

Use these fixed sample observations (copy them into your implementation as appropriate):

```python
readings = [35, 81, 80, 92]
threshold = 80
```

### Requirements

1. `evaluation.py` must define a reusable function that accepts a reading and a threshold and returns whether the reading is **strictly greater** than the threshold.
2. `app.py` must import and use that function to process every sample reading.
3. `app.py` must print a readable result for each reading, including whether it exceeds the threshold.
4. Importing `app.py` must **not** automatically produce the report; running `app.py` directly must produce it.
5. Keep decision logic in the helper module and presentation in the application module.

### Constraints

- Standard library only. No external packages or classes.
- You may use loops and functions from Module 02.
- Do not put operational printing at the top level of the helper module.
- Do not hardcode the four expected outcomes instead of calculating them.

## Part C — Reasoning

Explain which file owns application execution, which owns the computation, and why importing a helper should not print a report.

## Submission

- `evaluation.py`
- `app.py`
- `lesson01-answers.md`

## Completion checklist

- [ ] I predicted import behavior before executing tests.
- [ ] My application runs from the exercise directory.
- [ ] Importing the application does not run its report.
- [ ] The helper returns a value rather than printing a decision.

**Status:** Not submitted
