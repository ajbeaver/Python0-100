# Module 02 — Final Assessment

**Course:** Python 0–100, Edition 2  
**Module:** 02 — Control Flow and Functions  
**Coverage:** Lessons 2.1–2.3  
**Total:** 100 points  
**Status:** Not submitted

## Instructions

This is an independent assessment, not another guided lesson. Read all specifications before coding. Complete the written quiz without executing its examples first; then use Python to test your practical work. Consult official Python documentation if necessary, but write your own solutions.

- Part A — Multiple choice: 10 questions, 3 points each (30).
- Part B — Programming: 3 assignments (10 + 15 + 25 = 50).
- Part C — Design reasoning: 2 questions, 10 points each (20).

No solutions or answer key are included.

---

## Part A — Multiple Choice (30 points)

Record your answers in `assessment-answers.md`. Choose **one** answer for each question.

### 1. What does this print?

```python
values = [2, 4, 6]
total = 0
for value in values:
    total += value
print(total)
```

A. `6`  
B. `12`  
C. `24`  
D. `0`

### 2. What does this print?

```python
values = [3, 6, 9]
for value in values:
    total = 0
    total += value
print(total)
```

A. `18`  
B. `0`  
C. `9`  
D. An exception

### 3. How many times does this loop execute?

```python
count = 0
while count < 4:
    count += 1
```

A. 3  
B. 4  
C. 5  
D. Infinitely

### 4. What does this print?

```python
cpu = 97
if cpu >= 80:
    print("Elevated")
elif cpu >= 95:
    print("Critical")
else:
    print("Normal")
```

A. Critical  
B. Elevated  
C. Both messages  
D. Normal

### 5. What does this print?

```python
cpu = 90
memory = 95
if cpu > 80:
    print("CPU")
if memory > 80:
    print("Memory")
```

A. Only CPU  
B. Only Memory  
C. CPU and Memory  
D. Nothing

### 6. What does this print?

```python
def double(number):
    return number * 2
result = double(5)
print(result)
```

A. `5`  
B. `10`  
C. `None`  
D. Exception

### 7. What is stored in `result`?

```python
def display(number):
    print(number * 2)
result = display(5)
```

A. `10`  
B. `5`  
C. `None`  
D. `"10"`

### 8. What does this print?

```python
x = 10
def change():
    x = 20
change()
print(x)
```

A. `10`  
B. `20`  
C. `None`  
D. Exception

### 9. What does this print?

```python
def check(value, threshold):
    return value > threshold
print(check(80, 80))
```

A. `True`  
B. `False`  
C. `80`  
D. `None`

### 10. Which design most directly supports reuse of a calculated result?

A. A function that only prints its answer  
B. A function that returns its answer  
C. A global variable modified by many functions  
D. Duplicating the calculation at each call site

---

## Part B — Programming (50 points)

Create your own three initially empty Python files in a directory of your choosing, alongside this assessment. Do not use external packages, classes, or material beyond this module unless a requirement explicitly permits it.

### Assignment 1 — CPU Statistics (10 points)

**Starting data:**

```python
cpu_history = [35, 48, 62, 79, 88, 94, 71, 55]
```

Requirements:

1. Use a `for` loop and an accumulator to calculate the total.
2. Count readings **greater than or equal to 75**.
3. Calculate the average.
4. Print total, count, and average.
5. Do not call `sum()` or an external statistics package.

Expected values:

```text
Total CPU: 532
High Readings: 3
Average CPU: 66.5
```

**Filename:** `assessment02-1.py`

### Assignment 2 — Reusable Classification (15 points)

Implement a function that accepts one CPU measurement and **returns** one status string:

| CPU value | Status |
|---|---|
| Below 70 | `normal` |
| 70 through less than 90 | `elevated` |
| 90 or above | `critical` |

Use it to classify every value here, printing the value and the returned classification outside the function:

```python
measurements = [45, 72, 91, 69, 90, 88]
```

Expected status sequence:

```text
normal, elevated, critical, normal, critical, elevated
```

**Filename:** `assessment02-2.py`

### Assignment 3 — Mother's Environmental Analyzer (25 points)

**Starting data:**

```python
observations = [
    {"cpu": 45, "memory": 55, "temperature": 62},
    {"cpu": 82, "memory": 70, "temperature": 68},
    {"cpu": 65, "memory": 91, "temperature": 74},
    {"cpu": 96, "memory": 88, "temperature": 81},
    {"cpu": 50, "memory": 60, "temperature": 65}
]
```

Requirements:

1. Process every observation in chronological order.
2. Classify each observation as `critical` if CPU is **at least 95** or temperature is **greater than 80**.
3. Otherwise classify it as `warning` if CPU is **at least 80** or memory is **greater than 85**.
4. Otherwise classify it as `normal`.
5. Print each observation's status, followed by counts for normal, warning, and critical.
6. Create at least one function responsible for classification that **returns** its status.
7. Keep output printing outside classification and avoid global count accumulators.

Expected status sequence: `normal`, `warning`, `warning`, `critical`, `normal`.

Expected counts:

```text
Normal: 2
Warning: 2
Critical: 1
```

**Regression test:** Append a sixth observation:

```python
{"cpu": 30, "memory": 40, "temperature": 50}
```

It must classify as `normal`; updated counts should be 3 normal, 2 warning, and 1 critical. Earlier warnings must not leak into it.

**Filename:** `assessment02-3.py`

---

## Part C — Design Reasoning (20 points)

Answer in `assessment-answers.md`.

### Question 1 — Responsibility Separation (10 points)

A function reads CPU observations, calculates an average, determines alert status, prints warnings, and writes a file. Would you leave all of this in one function? Explain which responsibilities you would separate (if any) and why. No code required.

### Question 2 — Function Boundaries (10 points)

Compare:

```python
def calculate_average(values):
    total = 0
    for value in values:
        total += value
    print(total / len(values))
```

with:

```python
def calculate_average(values):
    total = 0
    for value in values:
        total += value
    return total / len(values)
```

Explain:

1. The behavioral difference between these functions.
2. Which is preferable for Mother's monitoring system and why.
3. A situation where the printing version might be appropriate.

---

## Submission and grading

Submit `assessment-answers.md`, `assessment02-1.py`, `assessment02-2.py`, and `assessment02-3.py` for review.

| Component | Points |
|---|---:|
| Multiple choice | 30 |
| Assignment 1 | 10 |
| Assignment 2 | 15 |
| Assignment 3 | 25 |
| Design reasoning | 20 |
| **Total** | **100** |

- 90–100: Mastered.
- 75–89: Passed; review weaknesses.
- Below 75: Reinforcement recommended.

## Completion checklist

- [ ] All ten quiz answers recorded before executing snippets.
- [ ] All three programs independently implemented and run.
- [ ] Boundary conditions and regression test checked.
- [ ] Design questions answered in my own words.
- [ ] Files submitted for review.

**Next module:** 03 — Scope, References, and State.
