# Exercise 3.2 — Shared References and Safe History

**Module:** 03 — Scope, References, and State  
**Lesson:** 3.2  
**Difficulty:** Intermediate  
**Estimated time:** 35–50 minutes

## Instructions

Create `lesson02.py` yourself, initially empty. Put predictions and design reasoning in `lesson02-answers.md`. Do not use classes or third-party dependencies.

## Part A — Predict without running

### Question 1

```python
def add_value(values):
    values.append(7)

first = [3, 5]
second = first
add_value(first)
print(second)
```

Predict the output and explain what is shared.

### Question 2

```python
def add_value(values):
    values = values + [7]
    return values

first = [3, 5]
second = add_value(first)
print(first)
print(second)
```

Predict both outputs. Explain why the caller's `first` remains unchanged.

### Question 3

```python
history = [{"cpu": 60}]
backup = history.copy()
backup[0]["cpu"] = 99
print(history[0]["cpu"])
```

Predict the output and explain the difference between a shallow copy of the outer list and a copy of its nested dictionaries.

## Part B — Implement

### Scenario

Mother receives a new CPU reading. You need two distinct operations: one deliberately changes an existing history, and one returns a new history without changing the input list.

### Starting data to transcribe

```python
cpu_history = [45, 60, 72]
new_reading = 88
```

### Requirements

1. Write one function that **appends** a reading to a list supplied by the caller.
2. Write another function that returns a **new list** containing the previous readings plus the new reading, leaving its input list unchanged.
3. Demonstrate each function on its own initial copy or fresh list, so one demonstration does not affect the other.
4. Print both the original and resulting histories after each demonstration.
5. Explain in your answer file whether each function mutates caller-owned data.

### Constraints

- Don't use imports or classes.
- No need for deep copying: readings are integers.
- Don't change the original data just to make your output match expectations.
- Your function names should make their behavior understandable.

## Part C — Reasoning

Would you permit an average-calculation function to change the supplied history? Explain why or why not. Under what circumstances would appending to a history be an appropriate side effect?

## Submission

- `lesson02.py`
- `lesson02-answers.md`

**Status:** Not submitted.
