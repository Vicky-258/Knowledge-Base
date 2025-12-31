# Day 1 — Data Pipeline & Preprocessing Notes

## Context

Today marked the **real transition from learning concepts to building the system**. Instead of jumping into models, we focused on something more fundamental:

> Turning raw time-series traffic into a **clean, reproducible supervised learning dataset**.

This day was not about ML accuracy — it was about **correct framing and engineering discipline**.

---

## 1. Initial Problem

We started with a simple but critical realization:

- We **cannot train a forecasting model** directly on a raw time-series.
    
- ML models need **input–output pairs**, not a continuous signal.
    

So the core question became:

> How do we convert workload traffic into a supervised learning problem _correctly_?

---

## 2. Data Availability Reality Check

### Question Raised

> “Isn’t traffic data already available on the internet?”

### Insight Gained

- Public traffic datasets **do exist**, but:
    
    - Often have wrong granularity (hourly/daily)
        
    - Lack autoscaling context
        
    - Are messy or over-aggregated
        

### Decision

We chose to **start with synthetic data**, not to fake results, but to:

- Validate the pipeline
    
- Control assumptions
    
- Avoid debugging chaos early
    

This mirrors how real infra teams prototype systems.

---

## 3. Designing Synthetic RPS (Engineering, not randomness)

We defined **explicit traffic assumptions**:

- Sampling: **1 minute per point**
    
- Duration: **7 days (10,080 points)**
    
- Baseline: **25 RPS**
    
- Daily seasonality: human-driven 24h cycle
    
- Noise: small, realistic fluctuations
    
- Spikes: ~3/day, ~20 minutes, autoscaler-relevant
    

Key realization:

> Synthetic data is valid **only if it obeys the physics of the system**.

The resulting signal passed the test:

- Kubernetes would _actually care_ about this traffic.
    

---

## 4. Major Architectural Decision (Important)

### Problem

Running data generation every time mixes concerns:

- generation
    
- preprocessing
    
- training
    

### Solution

We **separated responsibilities**:

```
[ generate_data.py ]  → creates raw traffic once
        ↓
[ data/raw/rps.npy ] → frozen ground truth
        ↓
[ preprocess.py ]    → normalization + windows
        ↓
[ data/processed/ ]  → ML-ready arrays
```

This guarantees:

- Reproducibility
    
- No accidental data leakage
    
- Clean experimentation
    

---

## 5. Normalization — Why and How

### Why normalization was necessary

- Stabilizes gradients
    
- Prevents activation saturation
    
- Makes the model learn **patterns**, not magnitudes
    
- Enables future data source swapping
    

### Choice

- **MinMaxScaler** chosen over StandardScaler
    

Reason:

- Preserves shape
    
- Intuitive [0, 1] stress scale
    
- Common in time-series forecasting
    

### Critical rule followed

- Scaler **fit only on training portion** (80%)
    
- Entire series transformed using that scaler
    

This avoided future data leakage.

---

## 6. Converting Time-Series → Dataset (Core of Day 1)

We implemented sliding windows:

- **X** = past **30 minutes**
    
- **y** = next **12 minutes**
    

This reframed the problem as:

> “Given the past 30 minutes of RPS, predict the next 12 minutes.”

Shapes verified:

- `X.shape = (10039, 30)`
    
- `y.shape = (10039, 12)`
    

Math validated:

```
10080 − 30 − 12 + 1 = 10039
```

---

## 7. Visual Sanity Checks (Non-negotiable)

We plotted:

- Raw synthetic RPS (7 days)
    
- Normalized RPS
    
- One `(X, y)` sample
    

Why this mattered:

- Prevents silent time reversal bugs
    
- Confirms future truly follows past
    
- Builds trust in the dataset
    

The `(X, y)` plot looked intuitive and physically correct.

---

## 8. Artifacts Produced (End of Day Assets)

By the end of Day 1, we had:

- `data/raw/rps_7d_1min.npy`
    
- `data/processed/X.npy`
    
- `data/processed/y.npy`
    
- `data/processed/scaler.pkl`
    

These are **frozen, reusable, and reproducible**.

Training code will only consume processed data — never raw signals.

---

## Final Outcome

Day 1 successfully achieved:

- Correct problem framing
    
- Realistic workload simulation
    
- Clean preprocessing pipeline
    
- Leak-free normalization
    
- Verified supervised dataset
    

Most importantly, we now have **confidence** in the data.

From here onward:

> Any modeling success or failure is _real_, not accidental.