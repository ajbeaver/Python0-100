# Exercise 9.3 — Evaluation, leakage, and model limits

**Textbook:** [Evaluation, leakage, and model limits](../../lesson03-evaluation-and-leakage.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

Compute the table's two validation and test errors before running code. Would fitting on all five values alter the apparent test error?

## Part B — Investigate and build

Create `evaluation.py` and `test_evaluation.py`. For node01 use sequences 0–4 with CPU [20, 30, 40, 50, 60]; for node02 use [60, 60, 60, 60, 60]. Implement the split and both predictors, report per-device and pooled MAE separately for validation and test. Expected pooled mean MAE is 10 on validation and 15 on test; persistence gives 5 and 10. Prove the fitted parameters do not change when only held-out CPU values change. Reject fewer than five readings for any included device.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Why does a random split misrepresent forecasting on drifting observations? Why can residual flags not supply anomaly accuracy without independent labels?

## Submission

Keep written work in `lesson03-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
