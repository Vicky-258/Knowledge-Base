# 🟧 WEEK 3 — Deep Forecasting (Informer-lite)

**Theme:** Stop reading. Start building.  
**Outcome:** A working LSTM-based workload predictor that forecasts the next 12 steps.

---

## 🟧 Day 1 — Data Pipeline & Framing the Problem

### 🎯 Goal:
Turn raw metrics into a supervised learning problem.

### ✅ Tasks:
- [x] Choose **one metric** as workload signal (CPU or RPS)
- [x] Load historical data (from Week 2 scrape)
- [x] Normalize the series (MinMax / StandardScaler)
- [x] Decide:
  - [x] input window size (e.g. 24 or 60)
  - [x] prediction horizon = 12
- [x] Implement sliding window dataset:
  - X → past `window_size`
  - y → next `12` values
- [x] Plot:
  - raw series
  - one sample window → target

📌 **Checkpoint:**  
You can clearly explain:  
“Given the past ___ points, I predict the next 12.”

---

## 🟧 Day 2 — LSTM Model (Single-Step First)

### 🎯 Goal:
Get a **minimal LSTM** to learn *anything*.

### ✅ Tasks:
- [x] Implement basic LSTM model:
  - input → LSTM → Linear
- [x] Start with **single-step prediction**
- [x] Choose loss:
  - [x] MSE
- [x] Train for a few epochs
- [x] Plot:
  - predicted vs actual (1-step)

📌 **Rule:**  
No fancy tricks. If this doesn’t learn, stop and debug.

📌 **Checkpoint:**  
Loss goes down. Predictions are not random noise.

---

## 🟧 Day 3 — Multi-Step Forecasting (Next 12 Steps)

### 🎯 Goal:
Predict **12 future steps at once**.

### ✅ Tasks:
- [x] Modify model output → size 12
- [x] Update dataset target accordingly
- [x] Train multi-step LSTM
- [x] Plot:
  - true future vs predicted future (12-step curve)
- [x] Measure:
  - [x] MSE per horizon
  - [x] overall MSE

📌 **Checkpoint:**  
You can visually see trend continuation (even if imperfect).

---

## 🟧 Day 4 — GRU Variant (Comparison Day)

### 🎯 Goal:
Understand *why* GRU sometimes works better.

### ✅ Tasks:
- [x] Replace LSTM with GRU
- [x] Keep everything else identical
- [x] Train GRU model
- [x] Compare:
  - [x] convergence speed
  - [x] stability
  - [x] prediction smoothness
- [x] Write short notes:
  - when GRU felt better
  - when LSTM felt better

📌 **Checkpoint:**  
You understand this is a **design choice**, not dogma.

---

## 🟧 Day 5 — Transformer Encoder (Minimal, Not Fancy)

### 🎯 Goal:
Touch transformers without drowning.

### ✅ Tasks:
- [x] Implement:
  - positional encoding
  - transformer encoder block
- [x] Input shape: (sequence_length, features)
- [x] Output → next 12 steps
- [x] Train on same dataset
- [x] Plot predictions

📌 **Rule:**  
No Informer. No attention hacks.  
Just encoder → linear head.

📌 **Checkpoint:**  
You *understand* what attention is doing, even if performance is similar.

---

## 🟧 Day 6 — Evaluation & Failure Analysis

### 🎯 Goal:
Stop celebrating. Start judging.

### ✅ Tasks:
- [x] Compare:
  - AR (Week 1)
  - LSTM
  - GRU
  - Transformer
- [x] Metrics:
  - [x] MSE
  - [x] MAE
- [x] Test on:
  - smooth workload
  - bursty workload
- [x] Identify:
  - where each model fails
  - lag vs overshoot behavior

📌 **Checkpoint:**  
You can justify *why* one model is chosen for Autoscale.

---

## 🟧 Day 7 — Freeze the Predictor (Engineering Day)

### 🎯 Goal:
Prepare this for integration later.

### ✅ Tasks:
- [ ] Choose **one final predictor**
- [ ] Wrap it as a clean Python module:
  - `fit()`
  - `predict_next_12()`
- [ ] Save / load model weights
- [ ] Add inference-only script
- [ ] Write a short `week3_notes.md`:
  - design choices
  - lessons learned
  - mistakes made

📌 **End-of-Week Win:**  
You now have a **production-shaped workload forecaster**.

---

## 🎉 WEEK 3 COMPLETE

You are officially past:
- toy ML
- notebook-only experiments

Next week = **burst detection**, where prediction meets reality.
