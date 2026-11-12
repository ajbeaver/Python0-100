# Exercise 2.3 — Reusable Environmental Analysis

**Module:** 02 — Control Flow and Functions  
**Lesson:** 2.3 — Functions, Parameters, and Responsibility Separation  
**Difficulty:** Intermediate  
**Estimated time:** 35–50 minutes

## Objective

Extract two reusable computations into functions with explicit inputs and returned results.

## Starting data

Put the following in a Python file you create yourself:

```python
cpu_history = [45, 62, 78, 91, 83, 67, 95]
```

## Part A — Implementation

Create an initially **empty** `lesson03.py`, then write:

1. A function named `calculate_average` that takes a list of measurements, uses a loop and accumulator to calculate the average, and **returns** the result.
2. A function named `count_high_readings` that accepts a list and an explicit threshold, counts readings **strictly greater than** that threshold, and **returns** the count.
3. Calling code outside both functions that passes the supplied history and a threshold of 80.
4. Output from the calling code that prints both returned results.

Both functions must operate on their parameters, not directly on a module-level `cpu_history` name. For this exercise, assume the list is nonempty.

## Expected results

```text
Average CPU: 74.42857142857143
High CPU Readings: 3
```

As an extra check, call the count function with a threshold of 90. It should return `2` without changing the function's implementation.

## Part B — Reasoning

In `lesson03-answers.md`, explain:

1. The difference between defining and calling a function.
2. Parameter versus argument, using one of your functions as an example.
3. Why returning the average is more reusable than printing inside the calculation function.
4. Why the accumulator inside a function does not need to be global.
5. What would happen if a function only printed a value and you assigned its call to a variable.

## Constraints

- Use no external packages or `sum()`.
- No classes.
- Keep printing outside the calculation functions.
- No hardcoded threshold inside `count_high_readings`.

## Submission

- `lesson03.py` — your implementation
- `lesson03-answers.md` — your reasoning

**Status:** Not submitted.
