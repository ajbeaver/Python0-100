# Lesson 9.1 — Iterators, generators, and bounded streams

**Course:** Python 0–100, Edition 2  
**Module:** [09 — Advanced Python and Machine Learning](README.md)  
**Prerequisite:** Module 08  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Distinguish iterables from iterators.
2. Trace lazy execution.
3. Bound resource use and preserve transaction semantics.

### A stream has an execution lifetime

An iterable can produce an iterator. An iterator supplies one item at a time and is ordinarily exhausted after consumption. Lists are reusable iterables; a generator object is a single iterator.

```python
def above(values, threshold):
    for value in values:
        print("examining", value)
        if value > threshold:
            yield value

stream = above([20, 81, 90], 80)
print("created")
print(next(stream))
print(list(stream))
print(list(stream))
```

Predict the complete print order. `yield` suspends the generator and later resumes its local state. Calling the generator function creates the generator object without running the body. Exceptions can therefore occur during iteration, long after construction.

### Lazy does not automatically mean bounded

Passing a generator to `list` materializes everything. Holding every result in a global history still grows without bound. A generator whose body opens a file must keep that resource alive during iteration and close it when finished; early abandonment complicates cleanup. Prefer an explicit outer file context when a caller owns the read lifecycle.

For simple line-oriented data, iterate a file handle rather than reading it all at once. For Mother, the capstone uses a deliberately bounded JSON list: at most 10,000 records. A streaming JSON parser is outside this course's core. Reject oversized batches before storage.

### Laziness changes failure timing

A validation generator can yield two good records and fail on the third. If the caller commits each yielded row separately, whole-batch atomicity is lost. Either materialize and validate a bounded batch first, or retain a transaction across the entire iteration and roll it back on failure. Faster-looking code must preserve the written contract.

Mother's later evaluation can consume ordered history with explicit limits. Start with correctness and measurable memory behavior before adding concurrency.

## Self-review

When is materializing a bounded batch the simpler choice? Why can catching errors only around generator creation miss validation failures?

## Practice

Complete the [exercise specification](exercises/lesson01/lesson01.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson01-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/tutorial/classes.html#generators)
