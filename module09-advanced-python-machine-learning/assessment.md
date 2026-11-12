# Module 09 — Final Assessment

**Course:** Python 0–100, Edition 2  
**Scope:** Lessons 9.1–9.3  
**Total:** 100 points

Complete all lessons first. Keep written answers in `assessment-answers.md` and implementation in the named directories. Use the standard library and supplied local data. Start independent assignments from empty files; you may reuse your own core for the Module 10 delivery, identifying changes since the exercises. Record predictions before experiments. Do not overwrite earlier answers or use a completed answer key.

## Part A — Knowledge assessment (30 points)

Choose one best answer per question. Each question is worth 3 points.

**1.** When does the body of a generator function normally begin running?

A. When its source file is written  
B. When the function is defined  
C. When its generator object is created  
D. When iteration requests a value

**2.** What does consuming an exhausted generator normally yield?

A. No more values  
B. The original sequence again  
C. A newly trained model  
D. A database rollback

**3.** What can destroy the memory advantage of a lazy stream?

A. Calling `next` once  
B. Materializing all its values into a list  
C. Keeping only the previous value  
D. Using a bounded transaction

**4.** Which value is learned by the course mean model?

A. The JSON field names  
B. The fixed tolerance selected by the developer  
C. A mean estimated from training CPU observations  
D. The device name spelling

**5.** What must inference avoid under the frozen evaluation protocol?

A. Using the stored mean  
B. Returning a numeric prediction  
C. Rejecting unknown devices  
D. Updating parameters with held-out actual values

**6.** For [20, 30, 40], what is the fitted mean?

A. 30  
B. 40  
C. 20  
D. 90

**7.** For actual 50 and prediction 30, what is the absolute error?

A. -20  
B. 20  
C. 30  
D. 80

**8.** Which split respects the stated forecasting protocol?

A. Shuffle all rows and tune on test  
B. Fit on the complete database  
C. Chronological train/validation/test within each device  
D. Use only records above the tolerance

**9.** What does the chosen persistence baseline use for each held-out prediction?

A. The current held-out actual  
B. The next actual  
C. A global zero  
D. The last training value, held fixed

**10.** Why can residual flags not establish anomaly accuracy here?

A. There are no independent ground-truth anomaly labels  
B. The mean is always wrong  
C. JSON prevents metrics  
D. MAE is a string

## Part B — Independent implementation (50 points)

### Assignment 1 — Bounded generator (15 points)

In `assignment01/`, implement per-device ordered deltas with one previous record of retained state. Demonstrate exhaustion, empty input, and lazy out-of-order failure. Explain how a caller would preserve whole-batch storage semantics rather than committing partial output.

### Assignment 2 — Fit and serialize (15 points)

In `assignment02/`, implement per-device mean fitting and a versioned JSON artifact with tolerance, counts, and trained-through sequences. Test two devices, round-trip equivalence, unknown devices, invalid model values, and unsupported version. Show learned parameters separately from chosen settings.

### Assignment 3 — Chronological evaluation (20 points)

In `assignment03/`, evaluate node01 CPU [10, 20, 30, 40, 50] and node02 [70, 70, 70, 70, 70] at sequences 0–4. Implement the precise 60/20/20 split, fixed mean and persistence predictions, per-device and pooled validation/test MAE, and residual labels at tolerance 15. Demonstrate no parameter change after altering only held-out values; reject devices with insufficient history. Show arithmetic independently in notes.

## Part C — Design reasoning (20 points)

Answer in your own words with a concrete example. Each question is worth 10 points.

1. A report claims perfect forecasting because the model was fitted on every available row before choosing the test slice. Identify the leakage and specify a reproducible corrected protocol.
2. The fitted mean loses to persistence on a rising series. Explain why this is a useful result, why anomaly accuracy remains unknown, and what evidence would be needed before a real deployment claim.

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
