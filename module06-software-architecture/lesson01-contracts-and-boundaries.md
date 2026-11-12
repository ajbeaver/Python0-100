# Lesson 6.1 — Contracts and boundaries

**Course:** Python 0–100, Edition 2  
**Module:** [06 — Software Architecture](README.md)  
**Prerequisite:** Module 05  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Separate domain decisions from I/O.
2. Specify a pipeline contract.
3. Draw an acyclic dependency graph.

### Follow one observation

Mother's first end-to-end path is: receive supplied data, validate it, evaluate it, then format a result. The **domain** is the meaning of an observation and its classification. A database path, terminal color, or JSON syntax belongs to an outer boundary.

Write contracts before choosing files. For example, `classify(reading, threshold)` accepts a validated reading and a valid threshold, returns a result, and neither prints nor stores anything. The caller decides whether to save that result. This makes two independent questions visible: was the decision correct, and was the decision delivered correctly?

```python
def classify(cpu, threshold):
    return cpu > threshold

def format_result(device, high):
    return f"{device}: {'high' if high else 'normal'}"

def render(readings, threshold):
    lines = []
    for reading in readings:
        high = classify(reading.cpu, threshold)
        lines.append(format_result(reading.device, high))
    return lines
```

This illustrative core assumes inputs have already been validated. It is not a complete parser. Predict whether running `render` twice changes the supplied readings.

### Dependencies point toward stable meaning

A useful graph is `__main__ -> service -> domain`, with input/output adapters also using domain records. Domain code must not import the command entry point or know the database filename. The entry point assembles concrete components. A service coordinates a use case; it should not become a global bag of unrelated utilities.

A boundary is useful when something varies independently: data origin, persistence, or output. Splitting every function into a new module creates navigation work without necessarily improving independence.

### Define failure and ordering

State whether a batch preserves input order, rejects the entire batch on any malformed record, or reports partial success. We will use whole-batch rejection for capstone imports. An invalid last row must not leave earlier rows silently stored. This decision will determine the database transaction boundary in Module 08.

## Self-review

Which module should change for a new output format? Why must batch failure behavior be decided before storage is added?

## Practice

Complete the [exercise specification](exercises/lesson01/lesson01.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson01-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/tutorial/modules.html)
