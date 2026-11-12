# Exercise 1.1 — Variables, References, and Objects

**Module:** 01 — Python Foundations
**Lesson:** 1.1 — Python Execution, Objects, and Variables
**Difficulty:** Foundational
**Estimated time:** 20–30 minutes

## Objective

Demonstrate an understanding of Python variables, object references,
assignment, mutation, and basic types.

This exercise contains three parts:

1. Predict program behavior.
2. Explain object references.
3. Implement a small Python program.

Do not use functions, classes, or external packages.

---

## Part A — Predicting Execution

### Question 1

Examine the following program without executing it:

```python
a = 10
b = a

a = 25

print(a)
print(b)
```

Answer:

1. What are the two printed values?
2. Why doesn't assigning a new value to `a` change `b`?
3. Does the assignment modify the original integer object?

Write your explanation in `lesson01-answers.md`.

### Question 2

Examine the following program without executing it:

```python
first = [1, 2]
second = first

first.append(3)

print(second)
```

Answer:

1. What does the program print?
2. Why does modifying `first` affect what we observe through `second`?
3. How does this differ from Question 1?

Write your explanation in `lesson01-answers.md`.

---

## Part B — Implementation

### Scenario

Mother is beginning to observe her computational environment.

Before implementing environmental monitoring, we need a basic
representation of a device and its current state.

### Requirements

Create a Python file named `lesson01.py`.

The file must begin empty. Write the implementation yourself.

Your program must:

1. Define a variable containing a device name as a string.
2. Define a variable containing CPU utilization as an integer.
3. Define a variable indicating whether the device is active as a Boolean.
4. Print the value of each variable.
5. Print the type of each variable using Python's built-in `type()` function.

### Constraints

- Use only built-in Python features.
- Do not import external packages.
- Do not define functions or classes.
- Do not use lists or dictionaries.
- Do not hardcode the expected output as a single string.

### Expected Behavior

The program should produce six pieces of information:

- Device name
- Device name type
- CPU utilization
- CPU utilization type
- Device active status
- Device active status type

You may choose your own device name, CPU measurement, and active status.

The exact output formatting is your decision.

---

## Part C — Design Reasoning

Answer the following question in `lesson01-answers.md`.

Suppose Mother maintains a collection of historical observations.

What problem could occur if multiple historical observations reference
the same mutable object?

Explain how this relates to the distinction between assignment and
mutation.

No implementation is required.

---

## Submission

Submit the following files for review:

- `lesson01.py`
- `lesson01-answers.md`

### Evaluation Criteria

| Category | Points |
|---|---:|
| Execution predictions | 20 |
| Understanding of references | 20 |
| Python implementation | 40 |
| Design reasoning | 20 |
| **Total** | **100** |

### Completion Checklist

- [ ] Predictions completed before running the examples.
- [ ] Python implementation satisfies all requirements.
- [ ] Program executes without errors.
- [ ] Written explanations use my own words.
- [ ] All files are ready for review.

---

**Status:** Not submitted

**Next Step:** Submit your implementation and written answers for review.
