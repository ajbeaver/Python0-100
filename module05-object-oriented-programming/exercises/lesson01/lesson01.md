# Exercise 5.1 — Classes, instances, and invariants

**Textbook:** [Classes, instances, and invariants](../../lesson01-classes-and-invariants.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

Predict what two instances see after appending to a list defined in their class body. Contrast a list created in each initializer.

## Part B — Investigate and build

Create `meter.py` with a `Meter` class using the contract above, adding a read-only-by-convention `value()` method. In `demo.py`, construct meters at 0 and 100, update one to 81, then attempt -1, 101, and `True`. Catch each expected exception individually so the demonstration continues. Show that rejected changes preserve the last valid reading and that the second instance is independent. Record exception types and before/after values.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Why is validation after assignment too late? Which restriction would a leading underscore fail to enforce?

## Submission

Keep written work in `lesson01-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
