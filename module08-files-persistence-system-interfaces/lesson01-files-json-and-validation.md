# Lesson 8.1 — Files, JSON, and validation

**Course:** Python 0–100, Edition 2  
**Module:** [08 — Files, Persistence, and System Interfaces](README.md)  
**Prerequisite:** Module 07  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Use context managers and explicit encodings.
2. Distinguish JSON decoding from validation.
3. Reject malformed batches before side effects.

## 8.1.1 — Syntax is the first boundary

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

## 8.1.2 — Trace the two translations

JSON text is not Python source code. JSON uses `true`, `false`, and `null`; decoding produces Python `True`, `False`, and `None`. JSON arrays become lists and objects become dictionaries. A successful decode establishes syntax, not a valid reading.

For example, `[{"device": "node01", "sequence": 0, "cpu": "80"}]` is syntactically valid JSON but violates this course's numeric CPU contract. Explicitly distinguish syntax errors, shape errors, and domain errors when reporting failures. The user can then fix the relevant part instead of guessing what “invalid file” means.

## 8.1.3 — Validate the entire shape

Check the top-level list, then each mapping's fields, types, and ranges. Python's decoder can accept nonstandard `NaN` tokens by default; domain validation must still reject non-finite values. For this course, reject unknown fields and duplicate `(device, sequence)` identities within one batch. Duplicate JSON object keys are a separate ambiguity from duplicate observation identities; they need a decoder-level check, described next.

Do not convert `null`, `"80"`, or `true` into valid readings. A format-specific adapter may support numeric text in another application, but the capstone JSON schema requires numbers.

## 8.1.4 — Validate information before discarding it

The JSON text `{"cpu": 20, "cpu": 90}` contains a repeated key. By the time an ordinary dictionary reaches domain validation, only the last value is normally visible. `object_pairs_hook` lets the decoder supply the original ordered pairs to a function that you write. That function checks for repeated names while constructing a mapping and raises on repetition. This is different from finding two separate observations with the same `(device, sequence)`.

Validation order should follow the structure: top-level list and record count, mapping shape and fields, field types, numeric ranges, then batch identity uniqueness. Checking an assumption after code already depends on it can produce misleading errors; for example, indexing a string as if it were a record should not be the first shape check.

## 8.1.5 — Limits have a scope

The 10,000-record rule bounds accepted batches. `json.load` still reads and decodes the document before that count can be checked. It is not a byte-size or memory-sandbox guarantee for arbitrary hostile files. A service accepting untrusted uploads would need a separate file-size and resource policy. For the supplied local labs, keep the stated record limit and avoid claiming stronger protection than it provides.

**Design principle:** Validate at the boundary where the necessary information still exists, and state exactly what each check establishes.

## 8.1.6 — Prepare, then commit

Parse and validate all records before calling storage. Preserve their order in memory; database reports will explicitly sort later. Handle `OSError`, `json.JSONDecodeError`, and domain validation failures with useful context. Missing input must not become an empty successful import.

For exports, do not overwrite an existing user file silently. Write to a new path or define a temporary-file-and-replace policy. Atomic replacement prevents partial visibility on a suitable local filesystem; it alone is not a promise of survival after power loss.

## Self-review

Why are duplicate keys and duplicate identities different? Why should path errors preserve a failing exit status?

## Practice

Complete the [exercise specification](exercises/lesson01/lesson01.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson01-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Python reference — JSON](https://docs.python.org/3/library/json.html)
