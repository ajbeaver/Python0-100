# Lesson 7.1 — Unit tests and fixtures

**Course:** Python 0–100, Edition 2  
**Module:** [07 — Testing, Debugging, and Reliability](README.md)  
**Prerequisite:** Module 06  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Write independent unittest cases.
2. Cover success, boundaries, and rejection.
3. Separate expected values from implementation.

## 7.1.1 — A test is an executable expectation

Arrange a known state, perform an action, then assert an observable result. Expected results should come from requirements, not from calling the same helper the implementation uses.

```python
import unittest

def is_high(cpu, threshold):
    return cpu > threshold

class ThresholdTests(unittest.TestCase):
    def test_boundary_is_normal(self):
        self.assertFalse(is_high(80, 80))

    def test_above_boundary_is_high(self):
        self.assertTrue(is_high(81, 80))

if __name__ == "__main__":
    unittest.main()
```

Save this scratch example as `test_threshold.py`. Run `python -m unittest -v test_threshold` from its directory. A test method starts with `test_`. The framework discovers and runs the test methods; the next sections explain their execution and state isolation.

## 7.1.2 — Read the test as ordinary Python

`ThresholdTests` subclasses `unittest.TestCase`, which supplies methods such as `assertFalse` and `assertEqual`. The runner discovers the `test_` methods and calls them. An assertion failure reports a failed expectation; an unexpected exception reports an error. Both make the suite unsuccessful, but they point to different investigation paths.

The main guard supports direct script execution. The command `python -m unittest -v test_threshold` instead imports the named module and lets unittest run its tests. Neither route should require another test to have prepared state first.

## 7.1.3 — Choose examples that distinguish implementations

If the only tested value is 90, both `cpu > 80` and the incorrect `cpu >= 80` pass. The value 80 separates those implementations because the contract says equality is normal. A useful boundary set is 79, 80, and 81; endpoints 0 and 100 check the valid domain's extremes.

For a rejected state change, capture the valid state, invoke the invalid operation inside `assertRaises`, then inspect state afterward. These checks prove separate facts: the operation reports a rejection, and it preserves its previous state. Do not calculate the expected state by running the update method again.

A **fixture** is the initial state and resources a test needs. Fresh instance construction in `setUp` is often sufficient. Copying a reference to one module-global list does not create a fresh fixture, even when each test gets a new test-case object.

Mother's suite will grow with its public contracts. At this point, test policy and state ownership. File parsing and database transactions get their own cases when those boundaries exist; testing an imagined future architecture now would make the suite harder to maintain without proving current behavior.

## 7.1.4 — Rejection must preserve state

For `Meter.set_value`, assert both the exception type and the old value afterward. An exception alone could hide a partial update. Use `with self.assertRaises(ValueError): ...` to scope the one call expected to fail. Keep unrelated setup outside that block so a different failure cannot satisfy the assertion.

## 7.1.5 — Test the public promise

Cover CPU 0, threshold equality, just above threshold, and 100. Include two distinct histories and warning-to-normal sequences. A frozen-record test does not prove the input parser rejects malformed JSON; those are separate boundaries.

Mother's tests should answer useful questions: can one device contaminate another, can invalid input alter history, and can the same batch be evaluated twice without changed results? Avoid assertions about private list names unless that structure is itself a requirement. A refactor preserving behavior should keep these tests meaningful.

## Self-review

Why is comparing an implementation with itself a weak oracle? Which second assertion catches mutation before a rejected update?

## Practice

Complete the [exercise specification](exercises/lesson01/lesson01.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson01-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Python reference — unittest](https://docs.python.org/3/library/unittest.html)
