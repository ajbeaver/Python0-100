# Module 01 — Final Assessment

**Course:** Python 0–100, Edition 2  
**Module:** 01 — Python Foundations  
**Scope:** Lessons 1.1, 1.2, and 1.3  
**Total:** 100 points

## Rules

- Predict before running programs in Part A.
- Write your own `.py` implementations; this file provides requirements and starting data only.
- Use only concepts covered in Module 01: no loops, functions, classes, or external packages.
- Keep your answers in `assessment-answers.md` and your implementations in separate `.py` files.
- Test with changed values when it makes sense. Read each requirement before writing code.

---

## Part A — Knowledge assessment (30 points)

Choose one answer per question (3 points each). Record your choices in `assessment-answers.md`.

**1.** What prints?

```python
a = 10
b = a
a = 20
print(b)
```

A. `10`  
B. `20`  
C. `None`  
D. `TypeError`

**2.** What is the resulting type of `value`?

```python
value = 5
value = "five"
```

A. `int`  
B. `str`  
C. `float`  
D. `TypeError`

**3.** What does `10 > 5 and 3 > 8` evaluate to?

A. `True`  
B. `False`  
C. `None`  
D. `TypeError`

**4.** What does `10 // 3` evaluate to?

A. `3.333...`  
B. `1`  
C. `3`  
D. `4`

**5.** What prints?

```python
device = "node01"
print(device[1:4])
```

A. `nod`  
B. `ode`  
C. `ode0`  
D. `node`

**6.** Which operation changes the *existing* list object?

```python
a = [1, 2, 3]
```

A. `a = a + [4]`  
B. `a.append(4)`  
C. `a = [1, 2, 3, 4]`  
D. All three

**7.** What prints?

```python
a = [1, 2]
b = a
a.append(3)
print(b)
```

A. `[1, 2]`  
B. `[1, 2, 3]`  
C. `[3]`  
D. `None`

**8.** What happens?

```python
environment = {"cpu": 75}
print(environment["memory"])
```

A. Prints `None`  
B. Prints `0`  
C. Raises `KeyError`  
D. Raises `TypeError`

**9.** Which operator checks whether two names reference the exact same object?

A. `==`  
B. `=`  
C. `is`  
D. `!=`

**10.** What most naturally represents 1,000 CPU readings in chronological order?

A. 1,000 separate variables  
B. One list  
C. A dictionary with arbitrary keys  
D. One string

---

## Part B — Programming (50 points)

Write three independent scripts. Begin each script empty; any data below is provided in this specification, not as a prewritten solution.

### Assignment 1 — System Status (10 points)

**File:** `assessment01_1.py`

Starting measurements:

```python
cpu = 87
memory = 65
temperature = 74
```

Requirements:

1. Create Boolean warnings for CPU greater than 80, memory greater than 80, and temperature greater than 75.
2. Create one `system_warning` that is `True` if any individual warning is active.
3. Print every individual warning and the overall warning.
4. Use expressions, not `if` statements.

### Assignment 2 — Observation History (15 points)

**File:** `assessment01_2.py`

Starting data:

```python
environment = {
    "cpu": 70,
    "memory": 55,
    "temperature": 64
}
```

Requirements:

1. Create `cpu_history` with `45`, `52`, and `63`.
2. Add the dictionary's current CPU value to the list.
3. Update the dictionary's CPU value to `91` and append it again.
4. Print the latest CPU measurement using negative indexing, and print the full history.
5. Do not duplicate CPU measurements by manually typing the current values into the history.

### Assignment 3 — First Data Model (25 points)

**File:** `assessment01_3.py`

Design a structure containing three complete environmental observations, each with named CPU, memory, and temperature values. Pick your own numerical values.

Requirements:

1. Preserve the observations in chronological order.
2. Support accessing each measurement by a meaningful key.
3. Retrieve and print **the CPU value in the second observation**.
4. Append a fourth complete observation without replacing the first three.
5. Print the resulting structure.

**Constraint:** No loops, functions, classes, or third-party packages.

---

## Part C — Design reasoning (20 points)

Answer in `assessment-answers.md` (10 points each).

**1.** Why would a list of dictionaries be appropriate for Mother's chronological environmental observations, instead of one dictionary containing all measurements? Explain what the list and each dictionary contribute.

**2.** Two names initially reference the same list. Explain the difference between modifying the list with `append()` and rebinding one name with `a = a + [value]`. What changes are visible through the second name in each case?

---

## Grading

| Component | Points |
|---|---:|
| Part A — 10 multiple-choice questions | 30 |
| Part B — Assignment 1 | 10 |
| Part B — Assignment 2 | 15 |
| Part B — Assignment 3 | 25 |
| Part C — Two design questions | 20 |
| **Total** | **100** |

**90–100:** Mastered  
**75–89:** Passed; review weaknesses  
**Below 75:** Reinforcement recommended

The assessment is complete only after the solutions and written answers have been reviewed. No answer key is included in the student materials.

**Status:** Not submitted for Edition 2
