# Lesson 5.1 — Classes, instances, and invariants

**Course:** Python 0–100, Edition 2  
**Module:** [05 — Object-Oriented Programming](README.md)  
**Prerequisite:** Module 04  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Explain instance attributes and method binding.
2. Construct independent objects.
3. Keep an invariant true across updates.

## 5.1.1 — Objects combine state with operations

A class defines a kind of object. An instance is one particular object created from it. Calling `Meter(20)` creates an instance and runs its initializer. `self` is the instance supplied to a bound method; it is a naming convention, not a special keyword.

```python
class Meter:
    def __init__(self, value):
        self.set_value(value)

    def set_value(self, value):
        if type(value) is not int:
            raise TypeError("value must be an integer")
        if not 0 <= value <= 100:
            raise ValueError("value must be between 0 and 100")
        self._value = value

    def is_high(self, threshold):
        return self._value > threshold

first = Meter(82)
second = Meter(20)
print(first.is_high(80), second.is_high(80))
```

Predict the output before running this in an empty scratch file. The two objects have separate `_value` attributes. `first.is_high(80)` supplies `first` as `self`; the explicit argument supplies `threshold`.

## 5.1.2 — Trace construction and a method call

Calling `Meter(82)` is an operation, not a declaration that every meter has the value 82. Python creates an instance, then the initializer calls `self.set_value(82)`. That method validates its argument before assigning an attribute on this particular instance. A second call, `Meter(20)`, repeats the process for another instance.

The distinction between a **parameter** and an **attribute** matters here. `value` is a local parameter name for one call. `self._value` is an attribute reachable through the instance after the call returns. The object retains the reading; the parameter binding does not need to survive.

| Operation | First instance's stored value | Second instance's stored value |
|---|---:|---:|
| Construct both | 82 | 20 |
| `first.set_value(60)` | 60 | 20 |
| Attempt `first.set_value(101)` | 60 | 20 |

The last row follows from statement order, not from automatic rollback. Python does not undo earlier assignments when an exception is raised. If a method changed three attributes and rejected the fourth input afterward, the first three changes would remain. Validate the proposed state before committing a multi-field change.

## 5.1.3 — Validate before mutation

An **invariant** is a condition intended to remain true throughout an object's valid lifetime. Here every stored value is an integer in range. The exact type check deliberately excludes `True`, even though `bool` is a subclass of `int`. `raise` interrupts the normal path with an exception. Invalid construction returns no usable instance; an invalid update leaves the previous value intact because assignment comes last.

## 5.1.4 — Observe a rejected operation

An exception stops the current normal path unless a caller catches it. Catch the specific error you expect when deliberately demonstrating bad input:

```python
try:
    int("not a number")
except ValueError as exc:
    print(type(exc).__name__)
```

This prints `ValueError` and then execution continues after the handler. Use the same structure around an intentional invalid update, catching `TypeError` for a wrong type and `ValueError` for a bad range. Do not wrap an entire demonstration in one broad catch: you want each rejected operation and its unchanged state to remain observable. Module 06 develops exception boundaries and cause handling further.

A leading underscore means “internal implementation”; it does not enforce privacy. A caller can still assign `_value` directly. Document the supported interface and test it. Do not promise that naming conventions provide a security boundary.

## 5.1.5 — Class attributes have a different lifetime

Putting `readings = []` directly inside a class body creates one list associated with the class, shared through ordinary instance lookup. Creating `self.readings = []` inside `__init__` creates a fresh list per instance. This is Module 03's ownership problem expressed through objects.

Mother needs objects only where identity, state, or an invariant makes them useful. A stateless threshold calculation can remain a function.

## 5.1.6 — When a class earns its place

With functions and a dictionary, you could represent the same reading. The class is useful when callers benefit from one supported place to construct and update valid state. It makes the owner and operations visible. It does not make all state safe automatically: callers still need to respect the interface, and methods must preserve their promises.

Mother's history will eventually need an owner because additions persist across operations. The simple calculation `cpu > threshold` has no comparable object lifetime to manage. Keeping it as a function avoids an object whose only purpose is to hold a method.

**Design principle:** Use an instance to represent a meaningful lifetime or state boundary, and trace every mutation through that boundary.

## Self-review

Why is validation after assignment too late? Which restriction would a leading underscore fail to enforce?

## Practice

Complete the [exercise specification](exercises/lesson01/lesson01.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson01-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Python tutorial — Classes](https://docs.python.org/3/tutorial/classes.html)
