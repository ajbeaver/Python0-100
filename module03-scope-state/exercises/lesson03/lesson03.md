# Exercise 3.3 — Independent Observations and State Lifetime

**Module:** 03 — Scope, References, and State  
**Lesson:** 3.3  
**Difficulty:** Applied  
**Estimated time:** 40–60 minutes

## Instructions

Create `lesson03.py` yourself, initially empty. Write conceptual answers in `lesson03-answers.md`. Do not use external packages or classes.

## Scenario

Mother receives independent environmental snapshots. Each snapshot needs a fresh status classification while an overall summary count persists throughout the batch.

## Starting data to transcribe

```python
observations = [
    {"cpu": 50, "memory": 45},
    {"cpu": 84, "memory": 92},
    {"cpu": 51, "memory": 42},
    {"cpu": 96, "memory": 40},
    {"cpu": 40, "memory": 40}
]
```

## Part A — Implement

1. Write a function that accepts an observation dictionary and returns one of the following strings:
   - `"critical"` when CPU is at least `95`.
   - `"warning"` when CPU is at least `80` **or** memory is greater than `85`, unless CPU is critical.
   - `"normal"` otherwise.
2. The function must not print, mutate the observation, or retain previous warning flags.
3. Process the observations in order and print each classification in the calling code.
4. Maintain category totals across the batch and print a final summary.
5. Append one final normal observation and confirm it still classifies as `"normal"`.

### Final observation to append

```python
{"cpu": 35, "memory": 30}
```

### Expected category counts after append

```text
normal: 4
warning: 1
critical: 1
```

## Part B — Design reasoning

Answer in `lesson03-answers.md`:

1. Which values should reset for each observation? Which should persist for the whole batch?
2. Why is it dangerous to keep the current warning Boolean outside the loop without recalculating it for every observation?
3. If Mother later learns patterns from history, which information should be retained intentionally?

## Constraints

- Do not use global counters.
- Use at least one function with explicit arguments and a return value.
- Keep presentation separate from classification.
- You may use lists, dictionaries, loops, conditions, and functions from earlier modules.

## Submission

- `lesson03.py`
- `lesson03-answers.md`

**Status:** Not submitted.
