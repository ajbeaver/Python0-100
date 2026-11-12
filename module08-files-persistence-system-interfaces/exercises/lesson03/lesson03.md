# Exercise 8.3 — Command-line interfaces and project delivery

**Textbook:** [Command-line interfaces and project delivery](../../lesson03-command-line-and-packaging.md)  
**Estimated time:** 40–75 minutes

## Part A — Predict

Will `--threshold nan` pass float conversion? What must reject it before any output path is opened?

## Part B — Investigate and build

Create a `mother_cli` package with `check INPUT --threshold N`. It reads the Module 08 JSON schema and prints device, sequence, and fixed-rule status in input order. Provide a default threshold of 80. Test help, valid input, invalid option, malformed file, and NaN threshold, including stdout, stderr, and exit status. Add a README with root-directory commands and a `.gitignore` for generated files. This exercise does not require building a wheel or installing dependencies.

Create source files empty and work from this exercise directory unless its requirements name a project root. Use only the standard library. Copy your own earlier code explicitly when needed; document its origin and any changed contracts. No completed implementation or answer key is supplied.

## Part C — Design reasoning

Why should importing `mother_cli.cli` not parse process arguments? Which command proves a package's entry point, and which claim does it leave untested?

## Submission

Keep written work in `lesson03-answers.md`, separate from the implementation. Include the named source files, test files where required, and enough commands and output to reproduce your evidence. For the capstone, continue in one project across all three exercises.

- [ ] I wrote predictions before executing examples.
- [ ] I checked each required success, boundary, and failure case.
- [ ] I recorded observed behavior and explained differences from my prediction.
- [ ] I can reproduce the result from the stated project root.
- [ ] I kept supplied observations and existing work unchanged.

**Status:** Not submitted
