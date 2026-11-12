# Lesson 3.2 — References, Aliasing, and Mutable Arguments

**Course:** Python 0–100, Edition 2  
**Module:** 03 — Scope, References, and State  
**Prerequisites:** Lesson 3.1  
**Estimated study time:** 50–65 minutes

## Learning objectives

1. Explain object identity and aliasing.
2. Distinguish mutation from rebinding when an object is passed to a function.
3. Predict the behavior of list and dictionary arguments.
4. Recognize the limitations of shallow copies when collections are nested.
5. State a clear expectation about whether a function will modify caller-owned inputs.

---

## 3.2.1 — References and identity

```python
history = [60, 70]
alias = history
```

Both names refer to the same list object. This relationship is called **aliasing**. `history is alias` evaluates to `True`; `is` checks identity, not just equal contents.

By contrast:

```python
first = [60, 70]
second = [60, 70]
```

`first == second` is `True` because the contents are equal, while `first is second` is `False` because these list literals create separate lists.

Avoid using `is` to compare ordinary numeric or string values. Use `==` for value equality. Identity checks are appropriate when identity itself matters, particularly `value is None`.

## 3.2.2 — Functions receive object references

Python calls bind function parameter names to the objects passed by the caller. It does **not** automatically duplicate lists or dictionaries.

Consider a function that mutates its input:

```python
def add_reading(history, reading):
    history.append(reading)

cpu_history = [60, 70]
add_reading(cpu_history, 80)
```

The list referred to by `cpu_history` now contains three readings. `history` was local to the function, but both names referred to the same mutable object.

**Key distinction:** Local names do not imply locally owned objects.

## 3.2.3 — Rebinding doesn't alter the caller's name

Compare this function:

```python
def add_reading(history, reading):
    history = history + [reading]
    return history

cpu_history = [60, 70]
updated = add_reading(cpu_history, 80)
```

`cpu_history` still refers to the original two-element list. `updated` refers to a new list. Inside the function, the local name `history` was rebound, not used to mutate the original list.

A function can also modify an object and then rebind its local name later. Always reason about *which object* a statement affects.

### Augmented assignment nuance

For immutable integers, `total += value` produces a new integer result and rebinds `total`. For mutable objects, augmented assignment may mutate in place. For example, a list's `+=` generally extends that list in place. Do not assume `+=` and `x = x + y` have identical identity effects for all types.

## 3.2.4 — Returning new data or changing existing data?

Two different contracts are possible:

- **Mutating contract:** "Append this reading to the supplied history." The caller expects its list to change.
- **Non-mutating contract:** "Produce an updated history." The caller receives a new list while the original remains unchanged.

Neither is universally correct. A monitoring collector may intentionally append to a history it owns. A statistics function should ordinarily read measurements without changing them.

The choice should be deliberate and predictable from the function name, documentation, and tests.

## 3.2.5 — Copies and nested collections

You can make a shallow list copy:

```python
copy_of_history = history.copy()
```

For a flat list of numbers, this gives you a separate outer list. But when the list contains dictionaries, the dictionaries themselves are still shared:

```python
observations = [{"cpu": 60}]
copied = observations.copy()
copied[0]["cpu"] = 95
```

Now `observations[0]["cpu"]` is also `95`. The outer lists are distinct, but their first elements reference the same dictionary object.

A **shallow copy** duplicates the outer container and retains references to nested objects. A **deep copy** recursively copies contained objects where possible; Python provides `copy.deepcopy()` for that purpose, but deep copying is not always necessary or desirable.

A frequent alternative is to create a fresh record each time you capture an observation, rather than copying an entire history.

## 3.2.6 — Mutable default arguments

This behavior is worth recognizing before it becomes a bug:

```python
def record(reading, history=[]):
    history.append(reading)
    return history
```

The default list is created when the function is defined, not once per call. Calls that omit `history` share that list, causing unexpected accumulation across calls.

When you later need an optional list parameter, the conventional pattern is to default to `None` and create a new list inside the function. We will practice that pattern after learning more about function interfaces.

---

## Design connection — Historical integrity

Mother should distinguish a newly captured observation from older stored observations. Accidentally reusing one mutable dictionary for every time step can cause every historical entry to change when the latest reading changes.

**Design principle:** Decide who owns mutable data and whether each function reads, modifies, or copies it. A local parameter name does not isolate an object from its caller.

## Self-review checkpoint

Predict before executing:

1. If a function calls `.append()` on its list argument, does the caller observe the change?
2. If a function assigns a different list to its parameter name, does that rebind the caller's variable?
3. Why doesn't `observations.copy()` protect nested dictionaries from mutation?

## Further reading

- [Python FAQ — How do I write a function with output parameters?](https://docs.python.org/3/faq/programming.html#how-do-i-write-a-function-with-output-parameters-call-by-reference)
- [Python Library Reference — copy](https://docs.python.org/3/library/copy.html)

**Exercise:** `exercises/lesson02/lesson02.md`  
**Next lesson:** 3.3 — State Lifetime, Side Effects, and Isolation.
