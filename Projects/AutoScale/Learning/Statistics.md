
# Standard Deviation

## What:

How much the data wanders away from the average.  
Low SD = peaceful village  
High SD = everyone fighting for no reason

## Why:

To know the system’s _normal craziness level_.  
So we can spot when something is **weird enough** to be called a burst.

## When:

- Data doesn’t shift unpredictably every minute
    
- You want a cheap, fast volatility measure
    
- You want simple burst detection without ML
    

## How it's useful in burst detection:

- Use **mean + k·SD** as a dynamic threshold
    
- Use **z-scores** to detect abnormal points regardless of scale
    

## Where it sucks:

- Outliers make it blind
    
- If the system is naturally chaotic (large SD), bursts hide inside noise
    
- If data has strong seasonal patterns
    
- If distribution is skewed (SD assumes symmetry)
    

---

# **Moving Average**

## What:

- Average of the last **N points** sliding over the dataset.
    
- **Simple MA** → every point equal.
    
- **Weighted MA** → some points matter more (usually the recent ones).
    
- **Exponential MA** → recent points matter **a lot** more.
    

---

## Why:

- Smooths out tiny random bumps that don’t matter.
    
- Gives a **clean baseline** for burst comparison.
    
- Helps see the **actual trend** hiding inside noisy raw data.
    

---

## When:

- Your data is noisy.
    
- You want to remove tiny fluctuations.
    
- The signal changes slowly over time.
    
- You want a cheap, fast smoothing method.
    

---

## How it's useful:

- Acts as the **local baseline** for burst detection.
    
- Removes noise so we don’t panic on every mini spike.
    
- Works beautifully with **rolling SD** to detect abnormal highs.
    

---

## Why it sucks:

- **Detects bursts late** (lag problem).
    
- **Short sudden bursts get absorbed** and may disappear.
    
- Only as good as your window size (too small = noisy, too big = blind).
    
- Has **no idea about seasonality** (night traffic looks like a daily crisis).
    
- Can't properly follow fast trends — it always lags behind.

