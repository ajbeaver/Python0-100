# Lesson 4.2 — Packages, Import Paths, and Project Layout

**Course:** Python 0–100, Edition 2  
**Module:** 04 — Modules, Packages, and Imports  
**Prerequisites:** Lesson 4.1  
**Estimated study time:** 60–80 minutes

## Learning objectives

After this lesson, you should be able to:

1. Explain the difference between a module and a package.
2. Recognize the role of `__init__.py` in an ordinary package.
3. Write unambiguous imports across sibling modules in a package.
4. Explain why direct execution of a package's internal file may break relative imports.
5. Run a package entry point with `python3 -m` from a known project root.

---

## 4.2.1 — Organizing related modules

A **package** groups related modules under a shared import name. An ordinary package is represented by a directory containing an `__init__.py` file. Python also supports *namespace packages* without that file, but we will use ordinary packages while learning.

Consider:

```text
workspace/
├── mother/
│   ├── __init__.py
│   ├── evaluation.py
│   └── reporting.py
└── run.py
```

Inside `run.py`, when invoked from an appropriate project environment, you can write:

```python
from mother import evaluation
```

This refers to the `mother.evaluation` module. Package names are not merely decorative: they provide qualification, so unrelated projects can have modules with similar short filenames without necessarily sharing an import name.

`__init__.py` is executed when an ordinary package is initialized by import. For now, keep it empty to avoid hidden initialization work. Later, package API design may justify selective exports from it.

## 4.2.2 — Absolute versus relative imports

An **absolute import** starts with the top-level package name:

```python
from mother import evaluation
```

Inside `mother/reporting.py`, a **relative import** can reference a sibling:

```python
from . import evaluation
```

The single dot means the current package. Two dots move up one package level in a package hierarchy. Relative imports require a known package context; they are not general filesystem path syntax.

For this course, prefer **absolute imports** for clarity at first. Relative imports are useful within tightly coupled packages, but knowing why they work is more important than choosing a universal style.

## 4.2.3 — Module execution and `-m`

Consider a second structure:

```text
workspace/
└── mother/
    ├── __init__.py
    ├── __main__.py
    └── evaluation.py
```

If `mother/__main__.py` contains:

```python
from mother import evaluation

print(evaluation.is_high(85, 80))
```

and `evaluation.py` defines `is_high`, running from `workspace/` with:

```bash
python3 -m mother
```

executes the package's `__main__.py` as the entry point. `-m` asks Python to find the named module/package using the import machinery. It is a different operation from executing `python3 mother/__main__.py` as a file path.

**Important:** The example assumes `workspace/` is on Python's import search path, as it normally is when the command is run from that directory. Changing the working directory or execution mode can affect what Python finds.

Do not try to fix all import failures by adding arbitrary directories to `sys.path` inside source files. First check the project root, the command used, and the package layout.

## 4.2.4 — Where imports are found

Python's import system uses a sequence of finders and locations. For typical file-based imports, `sys.path` participates in locating top-level modules and packages. It commonly includes an execution-dependent first entry, installed package locations, and standard-library locations.

This explains why:

- The same code may import correctly from one directory and fail from another.
- Running a package's internal file by path can differ from `python3 -m package.module`.
- A local file called `json.py` can accidentally intercept an intended `import json`.

A `ModuleNotFoundError` means the requested module was not found using the active import configuration; it does not necessarily mean the source file is absent from the disk. Check how Python was launched and what module name you requested.

**Debugging order:** (1) confirm your working directory, (2) check the package tree and spellings, (3) check the interpreter/environment, (4) check how you invoked Python. Avoid mutating import paths until you understand the cause.

## 4.2.5 — Package names and `__init__.py`

A package has a `__path__` used to search for its submodules. Ordinary imports initialize the package before loading the requested submodule, which explains why work in `__init__.py` can affect every importer. An empty initializer gives the student projects a package boundary without those effects.

### What `__init__.py` does not do

It does not create an isolated process, protect secrets, or stop another module from importing internal code. A Python package is an **organizational and naming boundary**, not a security sandbox.

## 4.2.6 — Project structure: start small

A reasonable beginner project layout is:

```text
mother-project/
├── README.md
├── mother/
│   ├── __init__.py
│   ├── __main__.py
│   ├── observations.py
│   └── evaluation.py
└── tests/
```

This is an example, not yet Mother's canonical architecture. Keep temporary coursework separate from the eventual capstone repository. Design may change as requirements become clearer.

A minimal virtual environment (`python3 -m venv .venv`) is useful later for third-party dependencies, but **packages and imports do not require installing anything**. We can build this entire module using Python's standard library.

## 4.2.7 — Common failure modes

1. Running `python3 mother/evaluation.py` and expecting package-relative imports to resolve as if it were imported as `mother.evaluation`.
2. Assuming `__init__.py` must contain application logic.
3. Confusing a Python package with a separately installed distribution from a package index; related concepts, not identical.
4. Choosing generic project filenames that shadow standard-library modules.
5. Using imports that only work from an editor's special working directory.

## Self-review

1. What does `from mother import evaluation` mean, and which file might satisfy it?
2. When is `from . import evaluation` meaningful?
3. Why would `python3 -m mother` use `mother/__main__.py`?
4. Why might direct file execution produce a different import result?
5. Does putting a module inside `mother/` prevent other code from calling it?

## Further reading

- [Python Tutorial — Packages](https://docs.python.org/3/tutorial/modules.html#packages)
- [Python Reference — Regular packages](https://docs.python.org/3/reference/import.html#regular-packages)
- [Python Library Reference — `__main__`](https://docs.python.org/3/library/__main__.html)

**Practice:** `exercises/lesson02/lesson02.md`  
**Next lesson:** 4.3 — Dependencies, Interfaces, and Import-Time Safety.
