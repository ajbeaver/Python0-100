# Lesson 5.2 — Data models and composition

**Course:** Python 0–100, Edition 2  
**Module:** [05 — Object-Oriented Programming](README.md)  
**Prerequisite:** Lesson 5.1  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Read basic type annotations.
2. Use dataclasses for value records.
3. Prefer composition when one object owns another.

## 5.2.1 — A record is a value, not a service

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

The `@` syntax applies a decorator to the class. Here it requests the dataclass-generated behavior. The example prints `True`: separately constructed records compare equal when their declared field values agree.

## 5.2.2 — Read an annotation as a contract

In `cpu: float`, the colon associates the field name with an intended type. It is not the assignment operator. The generated initializer receives the actual object supplied by the caller. A tool can use the annotation to flag questionable code, but a JSON reader can still supply a string or a Boolean at runtime.

This separates three questions: what shape the code expects, whether an incoming value satisfies that shape, and how the value is stored. Dataclasses help with the third. Your validation establishes the second. Annotations communicate the first.

The sample record intentionally omits validation so the generated behavior is visible. The exercise adds it. A `__post_init__` method can inspect `self.device`, `self.sequence`, and `self.cpu` after initialization and raise before construction returns to the caller. With a frozen record, prefer validating the assigned fields rather than trying to mutate them into different values afterward.

A finite CPU is an ordinary numeric value rather than positive/negative infinity or NaN (“not a number”). `math.isfinite(value)` checks this once you have established that the input is numeric. For the exercise, accept integers or floats but explicitly exclude booleans: Python permits `True` in many integer operations, while the measurement contract does not treat it as CPU 1. Check field type before range so the rejection describes the actual problem.

## 5.2.3 — Frozen does not mean deeply immutable

`frozen=True` rejects ordinary field assignment. If a field refers to a list, that list can still change. Use immutable field values for a small value record. Equality compares the declared fields; two equal records need not be the same object. Revisit `==` versus `is` from Module 03.

## 5.2.4 — Follow the references in a snapshot

A tuple is an immutable sequence: it preserves order and supports indexing, but has no list-style `append` operation. Consider this separate reference experiment:

```python
record = {"cpu": 20}
snapshot = (record,)
record["cpu"] = 90
print(snapshot[0]["cpu"])
```

The result is `90`. The tuple's membership did not change; the object at its first position did. This is the same shallow-versus-nested ownership issue encountered in Module 03. A tuple of frozen records with only immutable fields provides a stronger practical snapshot than a tuple of mutable dictionaries.

For Mother, retaining an observation means preserving what was observed, not preserving a live view of a dictionary somebody else continues to edit. A history owner can return a snapshot collection while retaining control over additions. Avoid exposing an internal list merely because it is convenient to print.

**Design principle:** Select record and container types together; immutability of an outer container says nothing by itself about its elements.

## 5.2.5 — Composition gives state an owner

A history object can **contain** readings. This is composition: history has readings; it is not a special kind of reading. Have `History.__init__` create its own empty list. Expose `add(reading)` and a `snapshot()` returning a tuple. Returning the internal list would let callers modify history without using its interface. Use the snapshot reasoning above to decide whether the contained records are stable too.

Avoid building a hierarchy simply to share three lines. Inheritance becomes useful when instances genuinely support the same behavioral contract. Mother can combine a record, history, and evaluation function without inventing a superclass for every component.

## Self-review

Where should validation happen if JSON input has annotations attached nowhere? Why is history not a subclass of a reading?

## Practice

Complete the [exercise specification](exercises/lesson02/lesson02.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson02-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Python reference — Dataclasses](https://docs.python.org/3/library/dataclasses.html)
