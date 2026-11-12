# Exercise 4.3 — Dependencies and Import Safety

**Module:** 04 — Modules, Packages, and Imports  
**Lesson:** 4.3 — Dependencies, Interfaces, and Import-Time Safety  
**Difficulty:** Intermediate  
**Estimated time:** 45–70 minutes

## Objective

Design a small, dependency-aware application with no circular imports or import-time data collection.

## Part A — Dependency analysis

Draw a dependency diagram in `lesson03-answers.md` for the following proposed design:

```text
cli imports classifier
classifier imports formatter
formatter imports classifier
```

Answer:

1. What cycle exists?
2. Why might the design be fragile even if it sometimes runs?
3. Propose a new direction for the dependencies that avoids the cycle.

## Part B — Implementation

Create empty source files in a package called `mother_sample`:

```text
lesson03/
└── mother_sample/
    ├── __init__.py
    ├── __main__.py
    ├── sample_data.py
    ├── classifier.py
    └── formatter.py
```

### Starting observations

These are fixed test values, **not live CPU readings**:

```python
observations = [
    {"device": "node01", "cpu": 72},
    {"device": "node02", "cpu": 88},
    {"device": "node03", "cpu": 80},
]
threshold = 80
```

### Requirements

1. `sample_data.py` must make the observations available without importing the other modules.
2. `classifier.py` must expose a function that accepts an observation and threshold and returns a classification result; it must not print.
3. `formatter.py` must accept a classification result and return readable text; it must not print.
4. `__main__.py` must orchestrate the modules and print one line per observation.
5. `python3 -m mother_sample` must work when launched from `lesson03/`.
6. No module import may automatically start monitoring, open a network connection, or modify files.
7. There must be no circular imports.

### Constraints

- Standard library only.
- No classes, global mutable observation history, or external system data collection.
- You choose the structure of the returned classification result and document it.
- Handle an observation equal to the threshold according to the lesson's *strictly greater than* convention.

## Part C — Design reasoning

Explain which module owns each responsibility. Identify one change you could make to presentation without changing classification, and one change to classification without changing sample data.

## Submission

- Five Python source files within `mother_sample/`
- `lesson03-answers.md` (diagram, API contract, reasoning)

## Completion checklist

- [ ] Dependencies form an acyclic graph.
- [ ] Application starts by `-m` from the required directory.
- [ ] Imported helper modules do not print or perform I/O.
- [ ] Classification and formatting are separate responsibilities.

**Status:** Not submitted
