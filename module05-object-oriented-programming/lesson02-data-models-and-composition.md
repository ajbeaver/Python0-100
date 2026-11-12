# Lesson 5.2 — Data models and composition

**Course:** Python 0–100, Edition 2  
**Module:** [05 — Object-Oriented Programming](README.md)  
**Prerequisite:** Lesson 5.1  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Read basic type annotations.
2. Use dataclasses for value records.
3. Prefer composition when one object owns another.

### A record is a value, not a service

An observation describes a device at one sequence position. It should not also read a sensor, open a database, and decide how to display itself. Python's `dataclass` decorator can generate routine initialization, representation, and equality methods for a record.

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class Reading:
    device: str
    sequence: int
    cpu: float

reading = Reading("node01", 1, 72.0)
print(reading == Reading("node01", 1, 72.0))
```

The `@` syntax applies a decorator to the class. The annotations describe intended field types; they do not validate incoming data. `Reading("node01", "wrong", -5)` still needs explicit validation if it must be rejected. Validation can live in a separate factory function, or in `__post_init__`, which the generated initializer calls after assigning fields.

### Frozen does not mean deeply immutable

`frozen=True` rejects ordinary field assignment. If a field refers to a list, that list can still change. Use immutable field values for a small value record. Equality compares the declared fields; two equal records need not be the same object. Revisit `==` versus `is` from Module 03.

### Composition gives state an owner

A history object can **contain** readings. This is composition: history has readings; it is not a special kind of reading. Have `History.__init__` create its own empty list. Expose `add(reading)` and a `snapshot()` returning a tuple. Returning the internal list would let callers modify history without using its interface. A tuple prevents changes to the collection itself; nested mutable items would still need a policy.

Avoid building a hierarchy simply to share three lines. Inheritance becomes useful when instances genuinely support the same behavioral contract. Mother can combine a record, history, and evaluation function without inventing a superclass for every component.

## Self-review

Where should validation happen if JSON input has annotations attached nowhere? Why is history not a subclass of a reading?

## Practice

Complete the [exercise specification](exercises/lesson02/lesson02.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson02-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/library/dataclasses.html)
