# Exercise 2.2 — Environmental Status Classification

**Module:** 02 — Control Flow and Functions  
**Lesson:** 2.2 — Conditions, Truthiness, and Branching  
**Difficulty:** Intermediate  
**Estimated time:** 35–50 minutes

## Objective

Classify each environmental observation, report independent warnings, and avoid retaining stale warning flags from earlier observations.

## Starting data

Use this data in your own new, initially empty `lesson02.py`:

```python
observations = [
    {"cpu": 45, "memory": 50, "temperature": 60},
    {"cpu": 85, "memory": 50, "temperature": 60},
    {"cpu": 50, "memory": 90, "temperature": 80},
    {"cpu": 97, "memory": 40, "temperature": 50},
    {"cpu": 95, "memory": 99, "temperature": 90},
    {"cpu": 45, "memory": 50, "temperature": 60}
]
```

## Part A — Implementation

For **every** observation, in chronological order:

1. Classify CPU utilization into `normal` (below 80), `elevated` (80–94 inclusive), or `critical` (95 or above).
2. Determine `memory_warning` as a Boolean indicating memory **greater than 80**.
3. Determine `temperature_warning` as a Boolean indicating temperature **greater than 75**.
4. Print a separate memory warning when memory is high, and a separate temperature warning when temperature is high. Both should print if both are high.
5. Assign one overall status: `critical` when CPU is critical; otherwise `warning` when CPU is elevated or either warning is active; otherwise `normal`.
6. Print the observation, CPU classification, and overall status.

Requirements concern the **current observation**; don't preserve warning state from earlier observations unless explicitly asked.

## Expected overall statuses

| Observation | Expected |
|---|---|
| 1 | normal |
| 2 | warning |
| 3 | warning |
| 4 | critical |
| 5 | critical |
| 6 | normal |

Observation 3 must print **both** independent warning messages. Observation 6 must be normal even after earlier warnings.

## Part B — Reasoning

Write `lesson02-answers.md` explaining:

1. Why is CPU classification a single `if`/`elif`/`else` chain?
2. Why do memory and temperature alerts need separate `if` statements for printing?
3. How can a warning flag become stale between observations?
4. When is a direct Boolean assignment preferable to an `if`/`else` assignment of `True` and `False`?

## Constraints

- No functions or classes in this exercise.
- No third-party packages.
- The starting observations must remain in their given order.
- Test the last observation explicitly.

## Submission

- `lesson02.py` — your implementation
- `lesson02-answers.md` — your reasoning

**Status:** Not submitted.
