# Module 08 — Final Assessment

**Course:** Python 0–100, Edition 2  
**Scope:** Lessons 8.1–8.3  
**Total:** 100 points

Complete all lessons first. Keep written answers in `assessment-answers.md` and implementation in the named directories. Use the standard library and supplied local data. Start independent assignments from empty files; you may reuse your own core for the Module 10 delivery, identifying changes since the exercises. Record predictions before experiments. Do not overwrite earlier answers or use a completed answer key.

## Part A — Knowledge assessment (30 points)

Choose one best answer per question. Each question is worth 3 points.

**1.** What does successful JSON decoding establish?

A. The text can be translated into Python values  
B. The observation schema is valid  
C. Every field is finite  
D. Every identity is unique

**2.** What does a relative file path use as its base?

A. The source file directory always  
B. The current working directory  
C. The user home always  
D. The package metadata

**3.** Which CPU satisfies the required JSON schema?

A. `true`  
B. `"80"`  
C. `80`  
D. `null`

**4.** Which condition is an identity conflict?

A. An exact numeric replay  
B. A new sequence  
C. An empty batch  
D. Same device and sequence with different CPU

**5.** Why bind SQL values through placeholders?

A. To separate data values from SQL syntax  
B. To replace every table name  
C. To skip validation  
D. To force row order

**6.** Which operation establishes database report order?

A. Insertion time alone  
B. `ORDER BY device, sequence`  
C. The existence of a primary key alone  
D. Opening the file twice

**7.** Under the chosen transaction mode, what should happen when a late conflict escapes a batch transaction context?

A. Earlier batch rows remain committed  
B. Every table disappears  
C. Batch row changes roll back  
D. The file must disappear

**8.** What does the connection transaction context NOT do automatically?

A. Commit on normal completion  
B. Roll back on exception  
C. Manage a transaction boundary  
D. Close the connection

**9.** Why is `float` conversion insufficient for a threshold?

A. It can accept non-finite values such as NaN  
B. It never accepts 80  
C. It always creates a database  
D. It automatically validates 0–100

**10.** What does running `python -m package` from a project root prove?

A. That a wheel is published  
B. That the package entry point runs in that layout  
C. That dependencies are installed everywhere  
D. That virtual environments restrict OS access

## Part B — Independent implementation (50 points)

### Assignment 1 — Strict JSON boundary (15 points)

In `assignment01/`, implement the specified bounded observation-list parser. Demonstrate correct records and rejection of duplicate JSON keys, duplicate identities, unknown fields, numeric strings, booleans, non-finite values, and invalid last rows. Include temporary-file tests and useful record context.

### Assignment 2 — Transactional history (15 points)

In `assignment02/`, implement SQLite initialization, parameterized batch import, and ordered readback. Test first import, identical replay, a new row, a late conflicting identity, and close/reopen. Prove the late conflict preserves both count and values. Use single-writer explicit transactions and close all connections.

### Assignment 3 — CLI delivery (20 points)

In `assignment03/`, build a package with `ingest INPUT --db DB` and `report --db DB --threshold 80`. Apply the module contracts, return JSON on stdout, and diagnostics on stderr. Test help without file creation, invalid threshold before effects, malformed import, replay, report order, and missing database. Document launch commands from the project root; no wheel is required.

## Part C — Design reasoning (20 points)

Answer in your own words with a concrete example. Each question is worth 10 points.

1. A developer uses `INSERT OR IGNORE` and calls every ignored row an identical replay. Explain a concrete data-loss scenario and specify the checks needed to distinguish replay from conflict.
2. A validation generator writes and commits each row as it yields, then fails on the last row. Explain why this violates the course contract and compare bounded prevalidation with one encompassing transaction.

## Grading

| Component | Points |
|---|---:|
| Knowledge — 10 × 3 | 30 |
| Assignment 1 | 15 |
| Assignment 2 | 15 |
| Assignment 3 | 20 |
| Design reasoning — 2 × 10 | 20 |
| **Total** | **100** |

For each implementation, award 60% for satisfying the stated requirements, 30% for reproducible success and failure evidence, and 10% for clarity. For a 15-point task this is 9/4.5/1.5; for a 20-point task it is 12/6/2. For each design answer award 6 points for the correct model and 4 for applying it to the scenario.

**90–100:** Mastered  
**75–89:** Passed with targeted review  
**Below 75:** Repeat the relevant lessons and reassess

## Submission checklist

- [ ] Written work is separate from source and instructions.
- [ ] Every command names its working directory and required starting state.
- [ ] Tests and demonstrations include boundaries and rejected operations.
- [ ] Evidence shows actual values and statuses, not only claims of success.
- [ ] Generated files are separate from submitted source.

**Status:** Not submitted
