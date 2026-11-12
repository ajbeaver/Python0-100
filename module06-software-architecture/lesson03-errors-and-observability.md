# Lesson 6.3 — Errors and observability

**Course:** Python 0–100, Edition 2  
**Module:** [06 — Software Architecture](README.md)  
**Prerequisite:** Lesson 6.2  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Distinguish invalid input from programming defects.
2. Handle exceptions at the appropriate boundary.
3. Log context without changing results.

### Failure has a shape

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

### Preserve cause, add useful context

`raise ... from exc` keeps the underlying cause in a traceback. A file parser can add the source name and record number. The CLI can print one concise diagnostic to stderr and exit nonzero. A developer can inspect a traceback while debugging. Do not use `except Exception: return []`; an empty successful batch and a failed batch have different meanings.

### Logs explain execution

Use `logging.getLogger(__name__)` in reusable modules. Configure handlers and levels in the entry point. Log a batch identifier and counts, rather than whole raw records. Standard logging normally uses stderr, leaving stdout available for machine-readable output. Logging should not decide classification or substitute for a returned result.

Mother will use exit code 0 for success, 2 for invalid user input, and 1 for operational failure. Unexpected defects should remain visible during development. A diagnostic must explain what failed without claiming the operation succeeded.

## Self-review

Which exceptions should a command-line boundary translate? What information makes a diagnostic useful without dumping a whole input file?

## Practice

Complete the [exercise specification](exercises/lesson03/lesson03.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson03-answers.md` beside it. Then complete the [module assessment](assessment.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/tutorial/errors.html)
