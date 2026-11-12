# Lesson 2.1 — Loops, Iteration, and Accumulators

**Course:** Python 0–100, Edition 2  
**Module:** 02 — Control Flow and Functions  
**Prerequisites:** Module 01  
**Estimated study time:** 45–60 minutes

## Learning objectives

After this lesson, you should be able to:

1. Explain what a `for` loop does with an iterable.
2. Distinguish `for` from `while` and identify termination conditions.
3. Build totals and counts with accumulators.
4. Reason about indentation and state across iterations.
5. Recognize empty-collection and boundary cases.

---

## 2.1.1 — The reason to iterate

Mother has measurements collected at successive moments:

```python
cpu_history = [60, 65, 70, 75, 85]
```

Writing one statement per index ties the program to an exact collection length. Instead, describe an operation applied to *every* item:

```python
for cpu in cpu_history:
    print(cpu)
```

Output:

```text
60
65
70
75
85
```

`cpu_history` is an **iterable**. Python asks it for an **iterator**, then repeatedly obtains the next element until the iterator is exhausted. The loop variable `cpu` is rebound to each element in turn; it does not contain the entire list.

A `for` loop is not limited to lists; other iterable objects include strings, dictionaries, ranges, and files. We will study their specific behavior as we need them.

### Indentation determines the loop body

```python
for cpu in cpu_history:
    print(cpu)

print("Finished")
```

The first print executes on every iteration. The unindented print executes once after iteration finishes.

---

## 2.1.2 — Accumulating a result

An **accumulator** retains a result across iterations:

```python
total = 0

for cpu in cpu_history:
    total += cpu

print(total)
```

`total += cpu` updates the binding of `total` to the new sum. For integers it has the same numerical effect as `total = total + cpu`.

| Step | Current CPU | Running total |
|---|---:|---:|
| Initial | — | 0 |
| 1 | 60 | 60 |
| 2 | 65 | 125 |
| 3 | 70 | 195 |
| 4 | 75 | 270 |
| 5 | 85 | 355 |

Initialization belongs **before** the loop:

```python
for cpu in cpu_history:
    total = 0
    total += cpu
```

This mistakenly resets the total during every iteration; after the final iteration it holds only the final element's value.

### Counters are accumulators too

A total accumulates values; a counter accumulates occurrences:

```python
high_count = 0

for cpu in cpu_history:
    if cpu > 80:
        high_count += 1
```

With the example list, `high_count` is `1`. Notice that **strictly greater than** excludes exactly `80`.

---

## 2.1.3 — One pass, multiple calculations

A loop can perform independent work for the same observation:

```python
total = 0
high_count = 0

for cpu in cpu_history:
    total += cpu
    if cpu > 80:
        high_count += 1
```

This is not an invitation to put every possible responsibility into one loop. It is appropriate here because both operations examine the same collection and are simple. Later, functions will let us give those calculations clear names and separate interfaces.

To compute an average, first compute the total, then divide by the number of observations:

```python
average = total / len(cpu_history)
```

`len()` is a built-in function. We intentionally avoid `sum()` in the exercise so you practice the mechanism rather than only calling an abstraction.

### An important failure case

If `cpu_history` is empty, `len(cpu_history)` is zero. The division raises `ZeroDivisionError`. That does **not** mean a CPU reading of `0` is invalid. A measured zero is valid data; an empty history means no data exists.

We can distinguish the two:

```python
if cpu_history:
    average = total / len(cpu_history)
else:
    average = None
```

`None` can represent an unavailable result, although the desired behavior should be specified by the caller or requirements. Our basic exercise assumes the list is nonempty.

---

## 2.1.4 — `while` and termination

A `while` loop repeats while a condition is truthy:

```python
count = 0

while count < 5:
    print(count)
    count += 1
```

The output is `0` through `4`. The condition is checked before every iteration. When `count` becomes `5`, the loop ends.

If the body never changes any relevant condition, the loop may run indefinitely:

```python
count = 0

while count < 5:
    print(count)
```

That is an infinite loop unless interrupted externally.

| `for` | `while` |
|---|---|
| Iterates over an iterable | Repeats while a condition holds |
| Naturally suits a known collection | Suits condition-driven repetition |
| Ends on iterator exhaustion | Must eventually become false or exit another way |

We'll eventually need carefully controlled, stoppable observation loops for Mother. We are **not** implementing a perpetual background loop yet.

---

## 2.1.5 — State and scope are not identical

A loop variable and accumulator persist across iterations in the surrounding scope. A regular `for` loop does **not** create a new function scope. But an accumulator need not be global: if the loop runs inside a function, its accumulator can be local to that function.

For integer accumulators, `+=` calculates and rebinds an integer; it does not mutate an integer object. We'll return to mutable objects in Module 03.

### Design takeaway

Initialize accumulators once per logical calculation; update them during iteration; use the completed results afterward. Check boundary conditions before assuming a collection contains data.

---

## Read, practice, and review

Use `exercises/lesson01/lesson01.md` for the independent exercise. Create `lesson01.py` yourself; do not paste the textbook's worked examples as a solution.

### Self-review

- [ ] I can explain how a loop variable receives successive values.
- [ ] I can trace the value of an accumulator by hand.
- [ ] I know why initialization belongs outside the loop body.
- [ ] I can distinguish a valid zero from an empty collection.
- [ ] I know when `for` and `while` fit a problem.

## Further reading

- [Python tutorial — `for` statements](https://docs.python.org/3/tutorial/controlflow.html#for-statements)
- [Python tutorial — `range()`](https://docs.python.org/3/tutorial/controlflow.html#the-range-function)
- [LearnPython.org — Basic Operators and Loops](https://www.learnpython.org/)

**Next lesson:** 2.2 — Conditions, Truthiness, and Branching.
