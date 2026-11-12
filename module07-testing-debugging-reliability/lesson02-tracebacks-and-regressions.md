# Lesson 7.2 — Tracebacks, debugging, and regressions

**Course:** Python 0–100, Edition 2  
**Module:** [07 — Testing, Debugging, and Reliability](README.md)  
**Prerequisite:** Lesson 7.1  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Read traceback frames.
2. Reduce a failing case.
3. Preserve the behavioral fix in a regression test.

## 7.2.1 — Start with a reproducible failure

A traceback is a path through active calls ending with an exception and message. Read the final line to identify the failure kind, then inspect your nearest relevant frame. A `KeyError` identifies a missing key; it does not tell you whether the real defect was bad input, a wrong field name, or an earlier transformation.

```python
def warnings(values):
    high = False
    result = []
    for value in values:
        if value > 80:
            high = True
        result.append(high)
    return result

print(warnings([91, 45]))
```

This program has no traceback because it runs successfully while computing the wrong result. Write the expected list before executing it. The smallest useful case needs one high value followed by one normal value; a list containing only high values hides the defect.

## 7.2.2 — Separate the symptom from the cause

For the stale flag, a hand trace is more informative than a large output dump:

| Iteration | CPU | Comparison | Flag before body | Flag after body |
|---|---:|---|---|---|
| 1 | 91 | True | False | True |
| 2 | 45 | False | True | True |

The second comparison is correct. The defect is that the false branch performs no assignment. This distinction prevents an irrelevant fix to threshold arithmetic when the real problem is state lifetime.

For an exception, inspect the input at the failing frame before changing access syntax. `reading["memory"]` can raise because the schema requires only CPU, because a malformed record missed validation, or because the code misspelled a required key. Substituting `.get("memory", 0)` would suppress the symptom while inventing a zero measurement.

## 7.2.3 — Investigate one hypothesis at a time

Hypothesis: the flag survives beyond the observation it describes. Inspect `high` on both iterations with a temporary print or `breakpoint()`, then test the correction: recompute the flag for each current value and expect `[91, 45] -> [True, False]`. Remove temporary output before delivery.

At `breakpoint()`, `next` advances within the current frame; `step` can enter a called function; `print(expression)` inspects a value; `continue` resumes execution. Place the breakpoint immediately before the point where expectation and observation diverge. The goal is evidence for a hypothesis, not stepping through every line by habit.

A minimal reproduction removes irrelevant components while retaining the failure. Here the two-reading example is enough; a database, logging setup, and CLI would add distractions. Keep this case in the regression suite and run the surrounding suite after the fix.

The original failure evidence belongs in notes, not as a permanently broken production function. Your final code should express the intended per-observation decision. Mother will benefit from a compact history of what failed and which contract the regression protects.

## 7.2.4 — A regression test protects meaning

First show that the test fails with the faulty implementation. Then fix it and show it passes. This establishes that the test can detect that particular bug; it does not prove the absence of every bug. Also check the opposite order and repeated values, because state-related defects often depend on sequence.

For exceptions, retain the traceback during investigation rather than hiding it with a broad handler. For wrong results, preserve input, expected output, actual output, and a minimal reproduction. Mother needs both kinds of evidence.

## Self-review

Why does a program with no exception still need tests? Which evidence distinguishes a parser failure from a stale-state defect?

## Practice

Complete the [exercise specification](exercises/lesson02/lesson02.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson02-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Python reference — pdb debugger](https://docs.python.org/3/library/pdb.html)
