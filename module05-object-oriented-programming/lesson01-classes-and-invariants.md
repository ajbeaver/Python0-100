# Lesson 5.1 — Classes, instances, and invariants

**Course:** Python 0–100, Edition 2  
**Module:** [05 — Object-Oriented Programming](README.md)  
**Prerequisite:** Module 04  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Explain instance attributes and method binding.
2. Construct independent objects.
3. Keep an invariant true across updates.

### Objects combine state with operations

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

### Validate before mutation

An **invariant** is a condition intended to remain true throughout an object's valid lifetime. Here every stored value is an integer in range. The exact type check deliberately excludes `True`, even though `bool` is a subclass of `int`. `raise` interrupts the normal path with an exception. Invalid construction returns no usable instance; an invalid update leaves the previous value intact because assignment comes last.

### Observe a rejected operation

An exception stops the current normal path unless a caller catches it. Catch the specific error you expect when deliberately demonstrating bad input:

```python
try:
    int("not a number")
except ValueError as exc:
    print(type(exc).__name__)
```

This prints `ValueError` and then execution continues after the handler. Use the same structure around an intentional invalid update, catching `TypeError` for a wrong type and `ValueError` for a bad range. Do not wrap an entire demonstration in one broad catch: you want each rejected operation and its unchanged state to remain observable. Module 06 develops exception boundaries and cause handling further.

A leading underscore means “internal implementation”; it does not enforce privacy. A caller can still assign `_value` directly. Document the supported interface and test it. Do not promise that naming conventions provide a security boundary.

### Class attributes have a different lifetime

Putting `readings = []` directly inside a class body creates one list associated with the class, shared through ordinary instance lookup. Creating `self.readings = []` inside `__init__` creates a fresh list per instance. This is Module 03's ownership problem expressed through objects.

Mother needs objects only where identity, state, or an invariant makes them useful. A stateless threshold calculation can remain a function.

## Self-review

Why is validation after assignment too late? Which restriction would a leading underscore fail to enforce?

## Practice

Complete the [exercise specification](exercises/lesson01/lesson01.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson01-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/tutorial/classes.html)
