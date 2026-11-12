# Lesson 9.3 — Evaluation, leakage, and model limits

**Course:** Python 0–100, Edition 2  
**Module:** [09 — Advanced Python and Machine Learning](README.md)  
**Prerequisite:** Lesson 9.2  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Split observations chronologically per device.
2. Compute MAE against a baseline.
3. Separate evaluation from tuning.

### Future data must remain unseen

For each device, sort unique readings by sequence and use the earliest 60% for training, the next 20% for validation, and the rest for final testing. Use `train_end = int(0.6 * n)` and `validation_end = int(0.8 * n)`, with at least five readings per device. Sequence expresses order within a device; it is not a wall-clock timestamp.

Fit the mean on the training portion only. Validation may inform a later model decision. In this course, tolerance remains the declared 15; final testing is performed once after the design is fixed. Repeatedly adjusting a model to improve test results leaks information from the test into development.

### Compare against something simple

Mean absolute error is `sum(abs(actual - predicted)) / count`. The persistence baseline predicts every held-out value using the last training CPU for that device, held fixed across validation and test. It must not peek at preceding held-out values under this protocol. A rolling one-step forecast is a different valid experiment, but requires a different stated protocol.

| Sequence | CPU | Role |
|---|---:|---|
| 0, 1, 2 | 20, 30, 40 | Train: mean 30; persistence 40 |
| 3 | 50 | Validate |
| 4 | 60 | Test |

The mean model's validation MAE is 20 and test MAE is 30. Persistence scores 10 and 20. Learning does not guarantee improvement. An honest report should say the fitted mean loses on this trend.

### Measurement has limits

Report sample counts and per-device MAE as well as pooled MAE across all held-out rows. Do not average device MAEs without naming the weighting choice. A drifting device can make an old mean obsolete. Synthetic fixtures prove arithmetic and mechanics; they do not establish real anomaly-detection quality.

Residual labels have no accuracy metric here because the observations have no independent ground-truth anomaly labels. Do not call every residual above 15 a confirmed incident. Mother's final report must separate predictive error, residual flags, and fixed-threshold status.

## Self-review

Why does a random split misrepresent forecasting on drifting observations? Why can residual flags not supply anomaly accuracy without independent labels?

## Practice

Complete the [exercise specification](exercises/lesson03/lesson03.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson03-answers.md` beside it. Then complete the [module assessment](assessment.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/library/statistics.html)
