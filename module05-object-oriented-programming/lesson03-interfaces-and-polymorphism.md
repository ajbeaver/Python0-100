# Lesson 5.3 — Interfaces and polymorphism

**Course:** Python 0–100, Edition 2  
**Module:** [05 — Object-Oriented Programming](README.md)  
**Prerequisite:** Lesson 5.2  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Describe a behavioral interface.
2. Substitute implementations through duck typing.
3. Distinguish composition from inheritance.

## 5.3.1 — Depend on behavior

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

## 5.3.2 — Trace the caller's expectations

For each element, the evaluator calls `policy.is_high(value)` and appends the returned result to a new list. The evaluator owns iteration; the policy owns the decision. This is **polymorphism**: the same call expression can invoke different implementations through different objects.

With `FixedLimit(80)`, the input 80 returns `False` and 90 returns `True`. A band policy with an inclusive normal interval [30, 70] instead treats 30 and 70 as normal, and 20 or 80 as high. The policies share the meaning “outside this policy's normal region,” while differing in how that region is configured. Document that meaning so a caller does not mistake a band policy for a simple upper threshold.

A method returning `"normal"` would be a subtle break. That nonempty string is truthy, so `if result:` would take the warning path. Matching a method name does not establish an interface; behavior includes the returned object's meaning.

## 5.3.3 — Substitution needs observable agreement

A fake policy returning `False` can help check orchestration. It cannot prove a real policy computes correctly. Test the policy directly, and include side-effect expectations such as no printing or input mutation in its contract.

## 5.3.4 — Choose between inheritance, composition, and callables

A subclass inherits accessible behavior from its base class. That can reduce duplication, but callers may then assume the subclass preserves the base class's contract. The test framework in Module 07 uses this relationship to supply supported assertion methods. A history should not inherit from a reading just to reuse a `device` attribute: history represents a collection and has different operations and invariants.

You can see composition in the evaluator: it receives a policy object and uses it without becoming a kind of policy. A future learned policy could own fitted parameters while the evaluator's iteration stays the same. You do not need to implement that future policy now.

When there is no retained configuration, a callable can express the same boundary:

```python
def classify_values(values, predicate):
    return [predicate(value) for value in values]
```

Here the comprehension means “evaluate the expression once for each value and collect the results.” `predicate` is a supplied function or other callable; it is invoked inside the loop.

If a policy has no state, accepting a function may be simpler than accepting an object. If it owns configuration or fitted parameters, an object can make that lifetime clear. You do not need an abstract base class or a third-party framework to start. Module 06 will apply the same idea to observation sources and storage boundaries.

## Self-review

When would a function argument be simpler? Which assumptions would a policy returning "normal" instead of `False` break?

## Practice

Complete the [exercise specification](exercises/lesson03/lesson03.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson03-answers.md` beside it. Then complete the [module assessment](assessment.md).

## Further reading

- [Python tutorial — Classes and inheritance](https://docs.python.org/3/tutorial/classes.html)
