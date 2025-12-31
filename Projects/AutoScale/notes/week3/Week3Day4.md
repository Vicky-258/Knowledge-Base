# 🟧 Day 4 — GRU Variant (Comparison Day)

## 🎯 Objective

Understand **why GRU sometimes works better than LSTM**, and when it does not.  
Treat the choice as an **engineering tradeoff**, not a rule.

---

## 🔁 Experiment Setup

- Replaced **LSTM** with **GRU**
    
- Kept everything else identical:
    
    - input sequence length = 30
        
    - prediction horizon = 12
        
    - hidden size = 32
        
    - optimizer = Adam
        
    - learning rate = 0.001
        
    - loss = MSE
        
- Architecture:
    
    ```
    30 timesteps → GRU → last hidden state → Linear → 12 outputs
    ```
    

This ensured a **fair, controlled comparison**.

---

## 📉 Training Behavior

- GRU converged **quickly** (major loss drop in early epochs)
    
- Training and validation MSE stayed **very close**
    
- No instability or oscillations
    
- Minimal tuning required
    

**Interpretation:**  
GRU’s simpler gating makes optimization easier and more stable.

---

## 📊 Forecast Error by Horizon

- Error increases monotonically with prediction horizon
    
- Near-term predictions (t+1 to t+3) are significantly more accurate
    
- Long-term predictions (t+10 to t+12) accumulate uncertainty
    

This is **expected behavior** in multi-step forecasting.

---

## 🌊 Prediction Characteristics

- GRU predictions are **smooth and conservative**
    
- Sharp spikes in the ground truth are **under-predicted**
    
- Deep drops are **smoothed out**
    
- Model collapses toward an average future trajectory
    

**Interpretation:**  
GRU prefers _expected behavior_ over _rare events_.

---

## 🧠 Why GRU Behaves This Way

- GRU uses a **single blended memory state**
    
- Aggressive state updates cause faster forgetting
    
- No separate long-term cell state
    
- Limited ability to preserve rare or abrupt patterns
    

This inductive bias encourages:

- stability
    
- trend-following
    
- smoothing
    

---

## ⚖️ GRU vs LSTM (Observed Tradeoff)

### ✅ GRU felt better when:

- Fast convergence mattered
    
- Training stability was important
    
- Short-term trends were dominant
    
- Smooth predictions were acceptable
    

### ❌ GRU felt worse when:

- Sharp spikes mattered
    
- Long-range precision was required
    
- Rare events carried high importance
    

---

## 🧩 Architectural Insight

The model compresses:

```
30 timesteps → 1 vector → 12 predictions
```

This bottleneck:

- favors GRU’s simplicity
    
- limits LSTM’s expressive memory advantage
    
- encourages smoothing regardless of cell type
    

Architecture choice can matter **more than** recurrent cell choice.

---

## ✅ Key Takeaway

GRU is **not better or worse** than LSTM.

> GRU works best when the task rewards  
> **simplicity, stability, and trend-following**.

> LSTM is preferable when the task demands  
> **long-term memory and sharp temporal distinctions**.

This is a **design decision**, not dogma.


```output
Epoch 01 | Train MSE: 0.0143 | Val MSE: 0.0061
Epoch 02 | Train MSE: 0.0064 | Val MSE: 0.0056
Epoch 03 | Train MSE: 0.0061 | Val MSE: 0.0053
Epoch 04 | Train MSE: 0.0058 | Val MSE: 0.0047
Epoch 05 | Train MSE: 0.0049 | Val MSE: 0.0041
Epoch 06 | Train MSE: 0.0047 | Val MSE: 0.0042
Epoch 07 | Train MSE: 0.0047 | Val MSE: 0.0043
Epoch 08 | Train MSE: 0.0046 | Val MSE: 0.0043
Epoch 09 | Train MSE: 0.0046 | Val MSE: 0.0041
Epoch 10 | Train MSE: 0.0045 | Val MSE: 0.0039
Epoch 11 | Train MSE: 0.0045 | Val MSE: 0.0039
Epoch 12 | Train MSE: 0.0045 | Val MSE: 0.0039
Epoch 13 | Train MSE: 0.0044 | Val MSE: 0.0039
Epoch 14 | Train MSE: 0.0044 | Val MSE: 0.0038
Epoch 15 | Train MSE: 0.0043 | Val MSE: 0.0038
Overall MSE: 0.0038267553318291903
t+1: MSE = 0.0017
t+2: MSE = 0.0021
t+3: MSE = 0.0025
t+4: MSE = 0.0029
t+5: MSE = 0.0033
t+6: MSE = 0.0036
t+7: MSE = 0.0040
t+8: MSE = 0.0044
t+9: MSE = 0.0048
t+10: MSE = 0.0051
t+11: MSE = 0.0056
t+12: MSE = 0.0059
⏎                      
```

![[Pasted image 20251222130743.png]]