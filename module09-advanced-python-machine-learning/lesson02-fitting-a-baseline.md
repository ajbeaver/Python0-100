# Lesson 9.2 — Fitting a predictive baseline

**Course:** Python 0–100, Edition 2  
**Module:** [09 — Advanced Python and Machine Learning](README.md)  
**Prerequisite:** Lesson 9.1  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Distinguish a fixed rule from a learned parameter.
2. Fit per-device means from training data.
3. Define a safe model artifact.

## 9.2.1 — Learning can begin with one parameter

A fixed threshold of 80 is a hand-written rule. A mean predictor estimates a parameter from observations: for each device, predict its next CPU value as the arithmetic mean of that device's training CPU values. This is a small statistical learning model. It does not infer causes or understand a machine.

The fitted artifact and the current observation have different lifetimes: training creates a parameter; later observations are assessed using it. Start with the arithmetic, then trace where the parameter is used.

```python
from statistics import fmean

training = [20.0, 30.0, 40.0]
fitted_mean = fmean(training)
print(fitted_mean)
print(abs(45.0 - fitted_mean))
```

The absolute difference is a **residual**. A policy can label a point unusual when its residual is strictly greater than a chosen tolerance. “Unusual” is separate from the fixed-rule “high” label. A value can be high but unsurprising for a heavily loaded device.

## 9.2.2 — Fitting and inference are separate operations

**Fitting** computes model parameters from a selected training dataset. **Inference** uses those parameters to produce a prediction. With a per-device mean, fitting needs all the chosen training CPU values for a device; inference needs only that device's stored mean. A later actual value is used to assess the prediction, not to revise it during the same evaluation.

For training values 20, 30, and 40, the sum is 90, the count is 3, and the mean is 30. This model assumes a roughly stable level is a useful prediction. A rising series violates that assumption: averaging older low values can lag behind the latest training value. Module 09's evaluation compares those assumptions instead of assuming a fitted model must be superior.

## 9.2.3 — Fit only the intended data

Group by device and use only training rows. Reject an empty group. Store count and `trained_through` (the greatest training sequence) alongside each mean. Reject non-finite values before fitting. Do not fit on the held-out observations or silently share one device's mean with another.

## 9.2.4 — Prediction error and a decision policy

A signed residual can be defined as actual minus predicted. The example uses its absolute magnitude, `abs(actual - predicted)`, because the unusualness rule cares about distance in either direction. Keep the convention explicit when naming fields.

| Actual | Prediction | Absolute residual | Unusual at tolerance 15? |
|---|---:|---:|---|
| 45 | 30 | 15 | False |
| 46 | 30 | 16 | True |
| 14 | 30 | 16 | True |

The tolerance is chosen configuration, not a learned mean. Neither residual flag says why a measurement changed. Device-specific modeling avoids treating a normally busy device as identical to a normally idle one, but small or unrepresentative training histories can still produce misleading expectations.

## 9.2.5 — Persist data rather than executable objects

Use a versioned JSON artifact: schema version 1, model kind `device_mean`, tolerance, and a mapping from device to mean, count, and trained-through sequence. Validate every field on load. Require a positive integer count, finite mean in [0, 100], nonnegative integer sequence, and finite tolerance in [0, 100], excluding booleans for numeric fields.

Save a new model with an explicit UTF-8 text handle and `json.dump(..., allow_nan=False)`. Opening a path with mode `"x"` refuses to overwrite an existing file; mode `"w"` truncates it. Exclusive creation prevents accidental replacement but a failed write can leave a partial new file. Report that failure and use a new path, or deliberately remove only the incomplete file your operation created. Loading must reject partial or malformed artifacts.

Unknown devices must produce an explicit no-model error. Do not deserialize untrusted `pickle` files; the course model needs only numbers and strings. JSON still requires schema validation. Serialization should preserve predictions after save/load and must never retrain implicitly.

## 9.2.6 — A model file is an interface across runs

The schema version tells a loader which fields and meanings to expect. Counts and sequence metadata explain where a parameter came from; they do not substitute for its value. Rejecting an unknown device is more informative than silently borrowing another device's parameter.

Round-trip testing means fitting once, saving, loading, and comparing predictions from the same stored parameters. It must not fit again during loading: retraining can conceal serialization errors by reconstructing what the file failed to preserve.

## Self-review

Why is a mean predictor learning while a fixed threshold alone is not? Which evidence would reveal that inference accidentally updates its model?

## Practice

Complete the [exercise specification](exercises/lesson02/lesson02.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson02-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Python reference — Arithmetic means](https://docs.python.org/3/library/statistics.html#statistics.fmean)
