# Exercise 5.2 — Data models and composition

**Textbook:** [Data models and composition](../../lesson02-data-models-and-composition.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

Predict equality for two separately constructed identical records. Would a frozen record containing a list prevent `record.values.append(1)`?

## Part B — Investigate and build

Create `models.py` and `history.py`. Define a frozen `Reading(device, sequence, cpu)` with integer sequence >= 0, a nonempty string device, and a finite numeric CPU between 0 and 100; reject booleans as numbers. Use `math.isfinite`. Implement `History.add` and `snapshot`. Demonstrate separate histories, identical-value equality, rejected malformed records, and the inability to alter the stored collection through its snapshot. Keep source observations unchanged.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Where should validation happen if JSON input has annotations attached nowhere? Why is history not a subclass of a reading?

## Submission

Keep written work in `lesson02-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
