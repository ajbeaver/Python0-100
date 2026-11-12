# Lesson 3.1 — Namespaces, Scope, and Name Resolution

**Course:** Python 0–100, Edition 2  
**Module:** 03 — Scope, References, and State  
**Prerequisites:** Module 02, Lesson 2.3  
**Estimated study time:** 45–60 minutes

## Learning objectives

By the end, you should be able to:

1. Distinguish an object from the name bound to it.
2. Identify local, enclosing, module-global, and built-in lookup locations.
3. Explain why assigning to a name inside a function usually makes it local.
4. Recognize `UnboundLocalError` and understand when `global` or `nonlocal` changes scope.
5. Describe why explicit arguments and return values are often easier to maintain than hidden dependencies.

---

## 3.1.1 — What is a namespace?

A **namespace** is a mapping from names to objects. Consider:

```python
cpu = 75
```

The name `cpu` is bound in some namespace. At the top level of a file, that namespace is the module's global namespace. Inside a function, parameter names and locally assigned names normally belong to that call's local namespace.

A name and an object are not the same thing. Two names may refer to one object. Rebinding a name changes that binding; it does not necessarily change the object.

## 3.1.2 — Function-local names

```python
def calculate_average(readings):
    total = 0
    for reading in readings:
        total += reading
    return total / len(readings)
```

`readings`, `total`, and `reading` are local names for a call of `calculate_average`. The `for` loop does not create a separate scope: its loop variable still belongs to the function's local namespace.

After this function finishes, its local names are no longer available for direct lookup by the caller. The **returned object** can still be used, however.

This makes function boundaries useful for reasoning: the caller supplies an input and receives an output instead of depending on internal names.

## 3.1.3 — Name lookup: LEGB

Python's commonly taught lookup model for ordinary names is **LEGB**:

- **L — Local:** names in the current function call.
- **E — Enclosing:** names in enclosing function scopes, if any.
- **G — Global:** names in the current module.
- **B — Built-in:** names made available by Python, such as `len` and `print`.

Consider:

```python
threshold = 80

def check(value):
    return value > threshold
```

The name `value` is local, while `threshold` is looked up in the module's namespace. This works, but it creates an implicit dependency: `check()` cannot be understood without knowing the module's current `threshold`.

Compare:

```python
def check(value, threshold):
    return value > threshold
```

The second function makes the dependency explicit. Either design can be valid, but the explicit version is easier to test with varying thresholds and reuse elsewhere.

**Important:** LEGB is a helpful model for ordinary names, not a complete model of every Python lookup (attribute access, for example, has its own rules).

## 3.1.4 — Assignment makes a name local

```python
cpu = 75

def update_cpu():
    cpu = 90

update_cpu()
print(cpu)
```

The final print displays `75`. Assignment inside `update_cpu` makes `cpu` a **local** name, leaving the module-level binding unchanged.

More surprisingly:

```python
count = 5

def increment():
    count += 1
```

Calling `increment()` raises `UnboundLocalError`. Because the function assigns to `count`, Python treats it as local throughout the function; `count += 1` tries to read that local name before a value has been bound to it.

This is different from a `NameError` for a name that Python cannot resolve at all. `UnboundLocalError` is a subclass of `NameError`.

## 3.1.5 — `global` and `nonlocal`

The `global` declaration instructs a function to bind a name in its module namespace:

```python
count = 0

def increment():
    global count
    count += 1
```

This works but couples the function to shared module state. Another caller or test can change `count`, potentially affecting the result.

For a nested function, `nonlocal` allows rebinding a name in the nearest appropriate enclosing function scope:

```python
def make_counter():
    count = 0

    def increment():
        nonlocal count
        count += 1
        return count

    return increment
```

You don't need to implement closures in this module. The purpose of the example is to show that **assignment targets a scope**, and scope is part of a program's behavior.

Prefer explicit inputs and outputs for simple calculations. Use deliberate shared state when it actually represents something shared.

## 3.1.6 — Built-ins can be shadowed

```python
len = 4
```

At module level, this name now shadows the built-in `len` during ordinary name lookup in that module. A later attempt to call `len(readings)` would fail because the local/global name refers to an integer, not the built-in function.

Use descriptive variable names such as `measurement_count` rather than reusing names of common built-ins.

---

## Design connection — Hidden dependencies

Suppose Mother's classification function reads thresholds from module-level names. Changing a threshold elsewhere changes how that function behaves without changing the call site.

If a function instead accepts thresholds through arguments, the function's dependencies are explicit. Later, a configuration module can supply values without forcing the classifier itself to own configuration.

**Design principle:** Scope determines where names are resolved; function contracts should reveal the data a calculation relies on.

## Self-review checkpoint

Without running code, explain:

1. Why does a `for` loop inside a function not make its counter global?
2. What causes `UnboundLocalError` in an augmented assignment?
3. When might a module-level constant be reasonable, even if mutable globals are not?

Do not confuse *local variable binding* with *immutability of objects*. We will tackle object sharing next.

## Further reading

- [Python Execution Model — Resolution of Names](https://docs.python.org/3/reference/executionmodel.html#resolution-of-names)
- [Python Tutorial — Scopes and Namespaces](https://docs.python.org/3/tutorial/classes.html#python-scopes-and-namespaces)

**Exercise:** `exercises/lesson01/lesson01.md`  
**Next lesson:** 3.2 — References, Aliasing, and Mutable Arguments.
