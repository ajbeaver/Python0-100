# Lesson 6.1 — Contracts and boundaries

**Course:** Python 0–100, Edition 2  
**Module:** [06 — Software Architecture](README.md)  
**Prerequisite:** Module 05  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Separate domain decisions from I/O.
2. Specify a pipeline contract.
3. Draw an acyclic dependency graph.

## 6.1.1 — Follow one observation

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

## 6.1.2 — A boundary separates reasons to change

Suppose a new output format requires a dictionary instead of a sentence. If classification returns domain data, only formatting needs to change. If classification prints its decision immediately, callers must intercept output or duplicate the classifier to obtain structured data. That coupling is a concrete maintenance cost.

An **adapter** translates between an outside representation and the core's language. A future JSON adapter will turn text into validated readings. A terminal adapter will turn returned results into printed output. Neither should redefine what “high” means.

For a batch [79, 80, 81] and threshold 80, trace the responsibility sequence:

| Stage | Result or effect |
|---|---|
| Validation | Three accepted reading records |
| Classification | `False`, `False`, `True` |
| Formatting | Three readable strings |
| Entry point | Prints those strings in order |

These are stages of one use case, not necessarily four classes. The diagram is useful because it identifies where a defect belongs. Wrong text with correct Booleans suggests formatting; wrong Booleans suggests policy or input; missing output may be orchestration.

## 6.1.3 — Dependencies point toward stable meaning

A useful graph is `__main__ -> service -> domain`, with input/output adapters also using domain records. Domain code must not import the command entry point or know the database filename. The entry point assembles concrete components. A service coordinates a use case; it should not become a global bag of unrelated utilities.

Keep the diagram proportionate to the program: splitting every function into a separate file creates navigation work without necessarily improving independence.

## 6.1.4 — Read dependencies as knowledge

An arrow from `service` to `domain` means service code knows domain names and contracts. It does not mean domain code calls the service back. Keeping dependencies acyclic makes initialization easier and allows the domain to be imported without starting the application.

Whole-batch rejection adds another important boundary. Validation may create temporary records for earlier rows, but it should not publish a successful partial batch when a later row is invalid. In-memory construction is preparation; storing or printing accepted results is an observable effect. The future persistence layer will preserve this distinction with a transaction.

**Design principle:** Choose boundaries around independently changing responsibilities, then specify what becomes observable on success and failure.

## 6.1.5 — Define failure and ordering

State whether a batch preserves input order, rejects the entire batch on any malformed record, or reports partial success. We will use whole-batch rejection for capstone imports. An invalid last row must not leave earlier rows silently stored. This decision will determine the database transaction boundary in Module 08.

## Self-review

Which module should change for a new output format? Why must batch failure behavior be decided before storage is added?

## Practice

Complete the [exercise specification](exercises/lesson01/lesson01.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson01-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Python tutorial — Modules](https://docs.python.org/3/tutorial/modules.html)
