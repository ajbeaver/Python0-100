# Lesson 9.3 — Evaluation, leakage, and model limits

**Course:** Python 0–100, Edition 2  
**Module:** [09 — Advanced Python and Machine Learning](README.md)  
**Prerequisite:** Lesson 9.2  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Split observations chronologically per device.
2. Compute MAE against a baseline.
3. Separate evaluation from tuning.

## 9.3.1 — Future data must remain unseen

For each device, sort unique readings by sequence and use the earliest 60% for training, the next 20% for validation, and the rest for final testing. Use `train_end = int(0.6 * n)` and `validation_end = int(0.8 * n)`, with at least five readings per device. Sequence expresses order within a device; it is not a wall-clock timestamp.

Fit the mean on the training portion only. Validation may inform a later model decision. In this course, tolerance remains the declared 15; final testing is performed once after the design is fixed. Repeatedly adjusting a model to improve test results leaks information from the test into development.

## 9.3.2 — Work through the split before computing a score

With five readings, `int(0.6 * 5)` is 3 and `int(0.8 * 5)` is 4. Python slicing therefore gives training rows `[:3]`, validation rows `[3:4]`, and test rows `[4:]`. The stop index is excluded, as in Module 01's sequence examples. Splitting happens independently for each device after sorting by its integer sequence.

The minimum-history rule ensures all three portions contain data for the specified ratios. It does not mean a single held-out reading is statistically persuasive. This tiny fixture exists so you can verify each stage without hiding errors in a large dataset.

## 9.3.3 — Compare against something simple

Mean absolute error is `sum(abs(actual - predicted)) / count`. The persistence baseline predicts every held-out value using the last training CPU for that device, held fixed across validation and test. It must not peek at preceding held-out values under this protocol. A rolling one-step forecast is a different valid experiment, but requires a different stated protocol.

| Sequence | CPU | Role |
|---|---:|---|
| 0, 1, 2 | 20, 30, 40 | Train: mean 30; persistence 40 |
| 3 | 50 | Validate |
| 4 | 60 | Test |

For this one-device table, the mean model's validation MAE is 20 and test MAE is 30; persistence scores 10 and 20. The fitted mean loses on this trend. The next section shows how to combine such errors across devices without hiding the weighting.

## 9.3.4 — Follow one score all the way through

For node01, train on [20, 30, 40], predict 30, then observe validation CPU 50. The absolute error is 20. For a second device whose values stay at 60, the mean is 60 and the corresponding error is 0. Pooled validation MAE is `(20 + 0) / 2 = 10`.

If one device contributed ten held-out rows and another contributed one, pooled MAE would weight every row equally. Averaging two per-device MAEs would weight the devices equally instead. Either can answer a useful question, but they are not interchangeable. Record counts so a reader can interpret the aggregate.

## 9.3.5 — Test leakage without requiring good predictions

Change only held-out CPU values, fit again from the same training slice, and require the parameters to remain unchanged. The scores should change because the actual outcomes changed. This is a stronger test of separation than asserting that the score looks small.

A random split would let later readings influence a predictor evaluated on earlier readings. That may answer a different question about a stationary dataset, but it does not represent the course's “learn from earlier observations, predict later ones” protocol. Mother retains chronological evaluation even when that makes its model's limitations more visible.

## 9.3.6 — Measurement has limits

Report sample counts with the per-device and pooled metrics so their weighting remains visible. A drifting device can make an old mean obsolete. Synthetic fixtures prove arithmetic and mechanics; they do not establish real anomaly-detection quality.

Residual labels have no accuracy metric here because the observations have no independent ground-truth anomaly labels. Do not call every residual above 15 a confirmed incident. Mother's final report must separate predictive error, residual flags, and fixed-threshold status.

## Self-review

Why does a random split misrepresent forecasting on drifting observations? Why can residual flags not supply anomaly accuracy without independent labels?

## Practice

Complete the [exercise specification](exercises/lesson03/lesson03.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson03-answers.md` beside it. Then complete the [module assessment](assessment.md).

## Further reading

- [Python reference — Statistics](https://docs.python.org/3/library/statistics.html)
