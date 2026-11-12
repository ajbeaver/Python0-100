# Exercise 2.1 — CPU History Statistics

**Module:** 02 — Control Flow and Functions  
**Lesson:** 2.1 — Loops, Iteration, and Accumulators  
**Difficulty:** Foundational  
**Estimated time:** 25–35 minutes

## Objective

Use a `for` loop and two accumulators to compute statistics from a chronological series of CPU readings.

## Starting data

Copy this data into a Python file you create yourself:

```python
cpu_history = [45, 62, 78, 91, 83, 67, 95]
```

## Part A — Implementation

Create an **empty** `lesson01.py` in this exercise directory. Write the implementation yourself.

Your program must:

1. Iterate through all readings with `for`.
2. Calculate total CPU utilization with a running accumulator.
3. Count readings **strictly greater than 80** with a separate accumulator.
4. Calculate the arithmetic average of all readings.
5. Print the total, high-reading count, and average after processing the list.

You may use `len()`; do not use `sum()` or an imported statistics library. The supplied list is nonempty.

## Expected results

```text
Total CPU: 521
High CPU Readings: 3
Average CPU: 74.42857142857143
```

You choose your own display formatting, but all numerical results must agree.

## Part B — Reasoning

Create `lesson01-answers.md` and explain:

1. Why does the running total need initialization outside the loop?
2. What would happen if it were reset inside the loop?
3. How is `for` different from `while` for this particular problem?
4. If the history were empty, what would happen to the average calculation? Why is a zero reading not the same as an empty history?

## Constraints

- No classes, imports, or external packages.
- No `sum()` or `statistics.mean()`.
- Keep the program small; no function extraction required yet.
- Predict the result before executing the program.

## Submission

- `lesson01.py` — your implementation
- `lesson01-answers.md` — your reasoning

**Status:** Not submitted.
