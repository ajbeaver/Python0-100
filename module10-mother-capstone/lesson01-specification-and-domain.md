# Lesson 10.1 — Mother specification and domain

**Course:** Python 0–100, Edition 2  
**Module:** [10 — Mother — Final Capstone](README.md)  
**Prerequisite:** Module 09  
**Estimated study time:** 60–90 minutes

## Learning objectives

1. Translate written requirements into interfaces.
2. Freeze identity and validation rules.
3. Define an executable acceptance plan.

### A deliberately small application

Mother imports explicitly supplied local observations, preserves a history, applies a fixed high-CPU rule, and fits a per-device mean predictor. It observes patterns in that supplied history. It does not collect live machine metrics in the required project. This boundary keeps runs deterministic and makes every prediction auditable.

Use standard-library Python 3.12+ and create the package from empty files. You may reuse your own earlier implementations after checking their contracts. Keep `domain.py`, `storage.py`, `baseline.py`, `evaluation.py`, `cli.py`, and `__main__.py` as understandable responsibilities; add helper modules only where useful. Include `__init__.py`, `tests/`, and `fixtures/`.

### The authoritative observation contract

An input is a UTF-8 JSON list of at most 10,000 objects, including an empty list. Each object has exactly `device`, `sequence`, and `cpu`. Device is a nonempty string; sequence is an integer >= 0, excluding booleans; CPU is a finite number in [0, 100], excluding booleans. Reject duplicate field names and duplicate identities inside a file. Empty import succeeds with zero inserted rows.

The database primary key is `(device, sequence)`. Across imports, an exact numeric replay is a no-op; different CPU at the same identity rejects the whole batch. Domain errors and conflicts exit 2. Operational failures exit 1. No failed import may change committed observation rows. Reports sort by device, sequence.

### Four commands, four responsibilities

| Command | Required behavior |
|---|---|
| `python -m mother ingest INPUT --db DB` | Validate then import atomically; report inserted and unchanged counts. |
| `python -m mother report --db DB --threshold 80` | Report ordered identity, CPU, and high status using strict `>`. |
| `python -m mother train --db DB --model MODEL --tolerance 15` | Split each device 60/20/20 and fit means on training only; save a new model file. |
| `python -m mother evaluate --db DB --model MODEL` | Report validation and test MAE for mean and fixed last-training-value baselines, plus residual flags. |

All paths are explicit. Use default threshold 80 and default tolerance 15 when the options are omitted; validate explicit overrides as finite numbers in [0, 100]. Keep the training tolerance in the saved artifact so evaluation uses the same policy. A missing required database is invalid user input (exit 2); a permissions or other operational database failure exits 1. Help exits 0 before opening resources. Report/train/evaluate require an existing initialized database and must not silently create an empty one. Refuse an existing model output path. Commands emit machine-readable JSON on stdout and diagnostics on stderr. Models never cause training on load.

### Make acceptance concrete

List tests before implementation: equality boundary, invalid last row, replay, conflict rollback, clean restart, unknown model version, and leakage resistance. Every requirement should have observable evidence, rather than a claim that the architecture is “clean.”

## Self-review

Which requirements belong in the domain and which in CLI/storage adapters? Why is a sequence key insufficient to claim wall-clock forecasting?

## Practice

Complete the [exercise specification](exercises/lesson01/lesson01.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson01-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/tutorial/modules.html)
