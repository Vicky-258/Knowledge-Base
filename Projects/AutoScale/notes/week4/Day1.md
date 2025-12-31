

# Day 1 — Residuals & Error Anatomy

## 🎯 Goal

Understand **how and where** the GRU predictor is wrong, instead of blindly trusting aggregate metrics like MSE.

---

## 🔧 Setup Summary

- Model: **Frozen GRU predictor** (`gru_v1.pt`)
    
- Evaluation: **Standalone `val.py`**
    
- Dataset: Validation split (20%)
    
- Output shape:
    
    - Predictions: `(2008, 12)`
        
    - Actuals: `(2008, 12)`
        
- Residual definition:
    
    ```
    residual = actual − prediction
    ```
    

---

## 📊 Observations

### 1️⃣ Overall Residual Distribution

- Residuals are **strongly centered around 0**
    
- Distribution is mostly symmetric **with a slight right skew**
    
- A noticeable **right-tail bump (~0.4–0.5)** exists
    

**Interpretation:**

- The model is largely **unbiased**
    
- Right skew indicates **systematic underprediction during load spikes**
    
- Errors are not random — they follow a pattern
    

---

### 2️⃣ Horizon-wise Residual Behavior

#### 🟢 Short horizons (t+1 → t+4)

- Very tight distributions
    
- Low variance
    
- Near-perfect centering around zero
    

**Conclusion:**  
Highly reliable for immediate forecasting and reactive scaling.

---

#### 🟡 Mid horizons (t+5 → t+8)

- Gradual increase in variance
    
- Still centered around zero
    
- Occasional right-tail outliers
    

**Conclusion:**  
Predictions remain stable but uncertainty grows predictably.

---

#### 🟠 Long horizons (t+9 → t+12)

- Clear variance inflation
    
- Right-tail outliers become consistent
    
- No major mean drift (still roughly unbiased)
    

**Conclusion:**  
Model increasingly **underpredicts upcoming spikes**, but failure mode is consistent rather than chaotic.

---

## 📌 Key Insight (Most Important)

The model is **not randomly wrong**.

Instead, it is:

- Mostly accurate during normal operation
    
- **Conservatively biased during sudden load increases**
    
- Predictably worse at longer horizons
    

This is a **desirable failure mode** for autoscaling systems, since policy-level buffers can compensate for conservative forecasts.

---

## ✅ Day 1 Checkpoint Answer

> **“What magnitude of error is normal for each horizon?”**

- **t+1 to t+3:** Small, tightly bounded errors around zero
    
- **t+4 to t+8:** Moderate, predictable variance increase
    
- **t+9 to t+12:** Larger errors become common, mainly due to spike underprediction
    

---

## 🧭 Outcome

- Established a **trust profile per horizon**
    
- Identified **spike underprediction** as the dominant failure mode
    
- Confirmed the model is suitable for autoscaling **with guardrails**
    

---

🧠 **Bottom line:**  
This model doesn’t lie randomly — it lies _politely and predictably_.  
That’s exactly the kind of liar autoscaling systems can work with 