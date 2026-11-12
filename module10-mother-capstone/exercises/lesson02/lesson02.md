# Exercise 10.2 — Persistence and learning pipeline

**Textbook:** [Persistence and learning pipeline](../../lesson02-persistence-and-learning-pipeline.md)  
**Estimated time:** 90–150 minutes

## Part A — Predict

If one held-out CPU changes, should the training mean change? Should the fingerprint change? Explain the different responsibilities.

## Part B — Investigate and build

Implement all four commands from Lesson 10.1. Use the two-device fixture from Exercise 9.3. Assert inserted=10 and unchanged=0, then replay and assert inserted=0 and unchanged=10. Verify the training parameters, expected MAEs, exact-threshold status, and JSON model round-trip. Change one database row in a disposable test and require stale-model rejection. Add a late conflict after a new row and prove both rollback and reopen preserve the previous history.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Why retain raw observations instead of only classifications? Which part of the model artifact proves provenance, and which part determines predictions?

## Submission

Keep written work in `lesson02-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
