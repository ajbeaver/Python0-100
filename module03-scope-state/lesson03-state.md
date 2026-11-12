# Lesson 3.3 — State Lifetime, Side Effects, and Isolation

**Course:** Python 0–100, Edition 2  
**Module:** 03 — Scope, References, and State  
**Prerequisites:** Lessons 3.1–3.2  
**Estimated study time:** 55–70 minutes

## Learning objectives

1. Distinguish temporary per-observation values from historical state.
2. Identify stale-state bugs in repeated calculations.
3. Separate pure calculations from intentional side effects.
4. Design small interfaces with explicit inputs and outputs.
5. Describe how tests can expose errors hidden by the order of input data.

---

## 3.3.1 — What do we mean by state?

**State** is information retained by a program that can affect later behavior. A running total is state, but so is a Boolean warning, a list of observations, or a configuration dictionary.

Not all state should live for the same duration. Consider three possible lifetimes:

- **Per-operation state:** a temporary accumulator used while calculating one average.
- **Per-observation state:** warning flags derived from one measurement snapshot.
- **Historical state:** records retained across many observations to support trends and future learning.

Problems arise when state survives longer than the data it represents, or when historical data changes accidentally.

## 3.3.2 — The stale-warning bug

Consider a stream of independent measurements:

```python
observations = [
    {"memory": 91},
    {"memory": 45}
]

memory_warning = False

for observation in observations:
    if observation["memory"] > 80:
        memory_warning = True
    print(memory_warning)
```

This prints `True` twice. The warning is activated on the first reading but never reset for the second.

Here, the intended meaning of `memory_warning` is "does the **current** observation exceed the threshold?" It should not preserve the previous observation's status.

An appropriate per-observation calculation is:

```python
for observation in observations:
    memory_warning = observation["memory"] > 80
    print(memory_warning)
```

This re-evaluates the Boolean for every observation, including when the answer becomes `False`.

Notice the distinction between *calculating a Boolean* and *acting on it*:

```python
if memory_warning:
    print("Memory warning")
```

The expression determines the state; the conditional decides whether to perform an action.

## 3.3.3 — Different states, different owners

Mother might maintain:

- A history list that is intentionally appended to as observations arrive.
- A current-observation status calculated independently each time.
- A configuration record defining thresholds.

These are different responsibilities and should not all be treated as one mutable global collection.

For example, a classification function can receive the observation and thresholds and return a status:

```python
def classify_cpu(cpu, elevated_threshold, critical_threshold):
    if cpu >= critical_threshold:
        return "critical"
    if cpu >= elevated_threshold:
        return "elevated"
    return "normal"
```

The function doesn't read hidden globals, mutate the observation, or print the result. The caller controls the persistence and presentation of its return value.

## 3.3.4 — Side effects are not always bad

A **side effect** is a change observable outside a function's return value, such as printing, writing a file, sending a network message, or mutating caller-owned data.

A calculation function may be **pure** when its return value is determined solely by its inputs and it does not cause observable side effects:

```python
def above_threshold(value, threshold):
    return value > threshold
```

Pure functions are easy to test because they don't depend on execution order or hidden state.

Mother will still need side effects. Reading sensors, storing records, and reporting warnings all interact with the outside world. We should place those actions in clearly defined parts of the program rather than pretend they can be eliminated.

## 3.3.5 — A small pipeline of responsibilities

A useful high-level architecture for Mother's early monitoring logic is:

```text
observation supplied
        |
        v
classify observation  --> returned status
        |
        v
record and/or display result
```

The classifier should not need to know whether a result is printed, written to disk, or fed to a learning model. The calling code can coordinate those decisions.

This is **separation of concerns**, not a demand for a class or a new module for every line of code. At this point, a few clearly named functions in one file are sufficient.

## 3.3.6 — Testing sequences, not just snapshots

A status function might pass a test with elevated memory followed by critical CPU, yet still contain stale state. The later critical CPU status could hide the incorrect memory warning.

To expose this, test sequences that change direction:

1. Normal → warning.
2. Warning → normal.
3. Critical → normal.
4. Repeated identical input.

Also inspect intermediate flags, not just the final status. A system can produce a correct overall classification for the wrong underlying reasons.

The simplest reliable approach is to recalculate current status using only current data unless the requirements explicitly call for memory or history.

## 3.3.7 — Preparing for persistent learning

Later, Mother's learning component will need **intentional history**. For example, detecting that CPU load typically peaks at a certain hour requires storing multiple observations over time.

The history is supposed to persist. Current warnings are not. Keeping these concepts separate allows you to build a machine with memory without accidentally making every decision depend on old flags.

**Design principle:** For every piece of state, ask who owns it, how long it should live, and which operations may change it.

---

## Self-review checkpoint

1. Explain why an accumulator belongs outside an iteration but a per-observation warning belongs inside it.
2. What distinguishes intentional historical state from stale per-observation state?
3. Why is it helpful to test warning → normal transitions?
4. When is a side-effecting function a reasonable design choice?

## Further reading

- [Python Tutorial — Defining Functions](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)
- [Python Tutorial — Data Structures](https://docs.python.org/3/tutorial/datastructures.html)

**Exercise:** `exercises/lesson03/lesson03.md`  
**Next:** Module 03 assessment.
