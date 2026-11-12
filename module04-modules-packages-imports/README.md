# Module 04 — Modules, Packages, and Imports

**Course:** Python 0–100, Edition 2  
**Prerequisites:** Modules 01–03  
**Status:** Not started

## Purpose

The first three modules used mainly standalone scripts. Real Python programs split responsibilities across files. That introduces names that belong to modules, import execution, package boundaries, and a choice of which file serves as an entry point.

The goal is to **design and navigate a small multi-file Python program**, not merely memorize `import` syntax. We will use only the standard library and Python's built-in package mechanisms.

## Textbook

| Lesson | File | Focus |
|---|---|---|
| 4.1 | `lesson01-modules.md` | Modules, namespace boundaries, imports, script execution |
| 4.2 | `lesson02-packages.md` | Packages, absolute/relative imports, project layout, import path |
| 4.3 | `lesson03-dependencies.md` | Module design, cycles, dependencies, entry points, testing seams |

## Coursework layout

```text
module04-modules-packages-imports/
├── README.md
├── lesson01-modules.md
├── lesson02-packages.md
├── lesson03-dependencies.md
├── assessment.md
└── exercises/
    ├── lesson01/lesson01.md
    ├── lesson02/lesson02.md
    └── lesson03/lesson03.md
```

No completed `.py` files, prefilled solutions, or answer key are supplied. Each exercise specifies which source files you create, starting empty. Any sample data is shown in the specification, not hidden in a Python stub. Keep written responses in a separate Markdown file.

## Progression

1. Explain the difference between importing a module and running a script.
2. Use explicit module boundaries to separate data transformation from presentation.
3. Build a small package with reliable imports when launched from the project root.
4. Recognize circular dependencies, import-time side effects, and global mutable state.
5. Describe how a stable, narrowly scoped interface makes components easier to test and replace.

## Mother connection

Mother will eventually gather observations, assess them, retain history, and perhaps learn patterns. We will not implement real monitoring or machine learning yet. This module establishes a structure where those responsibilities can be kept separate and inspected without acquiring network or system privileges.

## Completion

Complete the three lesson exercises in order. Then submit `assessment.md` responses and the independent implementation assignments. The assessment totals 100 points: 30 points multiple-choice, 50 points implementations, and 20 points design reasoning.

## References

- [Python Tutorial — Modules](https://docs.python.org/3/tutorial/modules.html)
- [Python Tutorial — Packages](https://docs.python.org/3/tutorial/modules.html#packages)
- [Python Language Reference — The import system](https://docs.python.org/3/reference/import.html)
- [Python Library Reference — `__main__`](https://docs.python.org/3/library/__main__.html)
- [Python Library Reference — `venv`](https://docs.python.org/3/library/venv.html)

**Next module:** 05 — Object-Oriented Programming.
