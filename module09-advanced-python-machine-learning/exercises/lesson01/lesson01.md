# Exercise 9.1 — Iterators, generators, and bounded streams

**Textbook:** [Iterators, generators, and bounded streams](../../lesson01-iterators-and-generators.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

Predict the four print calls and generator-side prints above. Will creating the generator validate all its values?

## Part B — Investigate and build

Create `streams.py` and `test_streams.py`. Implement a generator yielding (sequence, current minus previous) from a strictly increasing sequence of readings for one device; the first reading yields nothing. Test empty, single, three-item, exhausted, and out-of-order inputs. Keep only the previous reading as iteration state. Demonstrate that an out-of-order failure happens when consumed, and explain how a storage caller would preserve batch atomicity.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

When is materializing a bounded batch the simpler choice? Why can catching errors only around generator creation miss validation failures?

## Submission

Keep written work in `lesson01-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
