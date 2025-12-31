# Day 2 — LSTM Single-Step Forecasting

## 🎯 Goal of the Day

The goal of Day 2 was **not accuracy** and **not production readiness**.

The real question we wanted to answer was:

> **Can a minimal LSTM learn _anything meaningful_ from our dataset?**

If the answer was _no_, then:

- more layers would not help
    
- more epochs would not help
    
- more complex models would only hide the real problem
    

So the entire day was designed as a **sanity check**.

---

## 1. Why We Started With Single-Step Prediction

Our dataset from Day 1 provided:

- `X`: past **30 minutes** of RPS
    
- `y`: next **12 minutes** of RPS
    

Instead of predicting all 12 steps, we **intentionally simplified** the task:

- We used only `y[:, 0]`
    
- The model predicts **just the next minute**
    

### Why this matters

Single-step prediction answers one foundational question:

> _Is there short-term temporal signal in the data that a model can extract?_

If a model cannot predict the **very next timestep**, then:

- multi-step forecasting is guaranteed to fail
    
- the issue is with data framing or training, not model size
    

This simplification is a **diagnostic tool**, not a shortcut.

---

## 2. Data Preparation for LSTM

### Shape requirement

LSTMs require inputs in the form:

```
(batch, timesteps, features)
```

Our original data was:

```
(N, 30)
```

So we explicitly added a feature dimension:

```
(N, 30) → (N, 30, 1)
```

### Why this is important

This step tells the model:

- what dimension represents **time**
    
- what dimension represents **features**
    

Without this, the LSTM would not know how to interpret the sequence.

---

## 3. Train / Validation Split Philosophy

We split the dataset as:

- **80% training**
    
- **20% validation**
    

Why this split exists:

1. Training loss alone can lie
    
2. Validation loss tells us whether the model generalizes
    
3. Validation samples represent **future behavior** relative to training
    

This allows us to detect:

- overfitting
    
- underfitting
    
- training instability
    

---

## 4. Minimal LSTM Architecture (Deliberately Small)

### Model structure

```
Input (30 × 1)
   ↓
LSTM (hidden size = 32)
   ↓
Last hidden state
   ↓
Linear layer
   ↓
Single prediction
```

### Design principles

- One LSTM layer only
    
- No stacking
    
- No dropout
    
- No bidirectionality
    

The goal was **clarity**, not power.

If this minimal model failed, adding complexity would only hide the problem.

---

## 5. Why We Use the Last Hidden State

The LSTM outputs a hidden state for **every timestep**.

We intentionally selected:

- the **last hidden state**
    

Because it represents:

> _Everything the model remembers after seeing the entire past window._

This is the most natural representation for next-step prediction.

---

## 6. Loss Function and Optimizer Choices

### Loss: Mean Squared Error (MSE)

Chosen because:

- this is a regression problem
    
- large errors are penalized strongly
    
- gradients are smooth and stable
    

### Optimizer: Adam

Chosen because:

- adaptive learning rates
    
- stable defaults
    
- minimal tuning required
    

At this stage, changing optimizers would add noise, not insight.

---

## 7. Training Loop Design

We used an **explicit training loop** instead of a high-level `.fit()` API.

Why this matters:

- every step of learning is visible
    
- gradients are explicitly controlled
    
- debugging is straightforward
    

Key principles followed:

- gradients are zeroed every batch
    
- backpropagation happens once per batch
    
- validation runs with gradients disabled
    

This ensures training behavior is correct and observable.

---

## 8. Results and Interpretation

### Loss behavior

- Training loss decreased rapidly
    
- Validation loss decreased and stabilized
    
- No divergence or collapse observed
    

This indicates:

- the model is learning signal
    
- not memorizing noise
    
- training is stable
    

### Prediction behavior

The predicted curve:

- followed the **direction and shape** of the real signal
    
- was smoother than actual values
    
- did not collapse to a constant
    

This is expected behavior for:

- MSE loss
    
- single-step forecasting
    
- minimal LSTM
    

The model learned **local temporal structure**, which was the goal.

---

## 9. What Day 2 Proved

By the end of Day 2, we proved:

1. The data pipeline from Day 1 is correct
    
2. The normalization and windowing are correct
    
3. An LSTM can extract meaningful signal from the workload
    
4. CPU-based PyTorch training is sufficient at this scale
    

This eliminates a large class of potential future bugs.

---

## 10. What Day 2 Did _Not_ Claim

It is important to be precise about limitations:

- This is not a production forecaster
    
- Spikes are not predicted sharply
    
- Multi-step forecasting is not yet handled
    

Day 2 was about **existence of learning**, not performance.

---

## 🚀 Ready for Day 3

With single-step learning validated, we are now ready to:

- extend the model to **12-step forecasting**
    
- reason about error accumulation
    
- evaluate longer-horizon predictions
    

Day 2 established confidence.  
Day 3 will increase difficulty — not complexity.


```ouput
Epoch 01 | Train MSE: 0.0143 | Val MSE: 0.0033
Epoch 02 | Train MSE: 0.0030 | Val MSE: 0.0024
Epoch 03 | Train MSE: 0.0027 | Val MSE: 0.0022
Epoch 04 | Train MSE: 0.0024 | Val MSE: 0.0021
Epoch 05 | Train MSE: 0.0023 | Val MSE: 0.0020
Epoch 06 | Train MSE: 0.0022 | Val MSE: 0.0021
Epoch 07 | Train MSE: 0.0022 | Val MSE: 0.0019
Epoch 08 | Train MSE: 0.0021 | Val MSE: 0.0026
Epoch 09 | Train MSE: 0.0021 | Val MSE: 0.0019
Epoch 10 | Train MSE: 0.0021 | Val MSE: 0.0018
Epoch 11 | Train MSE: 0.0021 | Val MSE: 0.0018
Epoch 12 | Train MSE: 0.0020 | Val MSE: 0.0021
Epoch 13 | Train MSE: 0.0020 | Val MSE: 0.0018
Epoch 14 | Train MSE: 0.0020 | Val MSE: 0.0018
Epoch 15 | Train MSE: 0.0020 | Val MSE: 0.0018
```

![[Pasted image 20251222131026.png]]