# Lesson 6.2 — Dependency injection and configuration

**Course:** Python 0–100, Edition 2  
**Module:** [06 — Software Architecture](README.md)  
**Prerequisite:** Lesson 6.1  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Pass dependencies explicitly.
2. Replace a source without editing the core.
3. Validate configuration once at a boundary.

### Wiring is a separate responsibility

**Dependency injection** means a caller supplies something the function needs, rather than letting the function construct or locate it secretly. The term sounds larger than the technique.

```python
def assess(source, policy):
    readings = source()
    return [(reading.device, policy.is_high(reading.cpu))
            for reading in readings]
```

`source` is a callable returning validated readings. `policy` implements the contract from Lesson 5.3. A terminal entry point can pass a fixed-data source; a test can pass a different function. Neither needs to patch a global variable.

### Configuration is input too

For a fixed-threshold rule, validate a finite number between 0 and 100, excluding booleans. Choose one documented precedence rule: explicit command option overrides the default. Do not silently combine a hard-coded threshold, an environment variable, and an old saved value. A report should state the effective configuration so someone can reproduce the decision.

A frozen configuration record makes accidental updates harder. It does not replace validation. Construct the valid configuration before opening any output resource. A malformed option should fail without creating a database.

### Keep effects observable

A fake source can count how often it is called and return fixed readings. It should be called once per assessment run. A second call to obtain a count could observe a different sample or exhaust an iterator. Capture the batch once, then derive its count and results from that same batch.

Injection does not require a container library. Prefer an explicit constructor or function argument while the system is small. Hidden dependency lookup makes order-sensitive behavior harder to explain. In Mother, the entry point owns wiring, the source owns acquisition, and the core owns classification.

## Self-review

Why should a classifier not discover its own database path? Where should configuration precedence be visible?

## Practice

Complete the [exercise specification](exercises/lesson02/lesson02.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson02-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/library/functions.html#callable)
