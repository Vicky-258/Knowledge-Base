# 🟦 **WEEK 1 — Foundations: Time-Series + Kubernetes Basics**

### 🎯 Goals:

- Understand workload patterns
    
- Understand how autoscaling _actually_ works in K8s
    
- Build a tiny forecasting script
    

### 📚 Learn:

**ML side:**

- ~~Time-series basics (trend, seasonality)~~
    
- ~~Sliding windows~~
    
- ~~AR / ARIMA~~
    

**Cloud side:**

- ~~Pods, Deployments
    
- ~~ReplicaSets
    
- ~~Basic `kubectl`
    
- ~~CPU & memory requests/limits
    

### 🛠️ Mini-Project:

**Take Wikipedia hourly data → build AR model → predict next hour.**  
Just 20–30 lines of Python.

---

# 🟩 **WEEK 2 — Monitoring: Prometheus, cAdvisor, Metrics**

### 🎯 Goals:

- Learn how to _collect_ real metrics
    
- Learn scraping + PromQL
    
- Understand latency, CPU usage, RPS
    

### 📚 Learn:

- Prometheus fundamentals
    
- cAdvisor metrics
    
- Pod CPU/Memory over time
    
- Request rate / latency collection
    

### 🛠️ Mini-Project:

**Deploy a tiny Python API → Scrape metrics → Plot CPU & latency over time.**

You JUST created your own mini-Grafana.

---

# 🟧 **WEEK 3 — Forecasting Like BAScaler (Informer-lite)**

### 🎯 Goals:

- Move from linear → deep forecasting
    
- Learn LSTM/GRU
    
- Basic Transformer encoder for sequences
    

### 📚 Learn:

- LSTM forecasting
    
- GRU forecasting
    
- Transformers for time-series
    

### 🛠️ Mini-Project:

**Build LSTM-based workload predictor → Predict next 12 steps.**

Informer is overkill for now — this is perfect.

---

# 🟥 **WEEK 4 — Burst Detection System**

### 🎯 Goals:

- Detect when workload “breaks prediction interval”
    
- Implement the logic from Table 1 / Fig. 2 of the paper  
    (Yes, you’ll be recreating their detection logic.)
    

### 📚 Learn:

- Prediction intervals
    
- Quantile regression (simple versions)
    
- Error metrics
    
- Bootstrap sampling
    
- Residual calculations
    

### 🛠️ Mini-Project:

**Build a burst detector that prints:**

- “normal”
    
- “burst”
    
- “periodic spike detected but NOT burst”
    

This is the heart of BAScaler.

---

# 🟪 **WEEK 5 — Resource Modeling (SVR)**

### 🎯 Goals:

- Learn the performance model part
    
- Train SVR to predict response-time from:
    
    - workload
        
    - #instances
        

### 📚 Learn:

- Support Vector Regression
    
- Kernel tricks
    
- RBF kernel
    
- Hyperparameter tuning
    

### 🛠️ Mini-Project:

**Use k6 to load-test a small API → collect data → train SVR → predict latency.**

BOOM.  
You now have your own latency predictor.

---

# 🟨 **WEEK 6 — Build the First End-to-End Autoscaler (No RL Yet)**

### 🎯 Goals:

- Connect predictor → burst detector → SVR → scaling decision
    
- Trigger K8s API to scale up/down
    

### 📚 Learn:

- Python Kubernetes API
    
- Scaling pods programmatically
    
- Basic scheduling loops
    

### 🛠️ Mini-Project:

**Build your first autoscaler:**

- Every 1 minute:
    
    - fetch metrics
        
    - predict workload
        
    - detect burst or non-burst
        
    - estimate pods needed
        
    - scale K8s
        

This is your MVP.

---

# 🟫 **WEEK 7 — Reinforcement Learning (PPO-lite)**

### 🎯 Goals:

- Learn how RL fine-tunes scaling decisions
    
- Implement simplified PPO actions
    

### 📚 Learn:

- MDP basics
    
- Reward design
    
- PPO high-level logic
    
- Stable Baselines API (to simplify)
    

### 🛠️ Mini-Project:

**Train a PPO agent to adjust +1 or –1 pods based on:**

- current latency
    
- current CPU
    
- predicted pods
    
- workload trend
    

You now have the “Estimation Enhancer.”

---

# ⬛ **WEEK 8 — Full Integration + Stress Testing**

### 🎯 Goals:

- Build your final autoscaler
    
- Test with multiple load patterns
    
- Compare against HPA
    

### 📚 Learn:

- Kubernetes HPA behaviors
    
- Debugging autoscaling loops
    
- Logging
    
- Deployment best practices
    

### 🛠️ Mini-Project:

**Final Project:**  
🔥 **Your ML-driven Autoscaler**  
with these modules:

- historical predictor
    
- burst detector
    
- AR+bootstrap overestimator
    
- SVR latency model
    
- RL fine-tuner
    
- scaling executor (K8s)
    

Then run:

- bursty workload
    
- periodic workload
    
- chaotic workload
    

Measure:

- SLO violations
    
- cost (CPU)
    
- response time
    

You literally replicate the BAScaler experiment at a student level.