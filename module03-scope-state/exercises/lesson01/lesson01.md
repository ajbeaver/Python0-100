# Exercise 3.1 — Scope and Explicit Dependencies

**Module:** 03 — Scope, References, and State  
**Lesson:** 3.1  
**Difficulty:** Foundational  
**Estimated time:** 25–40 minutes

## Instructions

Create `lesson01.py` yourself, initially empty. Write your prediction and reasoning in `lesson01-answers.md`. Do not use external packages or classes.

## Part A — Predict before running

### Question 1

```python
threshold = 80

def check(value):
    return value > threshold

print(check(85))
```

What prints? Which namespace supplies `threshold`? What implicit dependency does `check()` have?

### Question 2

```python
count = 3

def increment():
    count += 1
    return count

print(increment())
```

Predict whether this succeeds and explain which name Python considers local.

### Question 3

```python
def examine(values):
    total = 0
    for value in values:
        total += value
    return total

result = examine([2, 4, 6])
```

Which names are local to `examine()`? Does the loop create its own scope?

## Part B — Implement

### Scenario

Mother evaluates readings using thresholds that might vary between environments.

### Requirements

1. Create a function that accepts one reading and one threshold.
2. Return a Boolean indicating whether the reading is **strictly greater** than the threshold.
3. Use the function to evaluate the three readings below against a threshold of `80`.
4. Print each reading and returned Boolean from the calling code, not inside the function.
5. Don't use module-level threshold variables inside the function.

### Starting data to transcribe

```python
readings = [65, 80, 92]
```

### Constraints

- No global declarations, classes, or imports.
- Use at least one `for` loop.
- The calculation function must return rather than print.

## Part C — Reasoning

Describe why giving the threshold as an argument is more predictable than reading a module-level variable, even when both approaches work.

## Submission

- `lesson01.py` (your work)
- `lesson01-answers.md` (your predictions and explanation)

**Status:** Not submitted.
