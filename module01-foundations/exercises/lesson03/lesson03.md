# Exercise 1.3 — Observation History and Data Modeling

**Module:** 01 — Python Foundations  
**Lesson:** 1.3 — Strings, Lists, and Dictionaries  
**Difficulty:** Foundational to intermediate  
**Estimated time:** 30–45 minutes

## Objective

Represent a changing environment using lists and dictionaries, and explain why those structures fit the information.

## Part A — Predict before executing

Answer in `lesson03-answers.md`.

### Question 1

```python
device = "node01"
print(device[1:4])
print(device[-1])
```

Predict both results. Explain why the ending slice index is excluded.

### Question 2

```python
a = [1, 2]
b = a
a.append(3)
print(b)
```

Predict the result. Would it differ if the last operation on `a` were `a = a + [3]` instead of `a.append(3)`?

### Question 3

```python
environment = {"cpu": 75}
print(environment.get("memory"))
```

Predict the output. How would bracket access to `environment["memory"]` differ?

## Part B — Implementation

Create an **empty** file named `lesson03.py`.

### Starting data

```python
environment = {
    "cpu": 75,
    "memory": 82,
    "temperature": 68
}
```

### Requirements

1. Create `cpu_history` containing historical CPU values `60`, `65`, and `70`.
2. Append the current CPU measurement from the environment dictionary. Do not manually enter `75` a second time.
3. Update the CPU value in the dictionary to `85`.
4. Append the updated CPU measurement to `cpu_history`.
5. Print both `cpu_history` and the current environment dictionary.

### Expected history

```text
[60, 65, 70, 75, 85]
```

### Constraints

- No functions, classes, loops, or external packages.
- Use dictionary key access, assignment, and list methods.
- Do not replace the entire list with a hardcoded expected result.

## Part C — Design reasoning

In `lesson03-answers.md`, explain:

1. Why a list is a good choice for chronological CPU history.
2. Why a dictionary is a good choice for a single observation containing CPU, memory, and temperature.
3. What a list of dictionaries would represent, and one risk of storing the same mutable dictionary object repeatedly.

## Submission

- `lesson03.py` — written by you
- `lesson03-answers.md` — predictions and reasoning

**Status:** Pending review
