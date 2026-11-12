# Lesson 4.3 — Dependencies, Interfaces, and Import-Time Safety

**Course:** Python 0–100, Edition 2  
**Module:** 04 — Modules, Packages, and Imports  
**Prerequisites:** Lessons 4.1–4.2  
**Estimated study time:** 60–80 minutes

## Learning objectives

By the end, you should be able to:

1. Draw a small dependency graph of Python modules.
2. Recognize circular imports and distinguish them from ordinary function calls between modules.
3. Design modules around one responsibility and explicit function contracts.
4. Recognize import-time side effects and shared module state.
5. Keep dependencies and system permissions minimal while leaving room for tests.

---

## 4.3.1 — Imports are dependencies

When `app.py` imports `evaluation.py`, the application **depends on** evaluation. We can represent that relationship with a directed arrow:

```text
app → evaluation
```

If evaluation imports a helper from `validation`, our graph becomes:

```text
app → evaluation → validation
```

Arrows point from the **importer** toward the **imported module**. A dependency graph helps answer: Which modules can change without forcing changes elsewhere? Where do we place shared logic? Which component is responsible for user interaction?

A module with many unrelated dependencies can be difficult to reason about. The goal is not zero dependencies; it is a readable, intentional direction of dependency.

## 4.3.2 — Circular imports

Suppose `evaluation.py` imports `reporting.py`, while `reporting.py` imports `evaluation.py`:

```text
evaluation → reporting
      ↑          ↓
      └──────────┘
```

When Python imports `evaluation`, it starts initializing that module. If it then imports `reporting`, and `reporting` tries to access a name from the still-initializing `evaluation`, that name may not exist yet. Python can produce an error mentioning a **partially initialized module**. Not every circular import fails, but cycles make initialization order difficult to predict and are often signs of mixed responsibilities.

One common redesign is to have both modules import a lower-level helper rather than each other:

```text
entrypoint → evaluation → common
           → reporting  → common
```

Another is to pass results from evaluation into reporting from the entry point. Evaluation does not need to know that reporting exists.

**Do not** hide every circular-import problem by moving imports inside functions. That can occasionally be appropriate, but first investigate whether the dependency structure is wrong.

## 4.3.3 — Separate computation from I/O

**Input/output (I/O)** refers to operations such as printing, reading files, accessing a device, or making network requests. Pure computation can often be separated from these effects.

Compare:

```python
def is_high(value, threshold):
    return value > threshold
```

with:

```python
def report_reading(value, threshold):
    if value > threshold:
        print("Warning")
```

The first function returns a result the caller can use without producing output. The second owns both a decision and presentation. Sometimes a combined function is reasonable for a tiny script, but reusable code tends to benefit from separating **evaluate** from **display**.

A *function contract* describes the expected inputs, outputs, and side effects. For example:

```text
is_high(value, threshold)
Input: numeric value and numeric threshold
Return: Boolean
Side effects: none intended
```

A contract helps another module use the function without studying its implementation. We will formalize testing in Module 07.

## 4.3.4 — Shared mutable module state

Modules are objects, and their globals can refer to mutable objects. For example:

```python
# history.py
readings = []
```

Other modules could import `history` and append to `history.readings`. Because the object is shared in the same process, updates become visible elsewhere. This may appear convenient, but it creates a hidden coupling: a function might behave differently depending on which other function ran previously.

Instead of relying on a global history list, pass history explicitly to functions that need it, and decide **who owns mutation**. This draws on Module 03's scope and aliasing lessons. The best choice can vary, but the decision should be documented and testable.

## 4.3.5 — Avoid expensive or dangerous import-time work

A module can be imported by a test runner, a command-line tool, or a different program. Importing it should not unexpectedly:

- Start a continuous monitoring loop.
- Open a network connection or scan a machine.
- Read credentials from a user's home directory.
- Rewrite files or change system configuration.

Place such actions behind an explicit entry point and a clearly named function. A `__main__` guard helps prevent accidental execution during imports, but it does not replace permission checks or input validation.

For Mother, observations should initially use **provided sample data**, not real system collection. Future real collection must be bounded, documented, and designed around least privilege: access only the data necessary for a defined task.

## 4.3.6 — Imports, dependencies, and environment management

A Python module is not automatically a third-party dependency. `math`, `pathlib`, and `json` are standard-library modules; they come with the chosen Python installation. A local module is source code belonging to your project. A third-party package is distributed separately and may require installation.

A virtual environment (`venv`) isolates installed Python packages for a project, but it is **not** an operating-system security sandbox. It does not prevent a script from accessing files or networks permitted to the running user.

Avoid installing packages to solve problems already covered by the standard library, especially at this early stage. We'll study reproducible dependency management and packaging in later modules.

## 4.3.7 — An initial design for Mother

One possible **illustrative** dependency direction is:

```text
__main__ → observations → validation
         → evaluation
         → reporting
```

The entry point orchestrates. Observation input can be sample data. Evaluation computes classifications. Reporting formats output. Validation defines accepted input shapes and ranges, if needed. This is not a mandate to create exactly four files; the module boundaries must serve the requirements rather than the other way around.

Ask of every module:

1. What responsibility does it own?
2. What input does it require?
3. What output does it provide?
4. Can its import perform unintended work?
5. Who owns objects it might mutate?

## Self-review

1. Why does a module depend on another when it imports it?
2. Why can a circular import produce a partially initialized module error?
3. If evaluation returns `True`, which component should decide how to display that result?
4. Does a `venv` stop Python from accessing user files?
5. What could go wrong if importing an observation module immediately starts a monitoring loop?

## Further reading

- [Python Reference — The import system](https://docs.python.org/3/reference/import.html)
- [Python Tutorial — Modules](https://docs.python.org/3/tutorial/modules.html)
- [Python Library Reference — `venv`](https://docs.python.org/3/library/venv.html)

**Practice:** `exercises/lesson03/lesson03.md`  
**Next:** Module 04 final assessment.
