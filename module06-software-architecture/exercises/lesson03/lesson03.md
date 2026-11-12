# Exercise 6.3 — Errors and observability

**Textbook:** [Errors and observability](../../lesson03-errors-and-observability.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

Predict what `parse_cpu("eighty")` raises and where its original cause appears. Why is returning zero dangerous?

## Part B — Investigate and build

Create `errors.py`, `parser.py`, and `demo.py`. Parse the explicit inputs "79", "80", "81", "bad", "nan", and "101" independently. Add a record number to each failure and preserve the cause where conversion fails. Configure logging only in `demo.py`; import the helpers and show no output on import. Keep successful classification output on stdout and diagnostics on stderr. Record exit behavior in a short contract.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Which exceptions should a command-line boundary translate? What information makes a diagnostic useful without dumping a whole input file?

## Submission

Keep written work in `lesson03-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
