# Lesson 7.3 — Integration and failure tests

**Course:** Python 0–100, Edition 2  
**Module:** [07 — Testing, Debugging, and Reliability](README.md)  
**Prerequisite:** Lesson 7.2  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Test boundaries together.
2. Isolate files and databases.
3. Exercise failure paths and invariants.

## 7.3.1 — Components can pass alone and fail together

A parser might return a string while the evaluator expects a number. An integration test exercises their actual connection. Keep unit tests for narrow diagnosis, then add a few tests proving the assembled path works.

For the package from Module 06, pass a fixed source through validation, classification, and formatting. Independently specify the expected ordered results. Exercise invalid input at the end of a batch and confirm the whole batch is rejected. No file or database should be required yet.

## 7.3.2 — Give every test an independent oracle

An **oracle** is how a test determines the expected answer. For an integrated three-reading batch, an explicit expected list derived from [79, 80, 81] is useful. Calling the same classifier to construct both expected and actual output would reproduce its mistakes on both sides.

A recording fake can be a plain object whose method appends received arguments to a list. It lets a test ask whether the service attempted a write and which values it supplied. It should implement the same boundary contract as the real writer, including failure behavior the test needs. It need not imitate an entire database.

## 7.3.3 — Resource ownership and cleanup

Module 08 will introduce disk I/O. Use disposable test resources rather than a real user database. The following example shows how `tempfile.TemporaryDirectory()` owns a directory for one test operation.

The recording fake establishes service-level behavior. A real temporary database test in Module 08 will establish whether a database transaction actually rolls back; keep that future integration claim separate.

A context manager has an entry and exit lifecycle. The `with` statement arranges cleanup when execution leaves its body, including when an exception escapes. You do not need to implement that protocol to use a standard-library context manager.

```python
from pathlib import Path
from tempfile import TemporaryDirectory

with TemporaryDirectory() as directory:
    test_path = Path(directory) / "sample.txt"
    print(test_path.parent.exists())
print(Path(directory).exists())
```

This prints `True` then `False`. Constructing the path does not create the file; the directory itself exists only for the context's lifetime. In Module 08, use the same path construction with `"history.sqlite"` for a real temporary database test.

## 7.3.4 — Invariants cover many sequences

A history snapshot should never shrink after a successful append. A rejected reading should never increase its length. Independent histories should never share additions. Exercise a bounded sequence, optionally generated with a fixed-seed local `random.Random(0)`, and compare with a simple independent expected list.

Random testing supplies examples, not a proof, so retain deterministic boundary cases. A sequence failure needs the more specific evidence described next.

## 7.3.5 — Read a sequence failure precisely

Suppose a history test fails after the twelfth operation. Retain the initial state, operation sequence, and expected snapshot at that point. A seed can reproduce generated choices, but the concrete sequence is easier to inspect and remains useful if the generator changes later.

`self.subTest(...)` can label each operation so a failing assertion identifies its inputs. It does not isolate shared state automatically; that remains the fixture's responsibility. Sequence tests complement narrow tests because a system can behave correctly on an isolated operation while carrying stale state into the next one.

## Self-review

What can an integration test establish that a formatter unit test cannot? Which later test must replace confidence based only on a fake writer?

## Practice

Complete the [exercise specification](exercises/lesson03/lesson03.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson03-answers.md` beside it. Then complete the [module assessment](assessment.md).

## Further reading

- [Python reference — Temporary files and directories](https://docs.python.org/3/library/tempfile.html)
