# Lesson 1.1 — Python Execution, Objects, and Variables

**Course:** Python 0–100, Edition 2
**Module:** 01 — Python Foundations
**Prerequisites:** Python 3 and a terminal
**Estimated study time:** 35–50 minutes

## Learning Objectives

After completing this lesson, you should be able to:

1. Describe how Python executes a source file.
2. Distinguish names, objects, types, and values.
3. Explain assignment versus mutation.
4. Predict how rebinding affects objects referenced by multiple names.

---

## 1.1.1 — What Happens When Python Runs?

A Python program begins as a text file containing source code.

For example, `lesson01.py` might contain:

```python
device = "node01"
print(device)
```

When you execute:

```bash
python3 lesson01.py
```

Your shell starts the Python interpreter and provides the filename as an argument.

The standard Python implementation is called **CPython**.

CPython parses your source code and compiles it into an intermediate representation called *bytecode*. Its virtual machine then executes those instructions.

The general process is:

1. Read and parse the source code.
2. Compile it into bytecode.
3. Execute the bytecode.
4. Produce results or side effects.

Compilation is usually automatic. Unlike a traditional C development workflow, you don't generally need to run a separate compiler before executing a Python script.

Python may also cache compiled bytecode in directories named `__pycache__`.

We'll examine those directories when we study modules and imports.

### The Shell Versus the Python Interpreter

These are separate programs with different responsibilities.

Your shell interprets commands such as:

```bash
python3 --version
```

The Python interpreter processes Python code.

This distinction becomes important when working with multiple Python installations, virtual environments, and executable scripts.

For now, two commands are useful:

```bash
python3 --version
command -v python3
```

The first reports the interpreter's version.

The second identifies the executable selected by your shell.

---

## 1.1.2 — Variables Are Names Bound to Objects

Consider:

```python
device = "node01"
```

Python binds the name `device` to a string object.

A useful mental model is that **variables are names referring to objects, not containers holding values**.

Objects have three important properties:

- **Identity:** Which particular object it is.
- **Type:** What kind of object it is.
- **Value:** The data it represents.

Python provides built-in functions for investigating objects.

For example:

```python
device = "node01"

print(type(device))
```

This reports the object's type:

```text
<class 'str'>
```

The type `str` represents Python strings.

### Multiple Names Can Reference One Object

Consider:

```python
device = "node01"
alias = device
```

Both names now refer to the same string object.

Suppose we continue:

```python
device = "node02"

print(device)
print(alias)
```

The names now refer to different strings.

The assignment to `device` did not modify the original string.

Instead, Python changed which object the name `device` references.

The name `alias` remains bound to the original string.

This behavior is called **rebinding**.

---

## 1.1.3 — Assignment Versus Mutation

These are related but fundamentally different operations.

### Assignment

Assignment changes which object a name references.

```python
temperature = 68
temperature = 72
```

The second assignment binds `temperature` to another integer object.

Integers are immutable, meaning an existing integer object's value cannot be changed in place.

Strings are also immutable.

### Mutation

Mutation changes the contents or state of an existing object.

For example:

```python
readings = [60, 65]
readings.append(70)
```

Here, `append()` changes the existing list.

Lists are mutable objects.

This distinction becomes particularly important when multiple names reference the same list.

Consider:

```python
first = [60, 65]
second = first

first.append(70)
```

Both `first` and `second` still reference the same list.

Because that list was modified, both names provide access to the updated contents.

However, this operation is different:

```python
first = [60, 65, 70]
```

That assignment creates a new list and binds `first` to it.

It does not automatically change the list referenced by `second`.

### Why Does This Matter?

As programs become more complex, multiple functions may work with the same objects.

If one function modifies a shared list, other parts of the program can observe that modification.

This may be intentional.

It may also introduce unexpected behavior.

Understanding mutation and rebinding is foundational to managing program state correctly.

---

## 1.1.4 — A Software Engineering Connection

Our eventual capstone, Mother, will observe information about her environment.

Imagine Mother records CPU measurements over time.

Each measurement represents information collected at a particular moment.

Suppose we store historical observations as mutable objects.

If multiple historical records accidentally reference the same underlying object, modifying the current observation could unintentionally change previously recorded information.

That's a serious data-integrity problem.

We aren't ready to implement Mother's storage system yet, but this illustrates why object references and mutation matter beyond simple syntax.

**Design principle:** Understand whether an operation modifies an existing object or creates a new one before sharing data between components.

---

# Lesson 1.1 — Exercises

Complete these exercises before continuing.

## Exercise 1 — Reassignment

Without executing the code, predict its output:

```python
a = 10
b = a

a = 25

print(a)
print(b)
```

Explain why the two values differ.

## Exercise 2 — Mutation

Without executing the code, predict its output:

```python
first = [1, 2]
second = first

first.append(3)

print(second)
```

Explain why the result differs from Exercise 1.

## Exercise 3 — Your First Program

Create a file named `lesson01.py`.

### Requirements

1. Store a device name as a string.
2. Store CPU utilization as an integer.
3. Store whether the device is active as a Boolean.
4. Print all three values and their types using `type()`.

Do not introduce functions, classes, or external packages yet.

## Written Reflection

Answer in your own words:

**What is the difference between assigning a new object to a name and modifying an existing mutable object?**

Provide one example of each.

Then explain one reason this distinction could matter in a monitoring application.

---

## Self-Review Checklist

- [ ] I understand the difference between the shell and the Python interpreter.
- [ ] I understand that variables are names bound to objects.
- [ ] I can distinguish assignment from mutation.
- [ ] I predicted the exercises before executing them.
- [ ] My program satisfies all four implementation requirements.

---

## Further Reading

- [Python Tutorial — An Informal Introduction](https://docs.python.org/3/tutorial/introduction.html)
- [Python Language Reference — Data Model](https://docs.python.org/3/reference/datamodel.html)
- [LearnPython.org](https://www.learnpython.org/)

---

## Completion

**Status:** Pending

**Next Lesson:** 1.2 — Types, Expressions, and Operators

Complete the exercises and submit your solutions for review before advancing.
