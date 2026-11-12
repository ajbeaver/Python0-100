# Lesson 6.2 — Dependency injection and configuration

**Course:** Python 0–100, Edition 2  
**Module:** [06 — Software Architecture](README.md)  
**Prerequisite:** Lesson 6.1  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Pass dependencies explicitly.
2. Replace a source without editing the core.
3. Validate configuration once at a boundary.

## 6.2.1 — Wiring is a separate responsibility

**Dependency injection** means a caller supplies something the function needs, rather than letting the function construct or locate it secretly. It extends the explicit-argument principle from Module 03 to behavior as well as data.

```python
def assess(source, policy):
    readings = source()
    return [(reading.device, policy.is_high(reading.cpu))
            for reading in readings]
```

`source` is a callable returning validated readings. `policy` implements the contract from Lesson 5.3. A terminal entry point can pass a fixed-data source; a test can pass a different function. Neither needs to patch a global variable.

## 6.2.2 — Pass behavior without executing it early

There is a difference between `assess(source, policy)` and `assess(source(), policy)`. In the first form, the service receives a callable and decides when to call it. In the second, Python calls the source before entering the service and passes its result instead. Both designs can work, but they are different contracts.

A small standalone example makes the timing visible:

```python
def fixed_source():
    print("source called")
    return [20, 90]

def collect_once(source):
    values = source()
    return len(values), values

print(collect_once(fixed_source))
```

The source message appears once, followed by `(2, [20, 90])`. The count and values refer to one acquisition. An environment source might return different values on a later call, so “call once” is a data-consistency decision as well as an efficiency choice.

## 6.2.3 — Configuration is input too

For a fixed-threshold rule, validate a finite number between 0 and 100, excluding booleans. Choose one documented precedence rule: explicit command option overrides the default. Do not silently combine a hard-coded threshold, an environment variable, and an old saved value. A report should state the effective configuration so someone can reproduce the decision.

A frozen configuration record makes accidental updates harder. It does not replace validation. Construct the valid configuration before opening any output resource. A malformed option should fail without creating a database.

## 6.2.4 — Distinguish no override from a valid zero

Configuration commonly arrives as an optional value. If `override` is `None`, use the default. If it is 0, use zero: it is a valid threshold under the course contract. The expression `override or 80` would incorrectly replace zero with 80 because zero is falsy.

Resolve precedence with an explicit absence check, then validate the chosen value before opening output resources.

## 6.2.5 — Keep effects observable

A counting fake source can check the one-acquisition contract demonstrated above. The entry point can also construct one policy per run and supply it to several operations, making both acquisition and configuration lifetimes explicit.

Injection does not require a container library. Prefer an explicit constructor or function argument while the system is small. Hidden dependency lookup makes order-sensitive behavior harder to explain. In Mother, the entry point owns wiring, the source owns acquisition, and the core owns classification.

## Self-review

Why should a classifier not discover its own database path? Where should configuration precedence be visible?

## Practice

Complete the [exercise specification](exercises/lesson02/lesson02.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson02-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Python reference — Callable objects](https://docs.python.org/3/library/functions.html#callable)
