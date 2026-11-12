# Lesson 1.3 — Strings, Lists, and Dictionaries

**Course:** Python 0–100, Edition 2  
**Module:** 01 — Python Foundations  
**Prerequisites:** Lessons 1.1–1.2  
**Estimated study time:** 50–65 minutes

## Learning objectives

By the end of this lesson, you should be able to:

1. Retrieve portions of strings using indexing and slicing.
2. Create, read, and modify lists.
3. Read and update dictionary values by key.
4. Explain how mutable collections behave when shared by multiple names.
5. Model chronological environmental observations with combined structures.

---

## 1.3.1 — Strings are immutable sequences

A string (`str`) is an immutable sequence of Unicode characters:

```python
device = "node01"
```

Each position has a zero-based index.

| Character | `n` | `o` | `d` | `e` | `0` | `1` |
|---|---|---|---|---|---|---|
| Index | 0 | 1 | 2 | 3 | 4 | 5 |

```python
print(device[0])
print(device[-1])
print(device[0:4])
```

These expressions produce `"n"`, `"1"`, and `"node"`, respectively. Negative indices count backward from the end. A slice uses `[start:stop]`: the starting position is included; the stopping position is excluded.

A string cannot be changed in place:

```python
device[0] = "N"  # TypeError
```

Construct a new string instead:

```python
device = "N" + device[1:]
```

This creates a new string and rebinds `device` to it; the original string is not mutated.

### String formatting

An f-string includes values in text by placing expressions in braces:

```python
cpu = 75
print(f"CPU: {cpu}")
```

Without a `{...}` expression, the `f` prefix does nothing useful. You can also pass several arguments to `print()` with commas. Printing displays data; it does not store or return the displayed value.

---

## 1.3.2 — Lists are ordered and mutable

A list stores an ordered collection of references:

```python
cpu_history = [60, 65, 70]
```

Indexing and slicing work on lists much as they do on strings:

```python
print(cpu_history[0])
print(cpu_history[-1])
print(cpu_history[1:])
```

The `append()` method adds one element to the existing list:

```python
cpu_history.append(75)
```

Lists may contain different object types, but consistent data types usually make a measurement series easier to understand and process.

### Mutation versus rebinding revisited

```python
a = [1, 2]
b = a

a.append(3)
```

`a` and `b` now both expose `[1, 2, 3]` because the same list was mutated.

```python
a = a + [4]
```

This constructs another list and rebinds `a`. `b` remains bound to the earlier list `[1, 2, 3]`. Both statements can make `a` *look* longer; they are not the same operation.

### Useful built-ins

`len(cpu_history)` returns the number of elements. It is useful when the number of observations is not fixed. The exact length may change at runtime, so avoid writing logic that assumes a permanent number of entries.

---

## 1.3.3 — Dictionaries use meaningful keys

A dictionary (`dict`) associates keys with values:

```python
environment = {
    "cpu": 75,
    "memory": 82,
    "temperature": 68
}
```

Use the key to retrieve a measurement:

```python
print(environment["cpu"])
```

Use assignment with the key to update it:

```python
environment["cpu"] = 85
```

This changes the dictionary's contents, rather than rebinding the name `environment` to another dictionary.

If a requested key is absent, bracket access raises `KeyError`:

```python
print(environment["disk"])  # KeyError
```

The `.get()` method can instead return `None` for a missing key:

```python
print(environment.get("disk"))
```

`None` represents absence of a value; it is not the same as a measured value of `0`.

Dictionary keys are unique and must be hashable. Strings are commonly used as descriptive keys. Dictionaries preserve insertion order in modern Python, but key-based lookup—not positional indexing—is their central purpose.

---

## 1.3.4 — Choosing a structure is a modeling decision

Imagine Mother has CPU readings over time:

```python
cpu_history = [60, 65, 70, 75]
```

A list fits because order matters and the entries form a series.

Now imagine Mother has one observation containing different kinds of measurements:

```python
observation = {
    "cpu": 75,
    "memory": 82,
    "temperature": 68
}
```

A dictionary fits because the data needs labeled meanings. A list `[75, 82, 68]` would force you to memorize what each position means.

To represent *multiple complete observations in chronological order*, combine a list and dictionaries:

```python
history = [
    {"cpu": 60, "memory": 70, "temperature": 65},
    {"cpu": 75, "memory": 82, "temperature": 68}
]
```

The list supplies sequence; each dictionary supplies named fields.

```python
print(history[1]["cpu"])
```

Read this from left to right: select the second observation using index `1`, then retrieve its CPU value using the key `"cpu"`.

### Data integrity warning

Distinct observations should be stored as distinct records. If you append the *same mutable dictionary object* repeatedly and later change it, earlier positions in your history can appear to change too. We will explore shared mutable state formally in Module 03.

---

## 1.3.5 — Design decisions for Mother

At this stage, we want to represent data, not build an entire monitoring framework. Avoid adding classes, files, imports, or elaborate abstractions before the requirements call for them.

A sensible first model might have:

- One string for a device identifier.
- One dictionary for the current observation.
- One list for a chronological series of observations.

Later modules will teach us to process this data with loops, divide responsibilities into functions, and persist observations to disk.

**Design principle:** Choose the structure that makes relationships and operations obvious to the next person reading the program—including your future self.

---

## Summary

- Strings are immutable sequences that support indexing and slicing.
- Lists are ordered mutable sequences; `append()` changes the existing list.
- Dictionaries map meaningful keys to values; missing keys require care.
- Nested structures can express both time order and field names.
- Repeated references to one mutable object can corrupt the meaning of historical snapshots.

## Exercise specification

Complete `exercises/lesson03/lesson03.md`. Create your own `lesson03.py` beginning empty.

## References

- [Python Tutorial — Lists](https://docs.python.org/3/tutorial/introduction.html#lists)
- [Python Tutorial — Dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries)
- [Python Language Reference — Data Model](https://docs.python.org/3/reference/datamodel.html)

**Status:** Textbook prepared; exercises not assessed.  
**Next step:** Module 01 assessment, after exercises are reviewed.
