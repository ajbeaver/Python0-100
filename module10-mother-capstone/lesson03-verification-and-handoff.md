# Lesson 10.3 — Final verification and handoff

**Course:** Python 0–100, Edition 2  
**Module:** [10 — Mother — Final Capstone](README.md)  
**Prerequisite:** Lesson 10.2  
**Estimated study time:** 60–90 minutes

## Learning objectives

1. Reproduce the full run from an empty workspace.
2. Demonstrate failure and recovery.
3. Explain model limits and operational assumptions.

### A clean run is a different test

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

### Demonstrate intermediate state

For the ten-row fixture, first ingestion inserts ten rows. Replay adds none. Node01's mean is 30; node02's is 60. Mean validation/test pooled MAE is 10/15; persistence is 5/10. No CPU exceeds 80 in this fixture, so add a separate boundary fixture or test for 79, 80, and 81. Do not contaminate the evaluation dataset without retraining under a new artifact path.

Submit malformed and conflicting imports against a disposable database and show unchanged rows afterward. Demonstrate model-output refusal, stale-model rejection, and restart behavior. Record statuses and separate stdout from stderr. Review model files to confirm there are no machine-specific hidden paths required for use.

### Explain what was delivered

The README should cover setup, schemas, commands, tests, replay policy, transaction guarantees, and recovery. `model-card.md` should name training data, splitting protocol, metrics, residual tolerance, leakage checks, and limits. If the mean loses to persistence, report that result directly. Synthetic measurements do not justify production monitoring claims.

List the single-writer assumption and the distinction between atomic import and crash durability. Keep source separate from generated databases and reports. Optional future work may add explicit sensor collection, a rolling evaluation protocol, or a richer predictor, but it must preserve the core's reproducible acceptance path.

## Self-review

What do synthetic fixtures prove, and what remains unknown about real-world data? Why must a final demonstration start with both an empty database and absent model output?

## Practice

Complete the [exercise specification](exercises/lesson03/lesson03.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson03-answers.md` beside it. Then complete the [module assessment](assessment.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/library/venv.html)
