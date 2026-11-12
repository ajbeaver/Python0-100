# Lesson 10.2 — Persistence and learning pipeline

**Course:** Python 0–100, Edition 2  
**Module:** [10 — Mother — Final Capstone](README.md)  
**Prerequisite:** Lesson 10.1  
**Estimated study time:** 60–90 minutes

## Learning objectives

1. Integrate transactions and model fitting.
2. Bind a model to a reproducible dataset.
3. Reject drift in evaluation inputs.

### Store observations, derive decisions

Persist validated CPU observations, not only “high” labels. A later threshold can then produce a new report without rewriting history. Integrate Module 08's single-writer transaction policy, explicit close, placeholders, and ordered readback. Test reopen behavior before adding the model pipeline.

### A model needs provenance

Extend Module 09's artifact with a dataset fingerprint and split metadata. Compute SHA-256 over UTF-8 JSON encoding of the complete ordered rows as `[device, sequence, float(cpu)]` arrays, using `ensure_ascii=True`, `allow_nan=False`, and `separators=(",", ":")`. Normalize any zero CPU to `0.0` so negative zero and positive zero share one canonical representation. The outer array is ordered by device then sequence.

This fingerprint binds evaluation to the exact imported dataset used by `train`, including held-out rows. It is provenance, not a learned feature. Never pass held-out CPU values into the fitting function. Store schema version, model kind, tolerance, fingerprint, split rule `chronological-60-20-20`, per-device split counts, training mean, last training value, and trained-through sequence.

Use these exact top-level artifact keys: `schema_version`, `model_kind`, `tolerance`, `dataset_sha256`, `split_rule`, and `devices`. Each device entry has exactly `mean`, `count`, `trained_through`, `last_training_value`, `train_count`, `validation_count`, and `test_count`. Here `count` equals `train_count`; all three split counts must be positive integers excluding booleans. `last_training_value` is a finite CPU in [0, 100]. The fingerprint is 64 lowercase hexadecimal characters. Counts, device names, and training summaries must agree with the current database and split. Reject unknown fields rather than silently extending version 1.

The row list passed into SHA-256 includes held-out rows only to bind provenance. The fitting interface receives training rows alone. Keep these two inputs separate in the code and in leakage tests.

### Validate before trusting a saved model

On load, check exact required fields, supported version/kind/split rule, device coverage, finite numeric values, integer counts, and fingerprint shape. Reject duplicate JSON keys. Before evaluation, recompute the dataset fingerprint and split metadata and require a match. A changed database requires a new training run and a new output path; do not silently evaluate a stale artifact.

For this teaching application, also recompute training summaries from the training slices and compare them with the artifact. This verifies internal consistency. It does not authenticate who created a model: a hash and self-consistent JSON are not a signature.

### Keep reports honest

Evaluate each held-out row with the frozen mean and last-training-value predictions. Emit validation and test counts, per-device MAE, pooled MAE, and residual flags with identity and split. Keep fixed-rule high status separately named. Refuse training if any included device has fewer than five readings. An empty history can be reported but cannot train a model.

## Self-review

Why retain raw observations instead of only classifications? Which part of the model artifact proves provenance, and which part determines predictions?

## Practice

Complete the [exercise specification](exercises/lesson02/lesson02.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson02-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/library/hashlib.html)
