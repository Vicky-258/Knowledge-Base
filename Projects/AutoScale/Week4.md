#  WEEK 4 — Burst Detection System (BAScaler Core)

**Theme:** Prediction is useless without knowing when it breaks.
**Outcome:** A rule-based burst detector that classifies workload behavior as **normal**, **periodic spike**, or **burst**.

This week turns your frozen predictor into a **decision-aware system**.

---

## 🟧 Day 1 — Residuals & Error Anatomy

### 🎯 Goal:

Understand **how and where** your predictor is wrong.

### ✅ Tasks:

* [ ] Load:

  * frozen GRU predictor
  * validation dataset
* [ ] Generate predictions for validation set
* [ ] Compute residuals:

  * `residual = actual − prediction`
* [ ] Plot:

  * residual histogram (overall)
  * residuals per horizon (t+1 … t+12)
* [ ] Observe:

  * symmetry vs skew
  * variance growth with horizon

📌 **Checkpoint:**
You can answer:

> “What magnitude of error is *normal* for each horizon?”

---

## 🟧 Day 2 — Prediction Intervals (Uncertainty Envelope)

### 🎯 Goal:

Define what “expected error” means numerically.

### ✅ Tasks:

* [ ] For each horizon:

  * compute std / percentile of residuals
* [ ] Build prediction intervals:

  * lower = `pred − α·σ`
  * upper = `pred + α·σ`
* [ ] Choose α using validation data (e.g. 2 or 3)
* [ ] Plot:

  * actual vs prediction
  * prediction interval bands
* [ ] Verify:

  * most points fall inside
  * spikes break the band

📌 **Rule:**
Intervals must be **data-derived**, not arbitrary.

📌 **Checkpoint:**
You can visually explain why a point is “unexpected”.

---

## 🟧 Day 3 — Interval Violation Logic

### 🎯 Goal:

Detect **when reality exits the model’s confidence zone**.

### ✅ Tasks:

* [ ] Implement interval check:

  * inside → 0
  * outside → 1
* [ ] Generate a violation sequence over time
* [ ] Visualize:

  * workload
  * prediction interval
  * violation markers
* [ ] Confirm:

  * noise ≠ burst
  * single violations are common

📌 **Checkpoint:**
You clearly see the difference between noise and surprise.

---

## 🟧 Day 4 — Temporal Pattern Analysis (Paper Logic)

### 🎯 Goal:

Recreate **Table 1 / Fig. 2 logic** from the paper.

### ✅ Tasks:

* [ ] Maintain sliding window of violations (size W)
* [ ] Detect patterns:

  * isolated violations
  * consecutive violations
  * regularly spaced violations
* [ ] Implement counters:

  * violation frequency
  * violation duration
* [ ] Map patterns to behavior types

📌 **Rule:**
Detection is **temporal**, not pointwise.

📌 **Checkpoint:**
You can explain why two identical violations may lead to different labels.

---

## 🟧 Day 5 — Burst vs Periodic Spike Classification

### 🎯 Goal:

Differentiate **dangerous bursts** from **harmless periodic spikes**.

### ✅ Tasks:

* [ ] Define rules for:

  * `"normal"`
  * `"periodic spike detected but NOT burst"`
  * `"burst"`
* [ ] Encode:

  * persistence threshold
  * recovery time
  * spacing regularity
* [ ] Test rules on:

  * smooth workload
  * bursty workload
* [ ] Print classification results

📌 **Checkpoint:**
The detector’s decision is explainable in plain English.

---

## 🟧 Day 6 — Failure Analysis & Stress Testing

### 🎯 Goal:

Ensure the detector fails **conservatively**.

### ✅ Tasks:

* [ ] Test edge cases:

  * borderline violations
  * short bursts
  * noisy but stable regions
* [ ] Identify:

  * false positives
  * false negatives
* [ ] Ask:

  * would this cause unnecessary scaling?
  * would this miss a real incident?

📌 **Checkpoint:**
You trust the detector more than raw predictions.

---

## 🟧 Day 7 — Freeze the Burst Detector (Engineering Day)

### 🎯 Goal:

Turn burst detection into a **stable system component**.

### ✅ Tasks:

* [ ] Wrap logic into a clean module:

  * `update(actual, prediction)`
  * `get_state()`
* [ ] Ensure inference-only operation
* [ ] Remove training dependencies
* [ ] Write `week4_notes.md`:

  * detection logic
  * thresholds used
  * known limitations
* [ ] Define interface for scaling logic

📌 **End-of-Week Win:**
You now have a **model-aware burst detector** that knows when prediction is no longer safe.

---

