# Module 10 — Final Assessment

**Course:** Python 0–100, Edition 2  
**Scope:** Lessons 10.1–10.3  
**Total:** 100 points

Complete all lessons first. Keep written answers in `assessment-answers.md` and implementation in the named directories. Use the standard library and supplied local data. Start independent assignments from empty files; you may reuse your own core for the Module 10 delivery, identifying changes since the exercises. Record predictions before experiments. Do not overwrite earlier answers or use a completed answer key.

## Part A — Knowledge assessment (30 points)

Choose one best answer per question. Each question is worth 3 points.

**1.** What is the required Mother data source?

A. An explicit local observation JSON file  
B. Automatic background monitoring  
C. A remote privileged sensor  
D. An implicit home-directory scan

**2.** Which identity uniquely names an observation?

A. CPU alone  
B. (device, sequence)  
C. Device alone  
D. The output filename

**3.** What should an identical import replay do?

A. Duplicate every record  
B. Overwrite model parameters  
C. Add no rows and report unchanged records  
D. Fail as a conflict

**4.** Which rows may update the fitted mean?

A. Every row including test  
B. Validation only  
C. Test only  
D. Training rows only

**5.** What does the dataset fingerprint establish?

A. A reproducible binding to the evaluation dataset  
B. Creator authentication  
C. A proof of anomaly accuracy  
D. An exemption from schema checks

**6.** What happens if the database changes after training?

A. Evaluation silently retrains  
B. Evaluation rejects a stale artifact  
C. The hash stays unchanged by definition  
D. Only stdout changes

**7.** Which values are distinct report concepts?

A. CPU and CPU  
B. Sequence and identical sequence  
C. Fixed high status, residual flags, and predictive error  
D. Two names for one automatically learned threshold

**8.** What is required when a model output path already exists?

A. Silently overwrite it  
B. Append JSON  
C. Ignore the option  
D. Refuse the output path

**9.** What should a clean demonstration include?

A. Fresh resources, actual test count, commands, intermediate state, and metrics  
B. Only a final success sentence  
C. Only an old database  
D. Only the package directory name

**10.** Which claim is supported by the trend fixture?

A. The mean always wins  
B. The application reproduces arithmetic while the mean loses to persistence  
C. Every residual is an incident  
D. Real-world production readiness

## Part B — Independent implementation (50 points)

### Assignment 1 — Domain and command contract (15 points)

In your capstone project, submit `design.md`, validation and classification code, all four command interfaces, and boundary tests. Include bounded JSON shape, duplicate keys and identities, CPU 79/80/81, help before effects, and explicit exit/output behavior. Map requirements to evidence.

### Assignment 2 — History and learned evaluation (15 points)

Complete transactional ingestion, replay/conflict policy, restart-safe reports, training-only fit, artifact validation, canonical fingerprint, and chronological evaluation. Use the two-device ten-row fixture from Exercise 9.3 and the expected metrics in Lesson 10.3. Test rollback, unknown schema, parameter consistency, stale-model rejection, and missing database behavior.

### Assignment 3 — Independent handoff (20 points)

Deliver `README.md`, `model-card.md`, `verification.md`, source, fixtures, and tests. Reproduce the complete command sequence in a clean temporary project with absent database/model files, record actual tests collected, and demonstrate at least four failures with unchanged history where required. Include expected/actual counts and metrics and explain the single-writer, synthetic-data, and model-quality limits.

## Part C — Design reasoning (20 points)

Answer in your own words with a concrete example. Each question is worth 10 points.

1. A saved model has a matching fingerprint but its stored mean was manually changed. Explain why provenance alone is insufficient, how training-summary consistency catches this, and why neither mechanism authenticates the author.
2. A demonstration ends with correct row count and a passing process status. Explain which incorrect intermediate operations this could hide, and design a stronger sequence checking replay, rollback, restart, and held-out evaluation.

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
