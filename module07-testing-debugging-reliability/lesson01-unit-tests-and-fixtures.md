# Lesson 7.1 — Unit tests and fixtures

**Course:** Python 0–100, Edition 2  
**Module:** [07 — Testing, Debugging, and Reliability](README.md)  
**Prerequisite:** Module 06  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Write independent unittest cases.
2. Cover success, boundaries, and rejection.
3. Separate expected values from implementation.

### A test is an executable expectation

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

Save this scratch example as `test_threshold.py`. Run `python -m unittest -v test_threshold` from its directory. A test method starts with `test_`. The framework creates separate test instances; `setUp` can construct a fresh fixture before each test. Tests must not rely on another test running first.

### Rejection must preserve state

For `Meter.set_value`, assert both the exception type and the old value afterward. An exception alone could hide a partial update. Use `with self.assertRaises(ValueError): ...` to scope the one call expected to fail. Keep unrelated setup outside that block so a different failure cannot satisfy the assertion.

### Test the public promise

Cover CPU 0, threshold equality, just above threshold, and 100. Include two distinct histories and warning-to-normal sequences. A frozen-record test does not prove the input parser rejects malformed JSON; those are separate boundaries.

Mother's tests should answer useful questions: can one device contaminate another, can invalid input alter history, and can the same batch be evaluated twice without changed results? Avoid assertions about private list names unless that structure is itself a requirement. A refactor preserving behavior should keep these tests meaningful.

## Self-review

Why is comparing an implementation with itself a weak oracle? Which second assertion catches mutation before a rejected update?

## Practice

Complete the [exercise specification](exercises/lesson01/lesson01.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson01-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/library/unittest.html)
