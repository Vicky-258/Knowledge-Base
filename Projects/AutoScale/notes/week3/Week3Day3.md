# Day 3 — Multi-Step Forecasting (12-Step Horizon)

## 🎯 Goal of the Day

The goal of Day 3 was **not to improve accuracy**, but to increase **forecasting difficulty**.

Specifically, we wanted to answer:

> **Can the model reason about the _future trajectory_ instead of just the next instant?**

This day marks the transition from _local continuity_ (Day 2) to _temporal planning_.

---

## 1. Why Multi-Step Forecasting Is Fundamentally Harder

Single-step forecasting:

- One prediction
    
- One error
    
- No compounding
    

Multi-step forecasting:

- One prediction vector (12 values)
    
- Errors at early steps influence later steps
    
- Uncertainty increases with horizon
    

This phenomenon is called **error accumulation**, and it is unavoidable.

Therefore, higher loss and smoother predictions are **expected outcomes**, not failures.

---

## 2. Forecasting Strategy Chosen: Direct Multi-Horizon

We used **direct multi-step forecasting**:

```
Input (past 30 mins) → Output (next 12 mins)
```

Each of the 12 future values is predicted **simultaneously** from the same latent state.

### Why this strategy

- Simple and stable
    
- No recursive feedback loops
    
- Easier to debug
    
- Clear error attribution per horizon
    

This avoids compounding feedback errors seen in recursive approaches.

---

## 3. Minimal Architectural Change

Only one structural change was required from Day 2:

- Output layer changed from `1` → `12`
    

```
Linear(32 → 1)   →   Linear(32 → 12)
```

Everything else remained identical:

- Same LSTM
    
- Same hidden size
    
- Same optimizer
    
- Same training loop
    

This confirmed that the original abstraction was correct.

---

## 4. Training Behavior and Stability

Observed behavior:

- Training loss decreased smoothly
    
- Validation loss followed closely
    
- No divergence or instability
    
- Loss plateaued at a higher value than single-step
    

This indicates:

- The model learned signal
    
- Capacity limits were reached naturally
    
- No overfitting occurred
    

The plateau is expected for a minimal model handling a harder task.

---

## 5. Visual Analysis of 12-Step Forecasts

### What the model learned

- Overall trend continuation
    
- Correct magnitude range
    
- Smooth trajectory across steps
    

### What the model did not learn

- Sharp spikes
    
- Sudden drops
    
- High-frequency volatility
    

This behavior is **correct** given:

- Mean Squared Error loss
    
- No spike-specific objectives
    
- Direct multi-horizon averaging
    

The model prefers **low-variance, stable predictions**.

---

## 6. Per-Horizon Error Analysis (Key Insight)

MSE increased monotonically with forecast horizon:

```
t+1  < t+2 < ... < t+12
```

This confirms:

- No target leakage
    
- No indexing bugs
    
- Proper temporal alignment
    

This pattern matches theoretical expectations for time-series forecasting.

---

## 7. Interpretation of Smoothing Behavior

The flattening of predictions at later horizons reflects a rational strategy:

> _When uncertainty grows, predict the safest continuation of the recent past._

For autoscaling, this behavior can be desirable:

- Prevents overreaction to transient spikes
    
- Encourages stability over jitter
    

This highlights an important tradeoff between **responsiveness** and **stability**.

---

## 8. What Day 3 Proved

By the end of Day 3, we verified that:

1. Multi-step forecasting is correctly framed
    
2. The model generalizes across future horizons
    
3. Error grows predictably with time
    
4. The system behaves according to theory
    
5. Failures (spikes) are explainable, not mysterious
    

This validates the forecasting backbone of the autoscaling system.

---

## 9. What Day 3 Did _Not_ Attempt

- No recursive prediction
    
- No teacher forcing
    
- No attention mechanisms
    
- No transformers
    
- No horizon-weighted losses
    

These were intentionally excluded to preserve interpretability.

---

## 🚀 Next Directions

With multi-step forecasting validated, the project can now move toward:

- Improving horizon-aware accuracy
    
- Comparing direct vs recursive forecasting
    
- Translating forecasts into autoscaling decisions
    

Day 3 completes the **forecasting foundation**.

Everything beyond this point builds on verified ground.


```output
    python train.py
Epoch 01 | Train MSE: 0.0176 | Val MSE: 0.0066
Epoch 02 | Train MSE: 0.0060 | Val MSE: 0.0048
Epoch 03 | Train MSE: 0.0053 | Val MSE: 0.0045
Epoch 04 | Train MSE: 0.0051 | Val MSE: 0.0045
Epoch 05 | Train MSE: 0.0049 | Val MSE: 0.0044
Epoch 06 | Train MSE: 0.0048 | Val MSE: 0.0044
Epoch 07 | Train MSE: 0.0047 | Val MSE: 0.0041
Epoch 08 | Train MSE: 0.0046 | Val MSE: 0.0040
Epoch 09 | Train MSE: 0.0045 | Val MSE: 0.0041
Epoch 10 | Train MSE: 0.0045 | Val MSE: 0.0039
Epoch 11 | Train MSE: 0.0045 | Val MSE: 0.0040
Epoch 12 | Train MSE: 0.0044 | Val MSE: 0.0039
Epoch 13 | Train MSE: 0.0044 | Val MSE: 0.0039
Epoch 14 | Train MSE: 0.0043 | Val MSE: 0.0040
Epoch 15 | Train MSE: 0.0043 | Val MSE: 0.0039
Overall MSE: 0.0038629164919257164
t+1: MSE = 0.0017
t+2: MSE = 0.0021
t+3: MSE = 0.0025
t+4: MSE = 0.0030
t+5: MSE = 0.0033
t+6: MSE = 0.0037
t+7: MSE = 0.0041
t+8: MSE = 0.0045
t+9: MSE = 0.0048
t+10: MSE = 0.0052
t+11: MSE = 0.0056
t+12: MSE = 0.0059
```

![[Pasted image 20251222130922.png]]