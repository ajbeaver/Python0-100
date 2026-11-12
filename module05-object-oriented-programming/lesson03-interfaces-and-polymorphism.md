# Lesson 5.3 — Interfaces and polymorphism

**Course:** Python 0–100, Edition 2  
**Module:** [05 — Object-Oriented Programming](README.md)  
**Prerequisite:** Lesson 5.2  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Describe a behavioral interface.
2. Substitute implementations through duck typing.
3. Distinguish composition from inheritance.

### Depend on behavior

An evaluator can use an object that supports `is_high(value)` without knowing how that object was constructed. This is **duck typing**: useful behavior matters more than a required base class.

```python
class FixedLimit:
    def __init__(self, threshold):
        self.threshold = threshold

    def is_high(self, value):
        return value > self.threshold

def evaluate(values, policy):
    return [policy.is_high(value) for value in values]

print(evaluate([70, 80, 90], FixedLimit(80)))
```

The interface is more than a method name. Specify inputs, return type, equality behavior, exceptions, and side effects. Here the intended contract accepts finite numeric values in range and returns a Boolean, with equality to the threshold classified as normal. A replacement must preserve that meaning or document a deliberate different policy.

### Substitution needs observable agreement

A fake policy returning `False` can help check orchestration. It cannot prove a real policy computes correctly. Test the policy directly as well. A replacement that prints, mutates input, or returns a string breaks a caller's reasonable assumptions even if the method name matches.

Inheritance shares behavior through a base class. It also creates a relationship that future changes must respect. Use it when substitutability is intentional. Start Mother with composed components and small explicit contracts. The test framework in Module 07 is a good example of purposeful inheritance: a test class inherits the runner's supported assertion methods.

### Choose the smallest useful abstraction

If a policy has no state, accepting a function may be simpler than accepting an object. If it owns configuration or fitted parameters, an object can make that lifetime clear. You do not need an abstract base class or a third-party framework to start. Module 06 will apply the same idea to observation sources and storage boundaries.

## Self-review

When would a function argument be simpler? Which assumptions would a policy returning "normal" instead of `False` break?

## Practice

Complete the [exercise specification](exercises/lesson03/lesson03.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson03-answers.md` beside it. Then complete the [module assessment](assessment.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/tutorial/classes.html)
