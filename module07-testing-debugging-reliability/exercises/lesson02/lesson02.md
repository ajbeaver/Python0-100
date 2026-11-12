# Exercise 7.2 — Tracebacks, debugging, and regressions

**Textbook:** [Tracebacks, debugging, and regressions](../../lesson02-tracebacks-and-regressions.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

Predict output for [91, 45], [45, 91], and [45, 45]. Which sequence best reveals stale state?

## Part B — Investigate and build

Create `warnings.py`, `test_warnings.py`, and `debug-notes.md`. Reproduce the supplied bug, reduce it to two inputs, and add a failing regression test before fixing it. Add reverse-order, empty, and repeated-value cases. Separately reproduce a `KeyError` by reading `{"cpu": 72}["memory"]`; record the traceback's relevant frame and explain why the fix depends on the input contract. Do not hide missing required fields with a default zero.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Why does a program with no exception still need tests? Which evidence distinguishes a parser failure from a stale-state defect?

## Submission

Keep written work in `lesson02-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
