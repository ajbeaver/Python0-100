# Module 06 — Final Assessment

**Course:** Python 0–100, Edition 2  
**Scope:** Lessons 6.1–6.3  
**Total:** 100 points

Complete all lessons first. Keep written answers in `assessment-answers.md` and implementation in the named directories. Use the standard library and supplied local data. Start independent assignments from empty files; you may reuse your own core for the Module 10 delivery, identifying changes since the exercises. Record predictions before experiments. Do not overwrite earlier answers or use a completed answer key.

## Part A — Knowledge assessment (30 points)

Choose one best answer per question. Each question is worth 3 points.

**1.** Which responsibility belongs in a pure classifier?

A. Opening the database  
B. Computing a status from explicit inputs  
C. Parsing process arguments  
D. Configuring logging

**2.** What makes a dependency graph acyclic?

A. Every module imports the entry point  
B. There are no functions  
C. No dependency path returns to its starting module  
D. All files have one line

**3.** What is dependency injection in this course?

A. Installing a framework  
B. Changing `sys.path`  
C. Reading hidden globals  
D. Passing required behavior or objects explicitly

**4.** Which source behavior supports reproducible assessment?

A. One call whose returned batch is reused  
B. One call for count and another for values  
C. A new random source for every field  
D. An implicit source chosen by a classifier

**5.** Where should effective configuration be validated?

A. Only after storing results  
B. At the boundary before effects  
C. Only in the README  
D. Inside every print statement

**6.** What does `raise NewError(...) from exc` preserve?

A. A successful exit code  
B. A default CPU  
C. The causal exception relationship  
D. The input file itself

**7.** Which handler is most likely to hide defects?

A. Catching a documented domain error  
B. Catching JSON syntax errors at a parser boundary  
C. Adding record context and reraising  
D. Catching every exception and returning an empty successful batch

**8.** Where should application logging handlers be configured?

A. At the application entry point  
B. Every helper import  
C. Inside record equality  
D. Inside every predicate

**9.** Which is the course configuration precedence rule?

A. An old global always wins  
B. An explicit option overrides the default  
C. All values are averaged  
D. The classifier picks randomly

**10.** Why specify whole-batch rejection early?

A. It eliminates validation  
B. It guarantees no disk file exists  
C. It determines the later transaction boundary  
D. It requires one class per field

## Part B — Independent implementation (50 points)

### Assignment 1 — Separate a pipeline (15 points)

In `assignment01/`, create a package accepting three supplied readings at CPU 79, 80, and 81 with threshold 80. Separate domain, orchestration, formatting, and entry point. Return results in order, print only at the boundary, and demonstrate unchanged inputs and import safety.

### Assignment 2 — Inject a changing source (15 points)

In `assignment02/`, implement a source returning a different fixed batch on successive calls. Inject it into an assessment service and demonstrate exactly one source call per service run. Add a second source implementation without changing the classifier. Record calls, effective configuration, and results.

### Assignment 3 — Failure contract (20 points)

In `assignment03/`, create a boundary parser for a list of supplied CPU text values. Reject the complete batch on any conversion or range failure, including NaN. Add index context and preserve causes. The entry point uses stdout only for successful results and stderr for diagnostics; invalid input exits 2. Demonstrate valid, invalid-final-item, and injected unexpected-defect cases without disguising a defect as success.

## Part C — Design reasoning (20 points)

Answer in your own words with a concrete example. Each question is worth 10 points.

1. A classifier reads an environment variable and writes its decision directly to SQLite. Explain how this affects reproducibility, testing, and replacement of storage. Propose explicit interfaces.
2. A service catches every exception and logs “processed 0 readings.” Explain why the message is ambiguous and define a better failure/result contract.

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
