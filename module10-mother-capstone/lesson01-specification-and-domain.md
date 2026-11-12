# Lesson 10.1 — Mother specification and domain

**Course:** Python 0–100, Edition 2  
**Module:** [10 — Mother — Final Capstone](README.md)  
**Prerequisite:** Module 09  
**Estimated study time:** 60–90 minutes

## Learning objectives

1. Translate written requirements into interfaces.
2. Freeze identity and validation rules.
3. Define an executable acceptance plan.

## 10.1.1 — A deliberately small application

Mother imports explicitly supplied local observations, preserves a history, applies a fixed high-CPU rule, and fits a per-device mean predictor. It observes patterns in that supplied history. It does not collect live machine metrics in the required project. This boundary keeps runs deterministic and makes every prediction auditable.

Use standard-library Python 3.12+ and create the package from empty files. You may reuse your own earlier implementations after checking their contracts. Keep `domain.py`, `storage.py`, `baseline.py`, `evaluation.py`, `cli.py`, and `__main__.py` as understandable responsibilities; add helper modules only where useful. Include `__init__.py`, `tests/`, and `fixtures/`.

## 10.1.2 — The authoritative observation contract

An input is a UTF-8 JSON list of at most 10,000 objects, including an empty list. Each object has exactly `device`, `sequence`, and `cpu`. Device is a nonempty string; sequence is an integer >= 0, excluding booleans; CPU is a finite number in [0, 100], excluding booleans. Reject duplicate field names and duplicate identities inside a file. Empty import succeeds with zero inserted rows.

The database primary key is `(device, sequence)`. Across imports, an exact numeric replay is a no-op; different CPU at the same identity rejects the whole batch. Domain errors and conflicts exit 2. Operational failures exit 1. No failed import may change committed observation rows. Reports sort by device, sequence.

## 10.1.3 — Four commands, four responsibilities

| Command | Required behavior |
|---|---|
| `python -m mother ingest INPUT --db DB` | Validate then import atomically; report inserted and unchanged counts. |
| `python -m mother report --db DB --threshold 80` | Report ordered identity, CPU, and high status using strict `>`. |
| `python -m mother train --db DB --model MODEL --tolerance 15` | Split each device 60/20/20 and fit means on training only; save a new model file. |
| `python -m mother evaluate --db DB --model MODEL` | Report validation and test MAE for mean and fixed last-training-value baselines, plus residual flags. |

All paths are explicit. Use default threshold 80 and default tolerance 15 when the options are omitted; validate explicit overrides as finite numbers in [0, 100]. Keep the training tolerance in the saved artifact so evaluation uses the same policy. A missing required database is invalid user input (exit 2); a permissions or other operational database failure exits 1. Help exits 0 before opening resources. Report/train/evaluate require an existing initialized database and must not silently create an empty one. Refuse an existing model output path. Commands emit machine-readable JSON on stdout and diagnostics on stderr. Models never cause training on load.

## 10.1.4 — Turn a command into a use case

For ingestion, the entry point first interprets paths and options. The input adapter decodes and validates the complete file. The service asks storage to apply the batch and receives inserted/unchanged counts. Only then does the CLI emit success JSON. This ordering connects the earlier lessons: parsing establishes meaning, transactions preserve the batch contract, and output communicates the committed result.

Reporting follows a different path: read ordered observations, classify under the effective threshold, then serialize the report. It must not modify stored observations or persist one threshold's labels as permanent truth. Training reads the same history but creates a separate artifact; evaluation consumes that artifact and compares predictions with held-out actual values.

## 10.1.5 — Preserve the distinction between observation and inference

`(device, sequence)` identifies supplied data, not a conclusion. High status depends on a selected threshold. A predicted mean depends on training data. A residual flag depends on both the model and its tolerance. Combining all of these into one ambiguous `status` field would make later inspection difficult.

The capstone integrates existing techniques rather than requiring another framework. Keep domain rules in a place usable by the CLI, tests, and model code. When two adapters need the same rule, share that rule instead of letting their copies gradually disagree.

## 10.1.6 — Make acceptance concrete

List tests before implementation: equality boundary, invalid last row, replay, conflict rollback, clean restart, unknown model version, and leakage resistance. Every requirement should have observable evidence, rather than a claim that the architecture is “clean.”

An acceptance matrix connects requirements to evidence:

| Requirement | Evidence to retain |
|---|---|
| Equality to 80 is normal | Report/test containing CPU 79, 80, 81 |
| A late invalid row rejects a batch | Failure status and unchanged stored rows |
| Replay adds nothing | Second ingest reports unchanged count |
| Evaluation never refits implicitly | Unchanged model parameters before/after evaluation |

These are examples for planning, not a completed test suite. You choose test organization and returned JSON shape, but it must expose enough information to establish the stated behavior. Keep those output fields stable once documented.

## Self-review

Which requirements belong in the domain and which in CLI/storage adapters? Why is a sequence key insufficient to claim wall-clock forecasting?

## Practice

Complete the [exercise specification](exercises/lesson01/lesson01.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson01-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Python tutorial — Modules](https://docs.python.org/3/tutorial/modules.html)
