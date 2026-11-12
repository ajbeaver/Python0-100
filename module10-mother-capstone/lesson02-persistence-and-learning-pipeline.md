# Lesson 10.2 — Persistence and learning pipeline

**Course:** Python 0–100, Edition 2  
**Module:** [10 — Mother — Final Capstone](README.md)  
**Prerequisite:** Lesson 10.1  
**Estimated study time:** 60–90 minutes

## Learning objectives

1. Integrate transactions and model fitting.
2. Bind a model to a reproducible dataset.
3. Reject drift in evaluation inputs.

## 10.2.1 — Store observations, derive decisions

Persist validated CPU observations, not only “high” labels. A later threshold can then produce a new report without rewriting history. Integrate Module 08's single-writer transaction policy, explicit close, placeholders, and ordered readback. Test reopen behavior before adding the model pipeline.

## 10.2.2 — A model needs provenance

Extend Module 09's artifact with a dataset fingerprint and split metadata. Compute SHA-256 over UTF-8 JSON encoding of the complete ordered rows as `[device, sequence, float(cpu)]` arrays, using `ensure_ascii=True`, `allow_nan=False`, and `separators=(",", ":")`. Normalize any zero CPU to `0.0` so negative zero and positive zero share one canonical representation. The outer array is ordered by device then sequence.

This fingerprint binds evaluation to the exact imported dataset used by `train`. The artifact also records the model parameters and the split rule `chronological-60-20-20`.

Use these exact top-level artifact keys: `schema_version`, `model_kind`, `tolerance`, `dataset_sha256`, `split_rule`, and `devices`. Each device entry has exactly `mean`, `count`, `trained_through`, `last_training_value`, `train_count`, `validation_count`, and `test_count`. Here `count` equals `train_count`; all three split counts must be positive integers excluding booleans. `last_training_value` is a finite CPU in [0, 100]. The fingerprint is 64 lowercase hexadecimal characters. Counts, device names, and training summaries must agree with the current database and split. Reject unknown fields rather than silently extending version 1.

The row list passed into SHA-256 includes held-out rows only to bind provenance. The fitting interface receives training rows alone. Keep these two inputs separate in the code and in leakage tests.

## 10.2.3 — Canonical encoding makes a hash reproducible

A cryptographic hash turns a byte sequence into a fixed-size digest. It does not decide which objects should be considered equivalent. JSON text can represent the same values with different spacing or record ordering, so the application must choose one encoding before hashing.

Sorting by device and sequence removes input-order differences. Converting CPU to float aligns accepted integer and floating representations. Normalizing zero prevents `-0.0` and `0.0` from producing different bytes for the same numeric reading. Fixed separators remove optional whitespace. The resulting bytes are the agreed fingerprint input.

The digest is sensitive to changed values, identities, added rows, and missing rows. That is why it detects a stale evaluation dataset. It does not prove that the original observations were truthful or that the file came from a trusted author.

## 10.2.4 — Validate before trusting a saved model

On load, check exact required fields, supported version/kind/split rule, device coverage, finite numeric values, integer counts, and fingerprint shape. Reject duplicate JSON keys. Before evaluation, recompute the dataset fingerprint and split metadata and require a match. A changed database requires a new training run and a new output path; do not silently evaluate a stale artifact.

Also recompute training summaries from the training slices and compare them with the artifact. This is an internal-consistency check, separate from checking the dataset fingerprint; neither authenticates the model's author.

## 10.2.5 — Trace two independent consistency checks

Suppose the last test CPU changes from 60 to 61. The training mean should remain 30 because the training slice is unchanged. The complete-dataset fingerprint should change because evaluation would now concern a different outcome. The application rejects the old artifact until a deliberate new training run binds a new one.

Now suppose the dataset stays fixed but somebody edits the artifact's mean to 31. The fingerprint still matches. Recomputing the training summary detects this different inconsistency. Keeping provenance and parameter validation separate explains why both checks exist.

## 10.2.6 — Avoid comparing different snapshots

Read one ordered dataset and derive its fingerprint, splits, and training summaries from that captured set. If the application repeatedly reads while another operation changes history, those computations could refer to different states. The capstone's single-writer workflow is deliberately simple; document that assumption rather than claiming arbitrary concurrent evaluation is supported.

Finally, comparison happens before trusting the model's metrics. A valid artifact that cannot be bound to the current history should produce a clear error, not a partially computed report. This is the same prepare-then-publish principle used for observation ingestion.

## 10.2.7 — Keep reports honest

Evaluate each held-out row with the frozen mean and last-training-value predictions. Emit validation and test counts, per-device MAE, pooled MAE, and residual flags with identity and split. Keep fixed-rule high status separately named. Refuse training if any included device has fewer than five readings. An empty history can be reported but cannot train a model.

## Self-review

Why retain raw observations instead of only classifications? Which part of the model artifact proves provenance, and which part determines predictions?

## Practice

Complete the [exercise specification](exercises/lesson02/lesson02.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson02-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Python reference — Hashing](https://docs.python.org/3/library/hashlib.html)
