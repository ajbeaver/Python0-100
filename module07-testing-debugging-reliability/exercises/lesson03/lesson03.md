# Exercise 7.3 — Integration and failure tests

**Textbook:** [Integration and failure tests](../../lesson03-integration-and-failure-tests.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

Would a fake writer prove SQLite rolls back? Predict what happens when two tests share the same fixed filename.

## Part B — Investigate and build

Create `tests/` with `__init__.py`, `test_pipeline.py`, and `test_history_sequences.py` in a project holding your Module 06 package. Add a full in-memory pipeline test, invalid-final-record rejection, and a sequence of at least 20 accepted/rejected history operations checked against an independently maintained expected list. Use a recording fake at the persistence boundary if your service supports one. Show the test command collects the intended cases and passes in a fresh process.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

What can an integration test establish that a formatter unit test cannot? Which later test must replace confidence based only on a fake writer?

## Submission

Keep written work in `lesson03-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
