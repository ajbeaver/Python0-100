# Module 03 — Final Assessment

**Course:** Python 0–100, Edition 2  
**Module:** 03 — Scope, References, and State  
**Total:** 100 points  
**Status:** Not submitted

## Assessment rules

- Predict outputs before running snippets.
- Use only concepts from Modules 01–03.
- No external packages or classes.
- Write your implementation files yourself, beginning with empty `.py` files.
- Keep source code, written answers, and the assessment specification separate.

## Part A — Multiple choice (30 points)

Choose one answer for each of the ten questions (3 points each). Record selections in `assessment-answers.md`.

### 1. Local assignment

```python
value = 10

def change():
    value = 20

change()
print(value)
```

A. `10`  
B. `20`  
C. `None`  
D. `UnboundLocalError`

### 2. Augmented assignment

```python
count = 5

def bump():
    count += 1
    return count

bump()
```

A. Returns `6`  
B. Returns `5`  
C. Raises `UnboundLocalError`  
D. Raises `TypeError`

### 3. Function argument

```python
def add(values):
    values.append(9)

items = [1]
add(items)
print(items)
```

A. `[1]`  
B. `[1, 9]`  
C. `[9]`  
D. `None`

### 4. Rebinding

```python
def replace(values):
    values = [7, 8]

items = [1, 2]
replace(items)
print(items)
```

A. `[7, 8]`  
B. `[1, 2]`  
C. `[]`  
D. `NameError`

### 5. Equal versus identical

```python
a = [1, 2]
b = [1, 2]
```

Which is correct?

A. `a is b` is true  
B. `a == b` is false  
C. Both comparisons are true  
D. `a == b` is true and `a is b` is false

### 6. Shallow copy

```python
records = [{"cpu": 60}]
other = records.copy()
other[0]["cpu"] = 90
print(records[0]["cpu"])
```

A. `60`  
B. `90`  
C. `None`  
D. `KeyError`

### 7. State lifetime

What should happen to a Boolean representing *the current observation's* memory warning when a new observation arrives?

A. It must remain true once triggered  
B. It should be recalculated from the new observation  
C. It should always be global  
D. It should always be stored as a string

### 8. Pure calculation

Which function is the best example of a pure calculation?

A. A function that prints an alert  
B. A function that appends a record to a global history  
C. A function that returns whether an input exceeds an explicit threshold  
D. A function that writes readings to a file

### 9. Shadowing built-ins

What is a likely consequence of binding the name `len` to an integer at module level?

A. Python permanently removes the built-in function  
B. Ordinary `len(...)` calls in that module can fail because the name refers to the integer  
C. It changes the behavior of all Python programs on the computer  
D. The assignment is prohibited

### 10. Mutable default

```python
def record(value, history=[]):
    history.append(value)
    return history
```

When `record()` is called repeatedly without the second argument, the default list:

A. Is newly created for each call  
B. Is shared across such calls  
C. Is automatically immutable  
D. Cannot be appended to

---

## Part B — Programming (50 points)

Create three initially empty Python files. Submit the programs and any required written reasoning. Do not copy example solutions from the textbook.

### Assignment 1 — Scope and explicit inputs (10 points)

**Filename:** `assessment03_1.py`

Starting data:

```python
readings = [55, 80, 93, 76]
threshold = 80
```

Requirements:

1. Define a function that accepts a reading and a threshold.
2. Return a Boolean indicating whether the reading is strictly above the threshold.
3. Call the function for every reading and print the reading and Boolean outside the function.
4. Do not reference the module-level `threshold` from inside the function.

### Assignment 2 — Mutating versus non-mutating (15 points)

**Filename:** `assessment03_2.py`

Starting data:

```python
history = [20, 30]
reading = 40
```

Requirements:

1. Define one function that intentionally appends to a caller-supplied list.
2. Define another function that returns an updated list while keeping the input list unchanged.
3. Demonstrate both operations on separate fresh input lists.
4. Print the input and resulting data clearly enough to distinguish identity-preserving mutation from a new-list result.
5. In your answer file, explain which operation changes caller-owned state.

### Assignment 3 — Mother's state isolation (25 points)

**Filename:** `assessment03_3.py`

Starting data:

```python
observations = [
    {"cpu": 40, "memory": 50},
    {"cpu": 82, "memory": 91},
    {"cpu": 45, "memory": 42},
    {"cpu": 97, "memory": 30},
    {"cpu": 55, "memory": 60}
]
```

Requirements:

1. Independently classify each observation: `critical` for CPU at least `95`; otherwise `warning` for CPU at least `80` or memory above `85`; otherwise `normal`.
2. Put classification in a function with an explicit observation parameter and returned status.
3. Maintain a list of historical status strings, preserving chronological order.
4. Maintain category counts for the full batch without using global counters.
5. Print per-observation results and a summary from code outside the classification function.
6. Append a final observation `{"cpu": 25, "memory": 25}` and verify it is `normal`.

Expected counts after the final observation:

```text
normal: 4
warning: 1
critical: 1
```

Test whether any earlier observation dictionary was mutated as a side effect. Explain your evidence in your answer file.

---

## Part C — Design reasoning (20 points)

Answer in `assessment-answers.md`. Each response is worth 10 points.

### Question 1 — Ownership and lifetime

Mother has three pieces of information: current warning status, a growing history of readings, and configurable thresholds. Explain which should be recalculated, which should persist, and how you would avoid hidden dependencies between functions.

### Question 2 — Function side effects

Compare a function that modifies a passed-in history with a function that returns a new history. Describe the tradeoffs and when either design would be appropriate. Address why local parameter names do not automatically prevent changes to caller-owned objects.

---

## Grading

| Component | Points |
|---|---:|
| Part A — Multiple choice | 30 |
| Part B — Assignment 1 | 10 |
| Part B — Assignment 2 | 15 |
| Part B — Assignment 3 | 25 |
| Part C — Design reasoning | 20 |
| **Total** | **100** |

90–100: mastered; 75–89: passed with review; below 75: reinforcement recommended. Correct output alone does not guarantee a correct implementation: the review also checks unintended mutation, stale state, and function boundaries.

## Submission checklist

- [ ] Multiple-choice predictions completed before running snippets.
- [ ] All three implementations written and tested independently.
- [ ] Boundary tests performed, including warning → normal.
- [ ] Design reasoning written in my own words.
- [ ] No external dependencies or prewritten solutions used.

**Next:** Submit the assessment for review before starting Module 04.
