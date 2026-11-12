# Exercise 8.1 — Files, JSON, and validation

**Textbook:** [Files, JSON, and validation](../../lesson01-files-json-and-validation.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

What types result from JSON `80`, `"80"`, `true`, and `null`? Which values meet the schema?

## Part B — Investigate and build

Create `json_input.py`, `valid.json`, and `test_json_input.py`. Use valid readings [("node01", 0, 20), ("node01", 1, 80), ("node02", 0, 81)] represented as JSON objects. Test missing file, malformed JSON, wrong top-level shape, missing/extra fields, duplicate field names, duplicate identities, booleans, numeric strings, NaN, and invalid final row. Return validated records only after the whole batch passes; assert that a recording writer is never called for any invalid file.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Why are duplicate keys and duplicate identities different? Why should path errors preserve a failing exit status?

## Submission

Keep written work in `lesson01-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
