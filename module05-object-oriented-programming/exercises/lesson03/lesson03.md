# Exercise 5.3 — Interfaces and polymorphism

**Textbook:** [Interfaces and polymorphism](../../lesson03-interfaces-and-polymorphism.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

Predict the three Booleans in the example. What would change if the policy used `>=`?

## Part B — Investigate and build

Create `policies.py` and `demo.py`. Implement `FixedLimit` and `BandLimit`, the latter high when a value lies strictly outside an inclusive [lower, upper] normal band. Validate configuration before use. Evaluate the same [20, 50, 80] through both policies using one evaluator. Demonstrate exact-boundary cases, bad configuration, and a fake policy for orchestration. Document the common value and return contracts.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

When would a function argument be simpler? Which assumptions would a policy returning "normal" instead of `False` break?

## Submission

Keep written work in `lesson03-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
