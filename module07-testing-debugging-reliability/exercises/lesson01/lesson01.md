# Exercise 7.1 — Unit tests and fixtures

**Textbook:** [Unit tests and fixtures](../../lesson01-unit-tests-and-fixtures.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

A fixture contains one reading at setup and each of two tests appends one. Predict each length with independent setup versus one shared module list.

## Part B — Investigate and build

Create `test_meter.py` and `test_policy.py` beside copies of your Module 05 implementations. Write at least eight cases spanning successful update, exact threshold, extremes, wrong type, out-of-range rejection with unchanged state, and independent objects. Deliberately change `>` to `>=` and record which test fails, then restore the implementation and rerun. Use `python -m unittest discover -s . -p 'test_*.py' -v`.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Why is comparing an implementation with itself a weak oracle? Which second assertion catches mutation before a rejected update?

## Submission

Keep written work in `lesson01-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
