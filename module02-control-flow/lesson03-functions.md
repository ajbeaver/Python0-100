# Lesson 2.3 — Functions, Parameters, and Responsibility Separation

**Course:** Python 0–100, Edition 2  
**Module:** 02 — Control Flow and Functions  
**Prerequisites:** Lessons 2.1–2.2  
**Estimated study time:** 50–65 minutes

## Learning objectives

After this lesson, you should be able to:

1. Define a function and explain when its body executes.
2. Distinguish parameters from arguments.
3. Return reusable results and distinguish them from printed output.
4. Identify local bindings and the limits of function scope.
5. Give simple computations distinct, meaningful responsibilities.

---

## 2.3.1 — Definition versus execution

A **function** groups a reusable operation under a name:

```python
def greet():
    print("Hello, Mother")
```

Defining the function does not execute its body. A call does:

```python
greet()
```

Python executes the function body and then returns control to its caller. Functions are one mechanism for abstraction: a caller can use an operation without needing to repeat its implementation.

---

## 2.3.2 — Parameters and arguments

A function can accept inputs:

```python
def check_cpu(cpu):
    return cpu > 80

warning = check_cpu(91)
```

`cpu` is a **parameter**, a name declared in the function definition. `91` is the **argument**, the object supplied at call time. Inside the function, the local parameter name refers to that argument's object.

Parameterization makes code reusable. A threshold might be an explicit parameter, too:

```python
def above_threshold(reading, threshold):
    return reading > threshold
```

Hardcoding a threshold in the function would force callers to accept that one policy. Making it an argument allows the same operation to serve different contexts.

---

## 2.3.3 — Return values are not console output

Compare:

```python
def double(number):
    return number * 2
```

with:

```python
def show_double(number):
    print(number * 2)
```

The first returns a numeric object to its caller. The second writes a representation to standard output and, because it has no explicit `return`, returns `None`.

```python
result = double(5)
```

Now `result` refers to `10`, which another calculation can reuse.

```python
result = show_double(5)
```

This prints `10`, but `result` refers to `None`. `print()` is not a substitute for returning a useful result.

For Mother, the same calculated average may eventually be displayed, logged, stored, or used by a learning algorithm. Returning the number keeps those choices in the caller rather than coupling the calculation to one output mechanism.

### Output is a side effect

A **side effect** is an observable effect beyond returning a value, such as printing, writing to a file, or changing shared data. Side effects are often necessary, but they should be intentional.

---

## 2.3.4 — Local scope

A function invocation has its own local names:

```python
temperature = 70

def update_temperature():
    temperature = 85

update_temperature()
print(temperature)
```

Output:

```text
70
```

Assigning the name `temperature` inside this function creates a local binding; it does not rebind the module-level name. In contrast, names in a `for` loop belong to the surrounding scope, since the loop does not establish a function scope.

Functions can read names in enclosing or global scopes under certain rules, but **explicit arguments are usually clearer than hidden dependencies** when building reusable computation functions.

Important limitation: scope does not prevent a function from modifying a mutable object passed as an argument. That is the focus of Module 03.

---

## 2.3.5 — A reusable average

Here is a worked example connecting this lesson to accumulators:

```python
def calculate_average(history):
    total = 0

    for reading in history:
        total += reading

    return total / len(history)
```

The function:

1. Receives observations rather than reading a particular global list.
2. Initializes and changes its own local accumulator.
3. Returns the calculated result rather than printing it.

This example assumes a nonempty list. A later design decision must establish what happens for empty or otherwise invalid input; silently returning `0` would incorrectly conflate missing observations with a genuine average of zero.

---

## 2.3.6 — A function should express a responsibility

Consider two separate tasks:

- Calculate how many measurements exceed a threshold.
- Display a message describing that count.

The first is computation; the second is presentation. They may belong to separate operations because a consumer of the result might not want terminal output.

Questions to ask before extracting a function:

1. Does this operation have a clear name and coherent purpose?
2. Does it receive the inputs it needs rather than relying on unrelated global state?
3. Does it produce a useful output or intentionally perform a side effect?
4. Would isolating it make the program easier to understand or test?

Do **not** maximize function count. A short, straightforward expression does not automatically need a wrapper. Functions introduce useful boundaries when those boundaries express genuine responsibilities.

### Pure functions — an introduction

A function is *pure* when the same input values produce the same result and evaluation has no observable side effects. Simple numerical calculation functions can be pure, provided their inputs are not being changed concurrently and they do not mutate shared objects.

Pure functions tend to be easy to test, but not every function in a real program can be pure. An observer that collects system measurements necessarily interacts with its environment.

---

## 2.3.7 — Main code and a note on entry points

For now, you may call functions directly at module level:

```python
average = calculate_average([60, 70, 80])
print(average)
```

You may already have seen `main()` and the `if __name__ == "__main__":` pattern. It is valuable in larger Python programs because it distinguishes running a file directly from importing it as a module. We'll formally study modules and entry points later rather than introducing boilerplate into every beginner exercise.

### Design takeaway

**Let calculation functions calculate and return results; let their callers decide what to do with those results.** This is a starting point for separation of concerns, not an absolute prohibition on printing or other side effects.

---

## Read, practice, and review

Complete `exercises/lesson03/lesson03.md`. Your job is to implement the two functions; the exercise specification does not supply their implementations.

### Self-review

- [ ] I know that defining a function does not call it.
- [ ] I can distinguish a parameter from an argument.
- [ ] I can predict what an implicit `None` return means.
- [ ] I understand local accumulator state inside a function.
- [ ] I can explain why return values improve reuse.

## Further reading

- [Python tutorial — Defining functions](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)
- [Python tutorial — More on defining functions](https://docs.python.org/3/tutorial/controlflow.html#more-on-defining-functions)

**Next:** Module 02 final assessment (`assessment.md`), then Module 03 — Scope, References, and State.
