# Module 03 — Scope, References, and State

**Course:** Python 0–100, Edition 2  
**Prerequisites:** Modules 01–02  
**Status:** Not started

## Purpose

Module 02 introduced functions with explicit parameters and return values. Module 03 investigates what happens when those parameters refer to *mutable* objects, where Python looks up names, and how a program can accidentally carry state between independent operations.

The objective is not to eliminate all mutation or global state. It is to understand their consequences and make deliberate decisions about ownership, lifetime, and boundaries.

## Textbook

| Lesson | File | Focus |
|---|---|---|
| 3.1 | `lesson01-scope.md` | Local/global namespaces, lookup, assignment, scope |
| 3.2 | `lesson02-references.md` | Identity, aliasing, mutation, copies, function arguments |
| 3.3 | `lesson03-state.md` | State lifetime, stale state, side effects, responsibility boundaries |

Read in order. Each chapter includes examples and a checkpoint; corresponding independent exercise specifications are under `exercises/`.

## Coursework layout

```text
module03-scope-state/
├── README.md
├── lesson01-scope.md
├── lesson02-references.md
├── lesson03-state.md
├── assessment.md
└── exercises/
    ├── lesson01/lesson01.md
    ├── lesson02/lesson02.md
    └── lesson03/lesson03.md
```

Create Python implementation files yourself, beginning empty unless an exercise explicitly supplies starting data to transcribe. No solution code is included in the package. Write written reasoning in your own Markdown answer files.

## Expected progression

1. Explain which names are local to a function and which are resolved from an enclosing or module namespace.
2. Predict how rebinding and mutation affect multiple references to the same object.
3. Recognize when state should reset for every observation versus persist across observations.
4. Define simple function contracts that limit accidental changes to caller-owned data.

## Mother connection

Mother records a sequence of independent observations. It must not silently rewrite history when a new reading arrives, and a warning from one observation must not automatically contaminate the next. Later, controlled historical state will support actual pattern learning. This module provides the conceptual foundation.

## Assessment

`assessment.md` contains ten multiple-choice questions (30 points), three independent implementation challenges (50 points), and two design questions (20 points). No answer key is provided. Complete after all three lessons have been reviewed.

## References

- [Python Programming FAQ — Why did changing a list also change another variable?](https://docs.python.org/3/faq/programming.html)
- [Python Tutorial — More on Defining Functions](https://docs.python.org/3/tutorial/controlflow.html#more-on-defining-functions)
- [Python Language Reference — Execution Model](https://docs.python.org/3/reference/executionmodel.html)
- [Python Library Reference — copy](https://docs.python.org/3/library/copy.html)

**Next module:** 04 — Modules, Packages, and Imports.
