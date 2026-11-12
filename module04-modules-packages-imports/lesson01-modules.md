# Lesson 4.1 — Modules, Imports, and Execution

**Course:** Python 0–100, Edition 2  
**Module:** 04 — Modules, Packages, and Imports  
**Prerequisites:** Module 03  
**Estimated study time:** 55–75 minutes

## Learning objectives

After this lesson, you should be able to:

1. Explain what a module is and how its namespace differs from a function's local namespace.
2. Distinguish `import module` from `from module import name`.
3. Describe what happens when Python imports a module for the first time in a process.
4. Explain how `__name__` and the `if __name__ == "__main__":` guard work.
5. Explain why computation on import creates architectural problems.

---

## 4.1.1 — A file becomes a module

A **module** is a unit of Python code with its own namespace. In ordinary projects, each `.py` source file can be loaded as a module. Splitting code into files does not automatically make good architecture; each module should have a clear responsibility.

Suppose `metrics.py` contains:

```python
DEFAULT_THRESHOLD = 80

def is_high(value, threshold):
    return value > threshold
```

And `app.py` contains:

```python
import metrics

print(metrics.is_high(85, metrics.DEFAULT_THRESHOLD))
```

`metrics` is a name bound in `app.py` to a module object. `metrics.is_high` looks up the function in that module's namespace. The `.` is **attribute access**. An attribute is a named value accessible on an object; modules expose their top-level definitions as attributes.

This creates a visible boundary: `app.py` knows where the function came from. Contrast a long single script, where functions, I/O, and configuration may be interwoven.

## 4.1.2 — Import forms

The common forms are:

```python
import math
print(math.sqrt(81))
```

```python
from math import sqrt
print(sqrt(81))
```

```python
import math as mathematics
print(mathematics.sqrt(81))
```

The first binds `math` in the importing module. The second binds the name `sqrt` directly in the importing module. The third binds an alias, `mathematics`. All three access the standard-library module; the difference is which names become available locally.

**Prefer explicit module imports** when origin matters or name collisions are possible. `from module import function` is also valid when the imported name is clear.

Avoid `from math import *` in course projects. Star imports obscure which module owns a name and make static reading harder.

### Importing does not copy a module's entire source code into another file

`import metrics` is not textual inclusion. Python finds or retrieves a module object and binds that object to a name in the importing module. The imported module retains its own global namespace.

## 4.1.3 — Import executes top-level statements

On a module's first ordinary import in a process, Python generally locates the module, creates/initializes a module object, and executes its top-level statements. That includes function and class *definitions* (which create function/class objects) and any other top-level statements.

If `metrics.py` contains:

```python
print("Loading metrics")

DEFAULT_THRESHOLD = 80

def is_high(value, threshold):
    return value > threshold
```

Importing it normally prints `Loading metrics`. Python usually caches the resulting module object in `sys.modules`; another ordinary import of the same fully qualified name typically reuses it rather than rerunning its top-level code. This cache is for a **running Python process**, not permanent storage.

This matters because imports can have side effects. A top-level command that deletes a file, opens a network connection, or reads private state would run merely because another module imported it.

**Design rule:** Define functions, constants, and classes at module level; put operational work behind explicit function calls or an entry-point guard.

## 4.1.4 — Running versus importing: `__name__`

Python provides a module-global attribute named `__name__`.

- When a source file is imported normally as `metrics`, its `__name__` is `"metrics"`.
- When a source file is executed as the main program, its execution namespace has `__name__ == "__main__"`.

A common entry-point pattern is:

```python
def main():
    print("Application started")

if __name__ == "__main__":
    main()
```

Running the file directly calls `main()`. Importing the module makes `main` available without automatically calling it.

The guard does **not** protect all top-level code in the file. Only code inside the guarded suite is conditional. It also does not provide a security boundary: callers may still invoke public functions.

Why a `main()` function? It gives execution a named boundary, permits direct calls during tests, and avoids putting operational logic in the global namespace. It is a useful convention, not a universal requirement for every tiny script.

## 4.1.5 — Imported names and reassignment

Consider a module with `THRESHOLD = 80` and these two approaches:

```python
import settings

print(settings.THRESHOLD)
```

```python
from settings import THRESHOLD

print(THRESHOLD)
```

The second import binds `THRESHOLD` in the *importing* namespace to the object referenced at import time. If `settings.THRESHOLD` is later rebound, the local `THRESHOLD` binding is not automatically updated. This is the same rebinding concept studied in Modules 01 and 03.

For mutable objects, importing a name can expose the same shared object, so mutation can be visible across modules. Avoid treating imported mutable lists or dictionaries as private application state.

## 4.1.6 — A first architecture boundary

For Mother, think about three distinct jobs:

- **Acquire** an observation, perhaps initially from predefined sample data.
- **Evaluate** whether an observation crosses a threshold.
- **Present** a result to a user.

Even before using packages, you could put evaluation logic in a dedicated module and keep printing in the entry script. A function that calculates a Boolean should not need to print, read from the terminal, or connect to a sensor. That makes it simpler to reuse in later lessons.

The number of files is not a measure of quality. Introduce a new module when it creates a meaningful, understandable responsibility boundary.

## 4.1.7 — Mistakes to watch for

1. Treating `import` as a harmless declaration even when imported code performs work at top level.
2. Forgetting that each file has its own namespace.
3. Assuming `from settings import VALUE` creates a live connection to all future reassignments.
4. Naming your own file `math.py`, `json.py`, or `random.py`, which can shadow an intended library module.
5. Assuming a `__main__` guard is required inside every helper module; usually it is not.

## Self-review

Without executing code, answer:

1. Why doesn't `import metrics` automatically call the body of `metrics.is_high()`?
2. What is the difference between `import metrics` and `from metrics import is_high`?
3. Why is `print("Hello")` at module top level different from `print("Hello")` inside `main()` guarded by `__name__`?
4. Why is it risky to import a module that automatically modifies a log file?

## Further reading

- [The Python Tutorial — Modules](https://docs.python.org/3/tutorial/modules.html)
- [The `__main__` environment](https://docs.python.org/3/library/__main__.html)
- [The import system](https://docs.python.org/3/reference/import.html)

**Practice:** `exercises/lesson01/lesson01.md`  
**Next lesson:** 4.2 — Packages, Import Paths, and Project Layout.
