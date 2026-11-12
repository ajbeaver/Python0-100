# Module 04 — Final Assessment

**Course:** Python 0–100, Edition 2  
**Module:** 04 — Modules, Packages, and Imports  
**Total:** 100 points  
**Status:** Not submitted

## Rules

- Answer without checking solutions or an answer key.
- For prediction questions, write your prediction before running the example.
- You may use all concepts from Modules 01–04.
- Use only standard-library Python; do not install packages.
- Create all implementation files empty, then write the programs yourself.
- Keep code, answers, and assessment instructions in separate files.
- Unless otherwise stated, run package commands from each challenge's project root.

## Part A — Multiple choice (30 points)

Choose **one best answer** per question. Record answers in `assessment-answers.md`. Three points each.

### Question 1 — Module namespace

If `import math` succeeds, which expression accesses its `sqrt` function?

A. `sqrt.math(4)`  
B. `math.sqrt(4)`  
C. `math->sqrt(4)`  
D. `import.sqrt(4)`

### Question 2 — Imported name

What does `from math import sqrt` normally bind in the importing module?

A. A local name `sqrt` referring to the imported function  
B. A new file named `sqrt.py`  
C. A global alias named `math`  
D. The full source text of `math`

### Question 3 — Import-time execution

A module contains `print("loaded")` at top level. What usually happens on its first ordinary import in a Python process?

A. Nothing prints until a function is called  
B. The print runs while the module is initialized  
C. Python raises `SyntaxError`  
D. The print is moved to `__main__`

### Question 4 — Repeated imports

Under ordinary conditions, what prevents rerunning top-level statements every time the same module is imported by its same qualified name?

A. `sys.modules` reuse  
B. The operating system's firewall  
C. The presence of a `README.md`  
D. All imported files are converted to comments

### Question 5 — Entry point

Which condition is normally true for a file directly executed as the main program?

A. `__name__ == "__file__"`  
B. `__name__ == "__main__"`  
C. `__name__ == "root"`  
D. `__name__ is None`

### Question 6 — Ordinary package

Which file is conventionally present in an ordinary Python package directory?

A. `package.lock`  
B. `__main__.json`  
C. `__init__.py`  
D. `requirements.md`

### Question 7 — Package execution

Given a directory `observer/` with `__init__.py` and `__main__.py`, which command normally runs the package entry point from its parent directory?

A. `python3 observer`  
B. `python3 -m observer`  
C. `pip observer`  
D. `python3 import observer`

### Question 8 — Relative import

Within `observer/reporting.py`, what does `from . import evaluation` mean when imported with its package context intact?

A. Import the current shell directory as Python source  
B. Import a sibling named `evaluation` from the current package  
C. Execute `evaluation.py` as a separate process  
D. Install `evaluation` from the internet

### Question 9 — Circular dependencies

Why can circular imports fail?

A. Python disallows every function call between modules  
B. One module may request a name from another that has not finished initializing  
C. Package directory names may not contain letters  
D. Importing always creates a new operating-system process

### Question 10 — Virtual environments

Which statement about `venv` is correct?

A. It is an operating-system security sandbox  
B. It prevents Python from reading all user files  
C. It helps isolate installed Python dependencies, but does not restrict OS permissions  
D. It is mandatory for importing standard-library modules

---

## Part B — Independent implementations (50 points)

### Challenge 1 — Separate the calculation (15 points)

Create empty files:

```text
assessment-challenge01/
├── app.py
└── analysis.py
```

Use these sample values:

```python
cpu_history = [42, 55, 86, 90, 75]
threshold = 80
```

Requirements:

1. `analysis.py` defines a function returning the average of a **nonempty** history list.
2. It defines a second function returning how many observations are strictly above a supplied threshold.
3. `app.py` imports both functions, invokes them with the supplied data, and prints the results.
4. Importing `analysis` performs no reporting, and importing `app` does not automatically run its report.
5. The implementation must work without modifying the incoming list.

Evaluation: function contracts (5), correct calculations (5), imports/entry-point behavior (5).

### Challenge 2 — Package entry point (15 points)

Create empty files in this exact tree:

```text
assessment-challenge02/
└── sensor_report/
    ├── __init__.py
    ├── __main__.py
    ├── samples.py
    └── summary.py
```

Starting data:

```python
samples = [20, 25, 30, 40]
```

Requirements:

1. `samples.py` exposes the sample list without printing.
2. `summary.py` exposes a function that computes the minimum, maximum, and average from a nonempty sequence, returning the results in a representation you choose.
3. `__main__.py` calls the function and presents the three results.
4. `__init__.py` remains empty.
5. Running `python3 -m sensor_report` from `assessment-challenge02/` works.
6. Importing `sensor_report.summary` must not print the report.

Evaluation: package organization (5), computed results (5), command/import behavior (5).

### Challenge 3 — Fix the architecture (20 points)

Build the following application from **empty files** (not from existing source):

```text
assessment-challenge03/
└── mother_preview/
    ├── __init__.py
    ├── __main__.py
    ├── samples.py
    ├── classifier.py
    └── report.py
```

Sample data:

```python
observations = [
    {"device": "node01", "cpu": 55, "memory": 68},
    {"device": "node02", "cpu": 93, "memory": 74},
    {"device": "node03", "cpu": 78, "memory": 88},
]
threshold = 80
```

Requirements:

1. `samples.py` provides this fixed data without collecting live metrics.
2. `classifier.py` accepts a single observation and threshold and returns whether **either** CPU or memory exceeds the threshold. It must not print.
3. `report.py` converts a device identifier and classification into readable text; it must not print.
4. `__main__.py` coordinates the modules and prints a line for each device.
5. The package must run as `python3 -m mother_preview` from `assessment-challenge03/`.
6. No cyclic imports or unexpected import-time reporting.
7. Your design must not mutate the supplied observations.

Evaluation: clear responsibilities (5), correct classification (5), package behavior (5), state safety (5).

---

## Part C — Design reasoning (20 points)

Write responses in `assessment-answers.md`. Ten points each.

### Design question 1 — Diagnose an import failure

A student has:

```text
project/
└── observer/
    ├── __init__.py
    ├── __main__.py
    └── evaluation.py
```

They execute `python3 observer/__main__.py` from `project/`, but their code contains `from . import evaluation` and fails with an error about a relative import without a known parent package.

Explain **why** the execution mode creates this problem, identify a more appropriate command from `project/`, and explain why arbitrarily modifying `sys.path` is not the best first fix.

### Design question 2 — Boundaries and risk

An early Mother prototype has an `environment.py` module that opens a network connection, obtains live observations, appends to a global list, and prints warnings immediately when imported.

Identify **at least three** separate architectural or security problems. Propose a better division of responsibilities between modules, including how to keep the observation process opt-in and test with fixed sample data.

---

## Submission and grading

| Component | Points |
|---|---:|
| Multiple choice — 10 × 3 | 30 |
| Challenge 1 | 15 |
| Challenge 2 | 15 |
| Challenge 3 | 20 |
| Design questions — 2 × 10 | 20 |
| **Total** | **100** |

Submit:

- `assessment-answers.md`
- The three challenge directories and source files.

### Completion checklist

- [ ] Questions answered before checking program output.
- [ ] Every program runs from its specified project root.
- [ ] Helper modules are import-safe.
- [ ] No program uses third-party packages or live system collection.
- [ ] Design questions explain **why**, not only **what**.

**Status:** Not submitted
