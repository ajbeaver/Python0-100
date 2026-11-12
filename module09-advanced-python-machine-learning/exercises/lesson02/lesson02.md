# Exercise 9.2 — Fitting a predictive baseline

**Textbook:** [Fitting a predictive baseline](../../lesson02-fitting-a-baseline.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

Fit node01 on [20, 30, 40]. Predict residuals for 45 and 80 with tolerance 15, including the equality case.

## Part B — Investigate and build

Create `baseline.py`, `model_io.py`, and tests. Fit device means from supplied validated records, predict CPU, and classify residuals with strict `> tolerance`. Round-trip a schema-versioned JSON model without changing predictions. Test two devices, empty training, unknown device, invalid numeric fields, unsupported version, and malformed artifacts. Use tolerance 15. Record parameters learned from the data separately from configuration selected by the developer.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Why is a mean predictor learning while a fixed threshold alone is not? Which evidence would reveal that inference accidentally updates its model?

## Submission

Keep written work in `lesson02-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
