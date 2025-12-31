# 🟧 Day 5 — Transformer Encoder (Minimal)

## 🎯 Objective

Understand **what attention is doing** in a transformer encoder, without relying on performance gains or architectural tricks.

The goal was **conceptual clarity**, not outperforming RNNs.

---

## 🔁 Experiment Setup

- Implemented a **minimal Transformer Encoder**
    
- Components:
    
    - Input projection: `1 → d_model`
        
    - Sinusoidal positional encoding
        
    - Transformer encoder layers (self-attention + FFN)
        
    - Linear head → 12-step forecast
        
- Architecture:
    
    ```
    30 timesteps → Transformer Encoder → last timestep → Linear → 12 outputs
    ```
    
- No decoder
    
- No causal masking
    
- No attention hacks
    
- Same dataset, optimizer, loss, and training loop as GRU/LSTM
    

This ensured a **fair comparison**.

---

## 📉 Training Behavior

- Slower early convergence compared to GRU
    
- Stable training after a few epochs
    
- Training and validation losses stayed close
    
- No instability or divergence
    

**Interpretation:**  
Attention is more flexible but less biased than recurrence, so it needs more data or time to settle.

---

## 📊 Forecast Error by Horizon

- Horizon-wise MSE increases monotonically
    
- Near-term predictions are more accurate
    
- Long-term predictions accumulate uncertainty
    

Error growth pattern was **very similar to GRU**, indicating that architecture constraints dominate behavior.

---

## 🌊 Prediction Characteristics

- Predictions are **less smooth than GRU**
    
- Better alignment with mid-range trends
    
- Still underestimates sharp spikes and deep drops
    
- Extreme volatility is smoothed, but less aggressively than GRU
    

The transformer shows **greater responsiveness**, but remains conservative.

---

## 🧠 What Attention Is Doing

Unlike RNNs, attention:

- Does not process time sequentially
    
- Sees all timesteps simultaneously
    
- Learns to **reweight past timesteps dynamically**
    

Instead of carrying memory forward, the model asks:

> “Which past timesteps matter most _right now_?”

This is the key conceptual difference from recurrence.

---

## 🧩 Why Transformer Did Not Dramatically Outperform

Two intentional constraints limited attention’s full power:

1. **Final timestep bottleneck**
    
    ```
    Encoder output → last timestep only
    ```
    
    This forces aggressive summarization.
    
2. **No decoder or autoregressive refinement**  
    All 12 future steps are predicted from a single vector.
    

As a result, attention’s expressiveness is partially suppressed by design.

---

## ⚖️ Transformer vs GRU (Observed Tradeoff)

### ✅ Transformer felt better when:

- Capturing mid-range temporal dependencies
    
- Responding to direction changes
    
- Avoiding excessive mean-collapse
    

### ❌ Transformer felt limited when:

- Predicting rare, sharp spikes
    
- Preserving extreme volatility
    
- Constrained by a single-vector bottleneck
    

---

## 🧠 Key Takeaway

Attention does not “remember” like RNNs.

> It **selectively mixes time** instead of carrying it forward.

Performance similarity with GRU is not failure — it is evidence that:

- architecture choices matter more than model family
    
- attention must be allowed to express itself to shine
    


```output

    python train.py
Epoch 01 | Train MSE: 0.0172 | Val MSE: 0.0082
Epoch 02 | Train MSE: 0.0075 | Val MSE: 0.0053
Epoch 03 | Train MSE: 0.0059 | Val MSE: 0.0044
Epoch 04 | Train MSE: 0.0054 | Val MSE: 0.0043
Epoch 05 | Train MSE: 0.0052 | Val MSE: 0.0040
Epoch 06 | Train MSE: 0.0049 | Val MSE: 0.0038
Epoch 07 | Train MSE: 0.0048 | Val MSE: 0.0038
Epoch 08 | Train MSE: 0.0047 | Val MSE: 0.0041
Epoch 09 | Train MSE: 0.0046 | Val MSE: 0.0038
Epoch 10 | Train MSE: 0.0045 | Val MSE: 0.0041
Epoch 11 | Train MSE: 0.0045 | Val MSE: 0.0038
Epoch 12 | Train MSE: 0.0045 | Val MSE: 0.0036
Epoch 13 | Train MSE: 0.0044 | Val MSE: 0.0036
Epoch 14 | Train MSE: 0.0044 | Val MSE: 0.0038
Epoch 15 | Train MSE: 0.0044 | Val MSE: 0.0037
Overall MSE: 0.003672398393973708
t+1: MSE = 0.0014
t+2: MSE = 0.0018
t+3: MSE = 0.0022
t+4: MSE = 0.0026
t+5: MSE = 0.0031
t+6: MSE = 0.0036
t+7: MSE = 0.0039
t+8: MSE = 0.0044
t+9: MSE = 0.0048
t+10: MSE = 0.0051
t+11: MSE = 0.0055
t+12: MSE = 0.0058
```

![[Pasted image 20251222130618.png]]