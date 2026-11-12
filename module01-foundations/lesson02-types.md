# Lesson 1.2 — Types, Expressions, and Operators

**Course:** Python 0–100, Edition 2  
**Module:** 01 — Python Foundations  
**Prerequisites:** Lesson 1.1  
**Estimated study time:** 45–60 minutes

## Learning objectives

By the end of this lesson, you should be able to:

1. Identify basic Python types and explain dynamic typing.
2. Distinguish statements from expressions.
3. Use arithmetic, comparison, and logical operators correctly.
4. Predict Boolean results, including short-circuit behavior.
5. Decide when a direct expression is clearer than a conditional statement.

---

## 1.2.1 — Types belong to objects

Python is dynamically typed: a name is not permanently restricted to one type. Objects have types, and names may be rebound to objects of different types.

```python
status = "online"
status = 200
```

Both assignments are valid. The first binds `status` to a `str` object; the second binds it to an `int` object. This does not mean all types are interchangeable. Python is strongly typed: incompatible operations usually raise errors instead of silently guessing a conversion.

| Type | Example | Purpose |
|---|---|---|
| `str` | `"mother"` | Text |
| `int` | `75` | Whole numbers |
| `float` | `75.5` | Floating-point numbers |
| `bool` | `True` | Boolean truth values |
| `NoneType` | `None` | Absence of a value |

You can inspect an object's type with `type()`:

```python
print(type(75))
print(type("75"))
```

The integer `75` and the string `"75"` are not the same kind of object. The `bool` type has the values `True` and `False`; note their capitalization.

### Conversions are explicit when needed

```python
reading = "75"
reading_number = int(reading)
```

The conversion succeeds because the text represents an integer. Converting arbitrary text such as `"high"` with `int()` raises `ValueError`. A conversion can fail; it is not merely a visual change.

---

## 1.2.2 — Statements and expressions

An **expression** is evaluated to produce a value:

```python
10 + 5
cpu > 80
```

An **assignment statement** binds a name to an evaluated result:

```python
result = 10 + 5
warning = cpu > 80
```

In both cases Python evaluates the right-hand expression first. Then the assignment binds the name on the left to the resulting object.

This matters because when the task is *calculate a value*, the expression often already gives you the answer. You do not need to build a separate `if`/`else` solely to assign `True` or `False`.

---

## 1.2.3 — Arithmetic operators

| Operator | Meaning | Example | Result |
|---|---|---|---|
| `+` | Addition | `10 + 3` | `13` |
| `-` | Subtraction | `10 - 3` | `7` |
| `*` | Multiplication | `10 * 3` | `30` |
| `/` | True division | `10 / 2` | `5.0` |
| `//` | Floor division | `10 // 3` | `3` |
| `%` | Modulo / remainder | `10 % 3` | `1` |
| `**` | Exponentiation | `10 ** 3` | `1000` |

Three distinctions are worth remembering:

1. `/` produces a floating-point result for ordinary integer operands, even when the answer is a whole number.
2. `//` rounds toward negative infinity: `-10 // 3` is `-4`, not `-3`.
3. `%` can check whether a value is divisible by another: `10 % 2 == 0` is `True`.

Division by zero raises `ZeroDivisionError`. This is different from a legitimate measured CPU utilization of `0`: zero data and *no measurements* are different situations.

Python follows operator precedence. Parentheses make grouping explicit and often improve readability:

```python
average = (60 + 70 + 80) / 3
```

---

## 1.2.4 — Comparison operators

Comparisons yield Boolean values.

| Operator | Meaning |
|---|---|
| `==` | Values are equal |
| `!=` | Values are not equal |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal |
| `<=` | Less than or equal |

Do not confuse assignment (`=`) with equality comparison (`==`).

```python
cpu = 80
cpu == 80
```

The first line binds a name. The second evaluates to `True`, but does not change `cpu`.

### Equality versus identity

`==` asks whether values compare equal; `is` asks whether two references identify the exact same object.

```python
a = [1, 2]
b = [1, 2]

print(a == b)
print(a is b)
```

This prints `True` followed by `False`. The lists contain equal elements but are separate objects. Use `==` for ordinary value comparisons. Identity checks are appropriate for special sentinel objects, especially `None` (`value is None`).

---

## 1.2.5 — Boolean operators and short-circuiting

Python uses `and`, `or`, and `not` to combine conditions.

```python
cpu = 75
memory = 82

cpu_warning = cpu > 80
memory_warning = memory > 80
system_warning = cpu_warning or memory_warning
```

`system_warning` is `True` because the memory condition is true. This is a direct calculation of a Boolean value, not a conditional action.

### Short-circuit evaluation

For `A or B`, Python need not evaluate `B` if `A` is already truthy. For `A and B`, Python need not evaluate `B` if `A` is falsy.

```python
memory_warning = True
system_warning = memory_warning or undefined_warning
```

This expression can succeed despite the undefined name because Python does not evaluate its right operand. That can hide a bug until the left operand changes.

A precise detail: `and` and `or` return one of their *operands*, which need not be a Boolean. In this lesson our operands are comparisons and Boolean values, so the results are Boolean.

### Truthiness

Python considers empty strings, empty lists, empty dictionaries, `0`, and `None` falsy. A nonempty string such as `"False"` is truthy. We'll use truthiness for collections and conditional statements in Module 02.

---

## 1.2.6 — Calculation versus action

Consider two responsibilities in Mother's monitoring system:

- **Compute whether memory is above a threshold.** A comparison produces the Boolean directly.
- **Print a warning when that condition is true.** A conditional statement controls the action.

```python
memory_warning = memory > 80

if memory_warning:
    print("Memory warning")
```

The `if` is useful because printing is conditional. It is not necessary simply to assign the warning value.

**Design principle:** Use expressions for values and control flow for choosing operations. Avoid constructing extra mutable state for a calculation that can be expressed directly.

---

## Summary

- Python names may reference objects of different types over time.
- Expressions evaluate to values; assignment binds names to those values.
- Comparisons produce Booleans, including values at exact threshold boundaries.
- `and` and `or` short-circuit and can return operands rather than strict Booleans.
- Prefer direct Boolean assignments when calculating warning flags.

## Exercise specification

Complete `exercises/lesson02/lesson02.md`. Create your own `lesson02.py` beginning empty.

## References

- [Python Tutorial — Numbers](https://docs.python.org/3/tutorial/introduction.html#numbers)
- [Python Language Reference — Expressions](https://docs.python.org/3/reference/expressions.html)
- [LearnPython.org](https://www.learnpython.org/)

**Status:** Textbook prepared; exercises not assessed.  
**Next lesson:** 1.3 — Strings and Collections
