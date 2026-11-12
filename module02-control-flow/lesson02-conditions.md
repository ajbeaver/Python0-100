# Lesson 2.2 — Conditions, Truthiness, and Branching

**Course:** Python 0–100, Edition 2  
**Module:** 02 — Control Flow and Functions  
**Prerequisites:** Lesson 2.1  
**Estimated study time:** 45–60 minutes

## Learning objectives

After this lesson, you should be able to:

1. Explain when `if`, `elif`, and `else` bodies execute.
2. Put overlapping classification conditions in the correct order.
3. Distinguish independent alerts from mutually exclusive classification.
4. Use truthiness and direct Boolean assignment.
5. Prevent stale state between separate observations.

---

## 2.2.1 — Controlling execution with `if`

An `if` statement runs its body only when the condition is truthy:

```python
cpu = 85

if cpu > 80:
    print("High CPU utilization")
```

Only the indented `print()` is conditional. Note that exactly `80` does not satisfy `> 80`; comparison boundaries belong in requirements, not assumptions.

An `else` provides a fallback path:

```python
if cpu > 80:
    print("Elevated")
else:
    print("Normal")
```

Within one such chain, only one of the branches executes.

---

## 2.2.2 — Choosing one classification with `elif`

Suppose Mother's CPU classification is normal, elevated, or critical:

```python
if cpu >= 95:
    cpu_status = "critical"
elif cpu >= 80:
    cpu_status = "elevated"
else:
    cpu_status = "normal"
```

Python checks the conditions in order. When a condition is true, its body runs and the remainder of that `if`/`elif`/`else` chain is skipped.

**Priority matters.** If we check `cpu >= 80` before `cpu >= 95`, a CPU reading of `98` would be classified as elevated rather than critical. The second branch would never handle values of 95 or above.

An explicit inclusive range is also valid:

```python
elif 80 <= cpu <= 94:
    cpu_status = "elevated"
```

For our integer CPU readings, both forms work. The shorter branch works because the earlier critical branch already excluded larger values.

---

## 2.2.3 — Independent warnings need independent conditions

CPU and memory can both be high:

```python
if cpu > 80:
    print("CPU warning")

if memory > 80:
    print("Memory warning")
```

Both messages can print. But if the second statement were `elif`, it would be skipped whenever the CPU condition is already true.

Use an **exclusive chain** to choose exactly one status; use **independent `if` statements** for actions that can both be required.

### Separate calculation from action

A comparison already produces a Boolean:

```python
memory_warning = memory > 80

if memory_warning:
    print("Memory warning")
```

This is preferable to assigning `True` or `False` in two separate branches when the goal is just to calculate a warning flag.

It also avoids unnecessary `== True` comparisons. `if memory_warning:` directly checks the Boolean's truthiness.

We often keep the named Boolean when another calculation (such as overall status) needs to reuse it. If nothing else needs the flag, checking the comparison directly in `if` may be sufficient.

---

## 2.2.4 — Truthiness and missing data

Conditions do not require objects whose type is exactly `bool`.

| Expression value | Truthiness |
|---|---|
| `False`, `None`, `0`, `0.0` | Falsy |
| `""`, `[]`, `{}` | Falsy |
| Nonempty string, list, or dictionary | Truthy |
| Nonzero number | Truthy |

For example:

```python
history = []

if history:
    print("Measurements exist")
else:
    print("No measurements")
```

The empty list is falsy. The string `"False"` is truthy because it contains characters; it is not the Boolean value `False`.

Use `if not history:` when you want to detect an empty collection. Later, we'll discuss the important difference between absent data (`None`) and valid zero measurements.

---

## 2.2.5 — The stale-state trap

Here is a subtle bug from our original Lesson 5. Mother processes multiple observations:

```python
observations = [
    {"memory": 92},
    {"memory": 40}
]

memory_warning = False

for observation in observations:
    if observation["memory"] > 80:
        memory_warning = True

    print(memory_warning)
```

Output:

```text
True
True
```

The second reading is normal, but `memory_warning` was never recalculated as false. It retained the prior observation's state.

A clearer approach recalculates the flag on each iteration:

```python
for observation in observations:
    memory_warning = observation["memory"] > 80
    print(memory_warning)
```

Output:

```text
True
False
```

The lesson isn't that retaining state is always wrong. It is that **state must have the lifetime appropriate to what it represents**. A warning for *this observation* should not silently become a historical latch. Historical alerts, if needed, should be modeled intentionally.

This also shows why testing just the final classification can miss a bug. Another condition, such as critical CPU, may mask a stale memory flag. Good tests include an alert followed by a completely normal observation.

---

## 2.2.6 — Designing a priority rule

A system can have several warning inputs but should produce one final status. Define the priority explicitly:

```python
if cpu_status == "critical":
    system_status = "critical"
elif cpu_status == "elevated" or memory_warning or temperature_warning:
    system_status = "warning"
else:
    system_status = "normal"
```

The flags in this example must be calculated for the **current** observation before the status decision. By contrast, warning messages may be printed using independent `if` blocks.

### Design takeaway

Ask whether you're **calculating a value** (use an expression), **choosing one case** (use an `if`/`elif`/`else` chain), or **performing several independent actions** (use separate `if` statements).

---

## Read, practice, and review

Use `exercises/lesson02/lesson02.md` for your assignment. Implement the case ordering and verify the transition from high readings back to normal; no prewritten solution is supplied.

### Self-review

- [ ] I can explain why condition order changes results.
- [ ] I know when two `if` blocks should not be replaced with `if`/`elif`.
- [ ] I can assign comparisons directly to Boolean variables.
- [ ] I can distinguish a current observation from retained history.
- [ ] I test normal-after-warning behavior, not only isolated samples.

## Further reading

- [Python tutorial — `if` statements](https://docs.python.org/3/tutorial/controlflow.html#if-statements)
- [Python standard types — Truth value testing](https://docs.python.org/3/library/stdtypes.html#truth-value-testing)

**Next lesson:** 2.3 — Functions, Parameters, and Responsibility Separation.
