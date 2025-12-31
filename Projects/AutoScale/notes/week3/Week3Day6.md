# 🟧 Day 6 — Evaluation & Failure Analysis

## 🎯 Objective

Evaluate all models **as autoscaling decision systems**, not as ML benchmarks.

The goal is to understand **how each model fails**, and which failure modes are acceptable in a production autoscaling environment.

---

## 🧪 Models Evaluated

- **AR (Week 1)** — linear autoregressive baseline
    
- **LSTM** — recurrent model with explicit long-term memory
    
- **GRU** — simplified recurrent model with faster, smoother memory
    
- **Transformer Encoder** — attention-based temporal mixing model
    

All models were trained and evaluated on the **same dataset**, using the **same prediction horizon (12 steps)**.

---

## 📏 Evaluation Metrics

### Mean Squared Error (MSE)

- Penalizes large errors heavily
    
- Sensitive to spikes and extreme misses
    
- Proxy for **risk** (under-provisioning is costly)
    

### Mean Absolute Error (MAE)

- Measures average deviation
    
- Less sensitive to outliers
    
- Proxy for **cost efficiency**
    

**Autoscale interpretation:**

- MSE ≈ safety & SLA risk
    
- MAE ≈ resource efficiency
    

Both metrics are necessary.

---

## 🌊 Workload Types Tested

### 🟢 Smooth Workload

Characteristics:

- Gradual trends
    
- Low variance
    
- Predictable behavior
    

Used to test:

- stability
    
- overfitting
    
- unnecessary model complexity
    

---

### 🔴 Bursty Workload

Characteristics:

- Sudden spikes
    
- Sharp drops
    
- Non-stationary patterns
    

Used to test:

- lag
    
- overshoot
    
- failure severity
    

This workload is **critical** for autoscaling.

---

## 🔬 Model-by-Model Failure Analysis

### 🧮 AR (Autoregressive Baseline)

**Failure Mode**

- Strong lag during sudden spikes
    
- No ability to adapt to regime changes
    
- Blind linear extrapolation
    

**Observed Behavior**

- Consistently under-predicts bursts
    
- Predictable but slow response
    
- Low variance, low adaptability
    

**Autoscale Verdict**

- ❌ Unsafe as a standalone predictor
    
- ✅ Useful as a baseline and sanity check
    

---

### 🧠 LSTM

**Failure Mode**

- Overreacts to recent patterns
    
- Retains outdated memory after regime shifts
    
- Sensitive to hyperparameters
    

**Observed Behavior**

- Captures long-term structure well
    
- May overshoot after spikes
    
- Slower convergence compared to GRU
    

**Autoscale Verdict**

- ⚠️ Powerful but risky
    
- Requires careful tuning and safeguards
    
- Overshoot risk acceptable, memory inertia is not
    

---

### ⚡ GRU

**Failure Mode**

- Smooths sharp spikes
    
- Underestimates rare extreme events
    
- Faster forgetting of anomalies
    

**Observed Behavior**

- Very stable training
    
- Fast convergence
    
- Conservative, mean-oriented predictions
    

**Autoscale Verdict**

- ✅ Best default choice
    
- Predictable failures
    
- Safe when combined with buffer margins
    

---

### 🧠⚡ Transformer Encoder (Minimal)

**Failure Mode**

- Bottleneck collapse into single vector
    
- Limited spike preservation
    
- Complexity not fully utilized
    

**Observed Behavior**

- Slightly better mid-range alignment than GRU
    
- Less aggressive smoothing
    
- Performance gains marginal under current architecture
    

**Autoscale Verdict**

- 🧪 Promising but unjustified in current form
    
- Requires decoder or relaxed bottleneck to shine
    
- Overkill for current Autoscale stage
    

---

## ⏱️ Lag vs Overshoot (Critical Autoscale Lens)

### Lag

- Causes under-provisioning
    
- Leads to outages and SLA violations
    
- **Unacceptable**
    

### Overshoot

- Wastes resources
    
- Increases cost
    
- **Acceptable and manageable**
    

**Preferred failure pattern:**

> Predictable overshoot > unpredictable lag

---

## 🧠 Comparative Summary

|Model|Main Failure|Risk Profile|Autoscale Suitability|
|---|---|---|---|
|AR|Lag|High|❌|
|LSTM|Overshoot + memory inertia|Medium|⚠️|
|GRU|Smoothing spikes|Low|✅|
|Transformer|Bottleneck-limited|Medium|🧪|

---

## ✅ Final Justification (Checkpoint)

**GRU is chosen for Autoscale** because:

- It converges quickly
    
- It is stable and predictable
    
- Its failure mode (smoothing) is safer than lag
    
- Safety margins can compensate for underestimation
    

Other models either:

- react too slowly (AR),
    
- react too aggressively (LSTM),
    
- or require architectural expansion (Transformer).
    

---

## 🏁 Key Takeaway

Model choice for autoscaling is **not about accuracy alone**.

> It is about **how the model fails**,  
> and whether those failures are survivable in production.

Day 6 marks the shift from **model building** to **system design**.