# Lesson 10.3 — Final verification and handoff

**Course:** Python 0–100, Edition 2  
**Module:** [10 — Mother — Final Capstone](README.md)  
**Prerequisite:** Lesson 10.2  
**Estimated study time:** 60–90 minutes

## Learning objectives

1. Reproduce the full run from an empty workspace.
2. Demonstrate failure and recovery.
3. Explain model limits and operational assumptions.

## 10.3.1 — A clean run is a different test

Your usual working directory may contain a database, cached imports, or an old model that hides missing setup. Copy only source, tests, fixtures, and documentation into a fresh temporary project directory. Create a new virtual environment there, then run the documented commands from that project root.

```sh
python -m unittest discover -s tests -v
python -m mother --help
python -m mother ingest fixtures/demo.json --db demo.sqlite
python -m mother ingest fixtures/demo.json --db demo.sqlite
python -m mother report --db demo.sqlite --threshold 80
python -m mother train --db demo.sqlite --model model.json --tolerance 15
python -m mother evaluate --db demo.sqlite --model model.json
```

These commands assume your implementation and fixture exist; they are the final acceptance interface, not supplied starter solutions. Record the Python version and SQLite runtime version. A passing command with zero collected tests is not evidence of a tested application: record the actual test count.

## 10.3.2 — Read the demonstration as evidence for distinct claims

A passing unit suite establishes the cases it actually collects. A clean CLI run additionally exercises launch paths, input files, resource creation, and serialization together. Neither substitutes for the other. The handoff should let a reader trace a claim to the command or test that supports it.

For example, replay evidence needs the row set before and after a second ingestion, not only its exit status. Rollback evidence needs a new row placed before the conflicting row in the attempted batch, so a partial-commit bug would be visible. Restart evidence needs a new process opening persisted state rather than an existing in-memory connection.

## 10.3.3 — Demonstrate intermediate state

For the ten-row fixture, first ingestion inserts ten rows. Replay adds none. Node01's mean is 30; node02's is 60. Mean validation/test pooled MAE is 10/15; persistence is 5/10. No CPU exceeds 80 in this fixture, so use a separate boundary fixture or test for 79, 80, and 81.

Submit malformed and conflicting imports against a disposable database and show unchanged rows afterward. Demonstrate model-output refusal, stale-model rejection, and restart behavior. Record statuses and separate stdout from stderr. Review model files to confirm there are no machine-specific hidden paths required for use.

## 10.3.4 — Isolate failure demonstrations from the scoring fixture

Keep a stable ten-row dataset for reproducing the known metrics. Use a separate disposable copy for conflict and stale-model tests. A failure experiment can intentionally change a row; it should not silently change the dataset used for the reported baseline comparison.

Compare expected and actual intermediate values: inserted and unchanged counts, two training means, split counts, and four pooled MAEs. A final successful process status cannot reveal that training accidentally included a validation row or that an import briefly committed a partial batch before later cleanup.

## 10.3.5 — Explain the delivery and model limits

The README should cover setup, schemas, commands, tests, replay policy, transaction guarantees, and recovery. `model-card.md` should name training data, splitting protocol, metrics, residual tolerance, leakage checks, and the limits discussed below.

List the single-writer assumption and the distinction between atomic import and crash durability. Keep source separate from generated databases and reports. Optional future work may add explicit sensor collection, a rolling evaluation protocol, or a richer predictor, but it must preserve the core's reproducible acceptance path.

State the question the model answers: predict a device's CPU from its earlier supplied CPU history under a fixed chronological protocol. State what it does not measure: causal diagnosis, independently labeled anomaly accuracy, or performance on live production observations. Include the persistence comparison even when it outperforms the learned mean.

A useful next step follows from a measured limitation. A trend might motivate a rolling protocol or a trend-aware predictor, but each changes assumptions and needs a new evaluation design. Adding complexity because it sounds more like machine learning would weaken the purpose of this capstone: a small system whose data, state changes, predictions, and limits you can explain end to end.

## Self-review

What do synthetic fixtures prove, and what remains unknown about real-world data? Why must a final demonstration start with both an empty database and absent model output?

## Practice

Complete the [exercise specification](exercises/lesson03/lesson03.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson03-answers.md` beside it. Then complete the [module assessment](assessment.md).

## Further reading

- [Python reference — Virtual environments](https://docs.python.org/3/library/venv.html)
