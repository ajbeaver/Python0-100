# Lesson 9.2 — Fitting a predictive baseline

**Course:** Python 0–100, Edition 2  
**Module:** [09 — Advanced Python and Machine Learning](README.md)  
**Prerequisite:** Lesson 9.1  
**Estimated study time:** 40–65 minutes

## Learning objectives

1. Distinguish a fixed rule from a learned parameter.
2. Fit per-device means from training data.
3. Define a safe model artifact.

### Learning can begin with one parameter

A fixed threshold of 80 is a hand-written rule. A mean predictor estimates a parameter from observations: for each device, predict its next CPU value as the arithmetic mean of that device's training CPU values. This is a small statistical learning model. It does not infer causes or understand a machine.

For a device with training values 20, 30, and 40, the fitted mean is 30. A prediction function should use that stored mean without updating it from the value it is supposed to predict. The fitted artifact and the current observation have different lifetimes.

```python
from statistics import fmean

training = [20.0, 30.0, 40.0]
fitted_mean = fmean(training)
print(fitted_mean)
print(abs(45.0 - fitted_mean))
```

The absolute difference is a **residual**. A policy can label a point unusual when its residual is strictly greater than a chosen tolerance. “Unusual” is separate from the fixed-rule “high” label. A value can be high but unsurprising for a heavily loaded device.

### Fit only the intended data

Group by device and use only training rows. Reject an empty group. Store count and `trained_through` (the greatest training sequence) alongside each mean. Reject non-finite values before fitting. Do not fit on the held-out observations or silently share one device's mean with another.

### Persist data rather than executable objects

Use a versioned JSON artifact: schema version 1, model kind `device_mean`, tolerance, and a mapping from device to mean, count, and trained-through sequence. Validate every field on load. Require a positive integer count, finite mean in [0, 100], nonnegative integer sequence, and finite tolerance in [0, 100], excluding booleans for numeric fields.

Save a new model with an explicit UTF-8 text handle and `json.dump(..., allow_nan=False)`. Opening a path with mode `"x"` refuses to overwrite an existing file; mode `"w"` truncates it. Exclusive creation prevents accidental replacement but a failed write can leave a partial new file. Report that failure and use a new path, or deliberately remove only the incomplete file your operation created. Loading must reject partial or malformed artifacts.

Unknown devices must produce an explicit no-model error. Do not deserialize untrusted `pickle` files; the course model needs only numbers and strings. JSON still requires schema validation. Serialization should preserve predictions after save/load and must never retrain implicitly.

## Self-review

Why is a mean predictor learning while a fixed threshold alone is not? Which evidence would reveal that inference accidentally updates its model?

## Practice

Complete the [exercise specification](exercises/lesson02/lesson02.md) from empty source files. Keep predictions, commands, observations, and reasoning in `lesson02-answers.md` beside it. Continue through the [module index](README.md).

## Further reading

- [Official Python reference](https://docs.python.org/3/library/statistics.html#statistics.fmean)
