# Lesson 9.1 — Iterators, generators, and bounded streams

**Course:** Python 0–100, Edition 2  
**Module:** [09 — Advanced Python and Machine Learning](README.md)  
**Prerequisite:** Module 08  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Distinguish iterables from iterators.
2. Trace lazy execution.
3. Bound resource use and preserve transaction semantics.

## 9.1.1 — A stream has an execution lifetime

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

## 9.1.2 — Separate a recipe from its progress

The list `[20, 81, 90]` stores three objects already available for repeated iteration. The generator remembers where one traversal has paused. After `next(stream)` finds 81, its loop has already examined 20 and 81. The following `list(stream)` resumes at 90; it does not restart from 20. A second `list(stream)` is empty because that same traversal has finished.

This matters when a caller wants both a count and results. Consuming the generator to count it uses up its values. Either accumulate the necessary results in one pass, create a fresh iterator from a reusable source, or materialize a deliberately bounded collection. Those choices have different memory and acquisition costs.

## 9.1.3 — State retained between yields

A generator's local variables survive suspension. For a delta calculation, retaining the previous reading is intentional. On the next iteration, subtract its CPU from the current CPU, then update the previous reading. A negative delta is a valid decrease; an out-of-order sequence is a different issue that validation should reject.

| Sequence | CPU | Delta emitted |
|---|---:|---:|
| 0 | 20 | None: no previous reading |
| 1 | 35 | 15 |
| 2 | 30 | -5 |

The first row has no delta because a difference needs two measurements. Inventing zero would confuse “unchanged” with “not yet defined.” The supplied sequence must belong to one device so adjacent differences have a meaningful interpretation.

## 9.1.4 — Lazy does not automatically mean bounded

Passing a generator to `list` materializes everything. Holding every result in a global history still grows without bound. A generator whose body opens a file must keep that resource alive during iteration and close it when finished; early abandonment complicates cleanup. Prefer an explicit outer file context when a caller owns the read lifecycle.

For simple line-oriented data, iterate a file handle rather than reading it all at once. For Mother, the capstone uses a deliberately bounded JSON list: at most 10,000 records. A streaming JSON parser is outside this course's core. Reject oversized batches before storage.

## 9.1.5 — Laziness changes failure timing

A validation generator can yield two good records and fail on the third. If the caller commits each yielded row separately, whole-batch atomicity is lost. Either materialize and validate a bounded batch first, or retain a transaction across the entire iteration and roll it back on failure.

Mother's later evaluation can consume ordered history with explicit limits. Start with correctness and measurable memory behavior before adding concurrency.

## 9.1.6 — Make failure timing part of the contract

If sequence 2 arrives after sequence 3, the generator can raise only when it reaches that input. A `try` block surrounding creation alone cannot catch this later error. The consumer must handle failures during consumption and decide what to do about earlier yielded values. That is why a storage contract cannot change from atomic batch import to partial commits merely because a generator looks convenient.

## Self-review

When is materializing a bounded batch the simpler choice? Why can catching errors only around generator creation miss validation failures?

## Practice

Complete the [exercise specification](exercises/lesson01/lesson01.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson01-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Python tutorial — Generators](https://docs.python.org/3/tutorial/classes.html#generators)
