# Exercise 1.2 — Types, Expressions, and Boolean Logic

**Module:** 01 — Python Foundations  
**Lesson:** 1.2 — Types, Expressions, and Operators  
**Difficulty:** Foundational  
**Estimated time:** 25–35 minutes

## Objective

Predict expression results and use comparisons to calculate environmental warnings without unnecessary conditional branches.

## Part A — Predict before executing

Write your predictions and reasoning in `lesson02-answers.md`.

### Question 1

```python
value = 10
value = "ten"
print(type(value))
```

What prints? Why does the second assignment not raise a type error?

### Question 2

```python
print(10 / 2)
print(10 // 3)
print(10 % 3)
```

Predict the output and explain why `/` and `//` differ.

### Question 3

```python
cpu_warning = False
memory_warning = True
print(cpu_warning or memory_warning)
```

What prints? Would the result change if both values were `False`?

## Part B — Implementation

Create an **empty** `lesson02.py` and implement the requirements. Do not copy a completed solution.

### Starting data

```python
cpu = 75
memory = 82
temperature = 68
```

### Requirements

1. Calculate `cpu_warning` as a Boolean indicating whether CPU exceeds 80.
2. Calculate `memory_warning` as a Boolean indicating whether memory exceeds 80.
3. Calculate `system_warning` as a Boolean indicating whether **either** warning is active.
4. Print all three warning values, even when they are `False`.

### Constraints

- Use direct Boolean expressions to assign warning values.
- Do not use `if`, `elif`, `else`, loops, or functions.
- Do not manually assign `system_warning = True` or `False` based on the supplied data.
- Use only standard Python features.

### Expected behavior

Given the starting data, `cpu_warning` is `False`, `memory_warning` is `True`, and `system_warning` is `True`. Your code should also work if you change the CPU and memory values.

## Part C — Design reasoning

Explain in `lesson02-answers.md`:

1. Why is direct assignment from `cpu > 80` simpler than assigning `True` and `False` in separate conditional branches?
2. What's the difference between calculating a warning value and deciding whether to print a warning message?

## Submission

- `lesson02.py` — written by you
- `lesson02-answers.md` — predictions and reasoning

**Status:** Pending review
