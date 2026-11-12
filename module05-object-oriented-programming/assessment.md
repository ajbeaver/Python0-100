# Module 05 — Final Assessment

**Course:** Python 0–100, Edition 2  
**Scope:** Lessons 5.1–5.3  
**Total:** 100 points

Complete all lessons first. Keep written answers in `assessment-answers.md` and implementation in the named directories. Use the standard library and supplied local data. Start independent assignments from empty files; you may reuse your own core for the Module 10 delivery, identifying changes since the exercises. Record predictions before experiments. Do not overwrite earlier answers or use a completed answer key.

## Part A — Knowledge assessment (30 points)

Choose one best answer per question. Each question is worth 3 points.

**1.** What is supplied as `self` when `meter.is_high(80)` is called?

A. The instance `meter`  
B. The integer 80  
C. The class name as text  
D. The caller module

**2.** Where should a fresh per-instance mutable history usually be created?

A. As a class-level list  
B. In `__init__` on the instance  
C. In a shared module global  
D. In the method default argument

**3.** Which ordering preserves state when validation rejects an update?

A. Assign then validate  
B. Print then assign  
C. Validate then assign  
D. Ignore invalid values

**4.** What does a leading underscore enforce?

A. OS access control  
B. Encrypted attributes  
C. Automatic type validation  
D. An internal-use convention, without enforced privacy

**5.** What does an ordinary type annotation guarantee at runtime?

A. It documents a type but does not itself enforce it  
B. Every argument is converted  
C. Bad input is rejected automatically  
D. Mutable fields become immutable

**6.** What does `frozen=True` prevent in ordinary dataclass use?

A. All changes to every reachable object  
B. Ordinary assignment to its fields  
C. Calling any method  
D. Creating two equal instances

**7.** Which relationship best describes a history containing readings?

A. A reading inherits history  
B. History is the same object as every reading  
C. Composition  
D. Circular import

**8.** Two distinct dataclass records have identical field values. What can be true?

A. They must be identical by `is`  
B. Equality must be false  
C. They must share every field object  
D. They compare equal without being identical

**9.** What is duck typing concerned with?

A. Required behavior rather than a mandatory common base  
B. Only class names  
C. Only constructor syntax  
D. Disabling exceptions

**10.** Why test two independently created histories?

A. To confirm they share state  
B. To detect accidental shared mutable state  
C. To force identical identity  
D. To avoid boundary tests

## Part B — Independent implementation (50 points)

### Assignment 1 — Validated measurement (15 points)

Create `assignment01/measurement.py` and `demo.py`. Implement an integer-only measurement with valid values 0–100 and a successful update method. Reject booleans, strings, and out-of-range values before mutation. Demonstrate 0, 100, successful update, and three rejected updates with before/after evidence.

### Assignment 2 — Owned history (15 points)

Create `assignment02/` with a frozen observation record and a history owner. Records contain device, sequence, and CPU using Lesson 5.2 validation. Expose add and snapshot operations. Demonstrate independent histories, equal-value records, and a snapshot that cannot change the internal collection. Explain shallow versus deep immutability.

### Assignment 3 — Replaceable policy (20 points)

Create `assignment03/` with an evaluator and two policies: strict-above-threshold and strictly-outside-inclusive-band. Run one evaluator with both on [0, 20, 50, 80, 100]. Include exact boundaries and invalid configuration. Document a shared interface, demonstrate no input mutation, and justify whether each policy needs an object.

## Part C — Design reasoning (20 points)

Answer in your own words with a concrete example. Each question is worth 10 points.

1. A history returns its internal list, and another component calls `clear()` on it. Explain the ownership failure and propose an interface preserving the supported operations.
2. A subclass returns strings from a method whose documented result is Boolean. Explain how matching a method name can still violate substitutability.

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
