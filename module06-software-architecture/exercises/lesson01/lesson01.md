# Exercise 6.1 — Contracts and boundaries

**Textbook:** [Contracts and boundaries](../../lesson01-contracts-and-boundaries.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

A formatter imports a service, and the service imports the formatter. Draw the cycle and predict an initialization failure it could cause.

## Part B — Investigate and build

Build a `mother_pipeline` package with `domain.py`, `service.py`, `formatting.py`, and a guarded entry point. Supply three fixed readings with CPU 79, 80, and 81 and threshold 80. Return results in input order; print only at the entry point. Validate the whole supplied list before producing results. Demonstrate repeatability and unchanged inputs, then document each module's imports and contract.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Which module should change for a new output format? Why must batch failure behavior be decided before storage is added?

## Submission

Keep written work in `lesson01-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
