# Lesson 7.3 — Integration and failure tests

**Course:** Python 0–100, Edition 2  
**Module:** [07 — Testing, Debugging, and Reliability](README.md)  
**Prerequisite:** Lesson 7.2  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Test boundaries together.
2. Isolate files and databases.
3. Exercise failure paths and invariants.

### Components can pass alone and fail together

A parser might return a string while the evaluator expects a number. An integration test exercises their actual connection. Keep unit tests for narrow diagnosis, then add a few tests proving the assembled path works.

For the package from Module 06, pass a fixed source through validation, classification, and formatting. Independently specify the expected ordered results. Exercise invalid input at the end of a batch and confirm the whole batch is rejected. No file or database should be required yet.

### Resources must belong to a test

Module 08 will introduce disk I/O. `tempfile.TemporaryDirectory()` gives each test a disposable directory, and cleanup occurs when its context exits. Never use a real user database as a fixture. Use `pathlib.Path(directory) / "history.sqlite"` to construct a temporary path.

A fake writer that records calls can show whether the service attempted a write. A real temporary database test later establishes whether transaction handling actually rolls back. A fake cannot prove database semantics.

### Invariants cover many sequences

A history snapshot should never shrink after a successful append. A rejected reading should never increase its length. Independent histories should never share additions. Exercise a bounded sequence with `subTest` or a fixed-seed local `random.Random(0)` generator and compare with a simple independent expected list.

Random testing supplies examples, not a proof. Deterministic boundary tests remain essential. A reproducible seed and recorded failing input let another person investigate. Mother’s reliability case combines narrow tests, sequence tests, and later a clean command-line run from an empty database.

## Self-review

What can an integration test establish that a formatter unit test cannot? Which later test must replace confidence based only on a fake writer?

## Practice

Complete the [exercise specification](exercises/lesson03/lesson03.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson03-answers.md` beside it. Then complete the [module assessment](assessment.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/library/tempfile.html)
