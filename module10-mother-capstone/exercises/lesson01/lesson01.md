# Exercise 10.1 — Mother specification and domain

**Textbook:** [Mother specification and domain](../../lesson01-specification-and-domain.md)  
**Estimated time:** 90–150 minutes

## Part A — Predict

Predict which failures should leave observation rows unchanged: invalid JSON, invalid last row, identity conflict, and model-file refusal.

## Part B — Investigate and build

Create `mother/`, `tests/`, `fixtures/`, `README.md`, and `design.md`. Write the exact schemas, command outputs you intend to support, dependency diagram, and acceptance matrix before coding the core. Implement validated records and pure fixed-rule classification first. Add tests for type/range boundaries, equality, no mutation, and independent devices. Record any policy carried forward from earlier modules and resolve differences against this chapter.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Which requirements belong in the domain and which in CLI/storage adapters? Why is a sequence key insufficient to claim wall-clock forecasting?

## Submission

Keep written work in `lesson01-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
