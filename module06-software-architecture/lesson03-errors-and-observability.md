# Lesson 6.3 — Errors and observability

**Course:** Python 0–100, Edition 2  
**Module:** [06 — Software Architecture](README.md)  
**Prerequisite:** Lesson 6.2  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Distinguish invalid input from programming defects.
2. Handle exceptions at the appropriate boundary.
3. Log context without changing results.

## 6.3.1 — Failure has a shape

Invalid input is expected at a boundary. A misspelled attribute inside a classifier is a programming defect. Converting both into “normal” gives false confidence. Define a small domain exception where it adds meaning, and catch only the errors the caller can handle.

```python
class InvalidReading(ValueError):
    pass

def parse_cpu(raw):
    try:
        value = float(raw)
    except (TypeError, ValueError) as exc:
        raise InvalidReading("cpu must be numeric") from exc
    if not 0 <= value <= 100:
        raise InvalidReading("cpu must be between 0 and 100")
    return value
```

This example accepts numeric text and rejects non-finite floats through the range condition. Its text-conversion contract differs from the already-typed record contract in Module 05. Decide deliberately whether an input adapter accepts numeric text; never rely on annotations to make that choice. If raw JSON is required to contain numbers, check its value type before conversion.

## 6.3.2 — Follow exception propagation

When `float(raw)` raises, execution leaves that expression immediately. The matching handler translates it into `InvalidReading`. Statements after the failed conversion do not run. If a caller does not catch the new exception, it propagates outward until a handler is found or the program terminates with a traceback.

The `pass` statement in the exception class means its body adds no new behavior. Inheriting from `ValueError` makes `InvalidReading` catchable either specifically or through the broader value-error family. `as exc` binds the caught exception so it can be retained as the cause.

For `"bad"`, conversion fails and supplies a cause. For `"101"`, conversion succeeds but domain validation rejects the value afterward. These two paths should both be understandable to a user, while retaining different diagnostic evidence for a developer.

## 6.3.3 — Preserve the cause and handle errors at the right boundary

`raise ... from exc` preserves the underlying cause in a traceback. Keep that developer evidence while adding the source name and record number needed to locate malformed input. Do not use `except Exception: return []`; an empty successful batch and a failed batch have different meanings.

A parser can identify a record number but cannot decide whether the terminal should exit. The command entry point can decide the exit status but should not guess which record failed. Let the parser add input context and let the CLI translate the documented exception into a diagnostic and failure status.

An error message might say `observations.json, record 4: cpu must be between 0 and 100`. That is enough to locate the problem without printing every record. A message such as “failed” loses the evidence, while “processed 0 records” falsely resembles a successful empty import.

**Design principle:** Handle an exception where the caller can make a meaningful decision; otherwise preserve its cause and add only the context that layer knows.

## 6.3.4 — Logs explain execution

Use `logging.getLogger(__name__)` in reusable modules. Configure handlers and levels in the entry point, so importing a helper does not take over the application's output policy. Log a batch identifier and counts, rather than whole raw records. Standard logging normally uses stderr, leaving stdout available for machine-readable output. Logging should not decide classification or substitute for a returned result.

Mother will use exit code 0 for success, 2 for invalid user input, and 1 for operational failure. Unexpected defects should remain visible during development. A diagnostic must explain what failed without claiming the operation succeeded.

## Self-review

Which exceptions should a command-line boundary translate? What information makes a diagnostic useful without dumping a whole input file?

## Practice

Complete the [exercise specification](exercises/lesson03/lesson03.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson03-answers.md` beside it. Then complete the [module assessment](assessment.md).

## Further reading

- [Python tutorial — Errors and exceptions](https://docs.python.org/3/tutorial/errors.html)
