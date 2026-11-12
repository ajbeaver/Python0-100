# Module 07 — Final Assessment

**Course:** Python 0–100, Edition 2  
**Scope:** Lessons 7.1–7.3  
**Total:** 100 points

Complete all lessons first. Keep written answers in `assessment-answers.md` and implementation in the named directories. Use the standard library and supplied local data. Start independent assignments from empty files; you may reuse your own core for the Module 10 delivery, identifying changes since the exercises. Record predictions before experiments. Do not overwrite earlier answers or use a completed answer key.

## Part A — Knowledge assessment (30 points)

Choose one best answer per question. Each question is worth 3 points.

**1.** What is a useful expected value in a unit test?

A. The result from calling the same implementation again  
B. Whatever is printed today  
C. A value independently derived from the contract  
D. A private variable name

**2.** When does ordinary unittest `setUp` run?

A. Once for the machine  
B. After every assertion  
C. Only on failures  
D. Before each test method

**3.** What should accompany an expected rejected update?

A. An assertion that state stayed unchanged  
B. A new threshold  
C. A real user database  
D. A successful unrelated operation inside the exception block

**4.** Which sequence exposes a stale high flag?

A. High then high only  
B. High then normal  
C. Empty input only  
D. One high value only

**5.** What is the first useful step for a wrong-result bug?

A. Add a broad exception handler  
B. Change several files at once  
C. Record a reproducible input and independent expected output  
D. Delete all intermediate evidence

**6.** What establishes that a regression test detects the original bug?

A. It prints the implementation  
B. It uses a different class name  
C. It passes before and after the fix  
D. It fails on the faulty behavior and passes after the fix

**7.** What can a recording fake writer establish?

A. Whether orchestration attempted writes  
B. Actual SQLite crash durability  
C. Actual database rollback  
D. Kernel filesystem semantics

**8.** Where should file-based test resources live?

A. In a shared production directory  
B. In disposable per-test temporary directories  
C. At a fixed home-directory path  
D. In the interpreter installation

**9.** What can a fixed random seed help provide?

A. A proof of correctness  
B. An absence of edge cases  
C. Reproducible example sequences  
D. A substitute for assertions

**10.** Why inspect discovered test count?

A. It changes CPU values  
B. It chooses the model  
C. It configures logging  
D. A successful run might have collected no tests

## Part B — Independent implementation (50 points)

### Assignment 1 — Boundary suite (15 points)

In `assignment01/`, write a validated meter and at least eight unittest cases for success, exact threshold, extremes, rejection, unchanged state, and independent instances. Temporarily introduce a boundary mutation, show a failing assertion, then restore it. Supply the final passing test count and command.

### Assignment 2 — Regression investigation (15 points)

In `assignment02/`, reproduce a stale-warning implementation, add the minimal high-to-normal regression, and fix it. Cover reverse order, empty input, and repeated readings. Record expected/actual results before the fix and the relevant state lifetime. Keep final source correct and preserve faulty evidence in notes.

### Assignment 3 — Pipeline reliability (20 points)

In `assignment03/`, integrate a supplied source, parser, classifier, and formatter. Test valid ordered output, invalid-last-record rejection, no mutation, and exactly one source call. Add a bounded sequence test for history ownership using an independent expected list. Keep any later file tests in temporary directories and state exactly what your fakes cannot prove.

## Part C — Design reasoning (20 points)

Answer in your own words with a concrete example. Each question is worth 10 points.

1. A test wraps fixture creation, input parsing, and the intended update in one broad expected-exception block. Explain how it can pass for the wrong reason and rewrite the test structure in prose.
2. All classifier unit tests pass, but CLI output contains numeric text where the classifier expects numbers. Explain the missing test boundary and specify an integration case that would catch it.

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
