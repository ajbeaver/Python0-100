# Lesson 7.2 — Tracebacks, debugging, and regressions

**Course:** Python 0–100, Edition 2  
**Module:** [07 — Testing, Debugging, and Reliability](README.md)  
**Prerequisite:** Lesson 7.1  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Read traceback frames.
2. Reduce a failing case.
3. Preserve the behavioral fix in a regression test.

### Start with a reproducible failure

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

### State a hypothesis and change one thing

Hypothesis: the flag survives beyond the observation it describes. Inspect `high` on both iterations. You can insert a temporary print or use `breakpoint()`, then `next`, `print(high)`, and `continue` in the debugger. Remove temporary output before delivery. Recompute the flag for each current value, and add a test for `[91, 45] -> [True, False]`.

### A regression test protects meaning

First show that the test fails with the faulty implementation. Then fix it and show it passes. This establishes that the test can detect that particular bug; it does not prove the absence of every bug. Also check the opposite order and repeated values, because state-related defects often depend on sequence.

For exceptions, retain the traceback during investigation rather than hiding it with a broad handler. For wrong results, preserve input, expected output, actual output, and a minimal reproduction. Mother needs both kinds of evidence.

## Self-review

Why does a program with no exception still need tests? Which evidence distinguishes a parser failure from a stale-state defect?

## Practice

Complete the [exercise specification](exercises/lesson02/lesson02.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson02-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/library/pdb.html)
