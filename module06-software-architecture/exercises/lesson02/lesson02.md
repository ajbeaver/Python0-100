# Exercise 6.2 — Dependency injection and configuration

**Textbook:** [Dependency injection and configuration](../../lesson02-dependency-injection.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

A source returns a different reading on each call. What happens if assessment calls it once for a count and again for results?

## Part B — Investigate and build

Create `wiring.py` and `demo.py`. Reuse or recreate the previous domain model and policy. Build two source functions returning different fixed batches. Run both through one assessment function with injected policy and source. Add a source call counter and show exactly one call per run. Implement a validated configuration record with default threshold 80 and optional explicit override. Show rejection before invoking a source when configuration is invalid.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Why should a classifier not discover its own database path? Where should configuration precedence be visible?

## Submission

Keep written work in `lesson02-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
