# Lesson 8.1 — Files, JSON, and validation

**Course:** Python 0–100, Edition 2  
**Module:** [08 — Files, Persistence, and System Interfaces](README.md)  
**Prerequisite:** Module 07  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Use context managers and explicit encodings.
2. Distinguish JSON decoding from validation.
3. Reject malformed batches before side effects.

### Syntax is the first boundary

A JSON decoder translates text into Python objects. It does not establish that those objects form valid observations. For Mother, a file will contain a list of at most 10,000 objects with exactly `device`, `sequence`, and `cpu`. A device is a nonempty string; sequence is a nonnegative integer excluding booleans; CPU is a finite JSON number from 0 through 100 excluding booleans.

```python
import json
from pathlib import Path

path = Path("observations.json")
with path.open(encoding="utf-8") as handle:
    raw = json.load(handle)
print(type(raw).__name__)
```

Create the input file explicitly before running this scratch example. The `with` context closes the handle on normal completion and exceptions. A relative path is resolved from the current working directory, not from the source file's directory.

### Validate the entire shape

Check the top-level list, then each mapping's fields, types, and ranges. Python's decoder can accept nonstandard `NaN` tokens by default; domain validation must still reject non-finite values. For this course, reject unknown fields and duplicate `(device, sequence)` identities within one batch. Duplicate JSON object keys are a separate ambiguity: use `object_pairs_hook` to reject repeated field names rather than silently accepting the last one.

Do not convert `null`, `"80"`, or `true` into valid readings. A format-specific adapter may support numeric text in another application, but the capstone JSON schema requires numbers.

### Prepare, then commit

Parse and validate all records before calling storage. Preserve their order in memory; database reports will explicitly sort later. Handle `OSError`, `json.JSONDecodeError`, and domain validation failures with useful context. Missing input must not become an empty successful import.

For exports, do not overwrite an existing user file silently. Write to a new path or define a temporary-file-and-replace policy. Atomic replacement prevents partial visibility on a suitable local filesystem; it alone is not a promise of survival after power loss.

## Self-review

Why are duplicate keys and duplicate identities different? Why should path errors preserve a failing exit status?

## Practice

Complete the [exercise specification](exercises/lesson01/lesson01.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson01-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/library/json.html)
