#  MODULE GUIDE — MEMBER 3

## LiDAR-Based Human-Like Target Tracking Module

---

## Module Purpose (Read This Carefully)

Your responsibility is to **track a single moving target** using **LiDAR data only**.

You are **NOT detecting humans**.  
You are **tracking the most probable human-like moving object** based on motion and geometry.

Your module answers only one question:

> **“Where is the target relative to the robot right now?”**

Nothing more.

---

## What You Are Responsible For 

###  LiDAR Data Processing

- Subscribe to LiDAR scan data
    
- Filter out:
    
    - invalid points
        
    - points beyond max follow range
        
- Convert polar → Cartesian coordinates
    

---

### Object Clustering

- Group nearby points into clusters
    
- Each cluster represents a physical object
    
- Reject noise clusters (too small)
    

---

###  Motion-Based Target Identification

- Track cluster centroids across frames
    
- Identify **moving clusters**
    
- Apply **human-like motion heuristics**
    

This is the core intelligence of your module.

---

## What You Must NOT Do ❌ (Non-Negotiable)

- ❌ Do NOT control motors
    
- ❌ Do NOT implement obstacle avoidance
    
- ❌ Do NOT implement STOP gesture
    
- ❌ Do NOT implement state machines
    
- ❌ Do NOT decide when to follow or stop
    

You only provide **tracking data**.

---

##  Input / Output Contract (Strict)

### Inputs

- Filtered LiDAR scan data (from ROS)
    

Nothing else.

---

### Outputs (ONLY these)

```text
target_angle      (θ in radians)
target_distance   (r in meters)
target_confidence (0.0 – 1.0)
```

Rules:

- If no valid target → `target_confidence = 0`
    
- Output must be **stable** (no sudden jumps)
    

---

##  Human-Like Motion Heuristics (How You Score Targets)

You **do not classify humans**.  
You **score clusters**.

Use a weighted score based on:

### Size Constraint

- Reject clusters too small (noise)
    
- Reject clusters too large (walls)
    

---

### Velocity Constraint

- Prefer clusters moving at walking speed
    
- Reject very fast or static objects
    

---

### Motion Smoothness

- Human motion is smooth, not jerky
    
- Penalize sudden direction changes
    

---

###  Temporal Consistency

- Prefer previously tracked cluster
    
- Avoid switching targets unnecessarily
    

Final score = weighted combination.

---

##  Target Selection Logic

From all clusters:

1. Compute human-likeness score
    
2. Select highest scoring cluster
    
3. Maintain lock until confidence drops
    

This prevents oscillation.

---

##  Tracking Stability (Very Important)

Your outputs must be:

- smooth
    
- predictable
    
- temporally consistent
    

Techniques allowed:

- Exponential smoothing
    
- Kalman filter (optional)
    

Raw LiDAR noise must **not** leak to integration.

---

##  Tools & Technologies

- ROS2
    
- Python / C++
    
- Geometry math
    
- Clustering logic
    
- Kalman filter (optional)
    

No machine learning required.

---

##  Deliverables (Required)

###  Code

- LiDAR clustering node
    
- Tracking logic
    
- Scoring & selection logic
    
- Well-commented code
    

---

###  Documentation

- Cluster visualization
    
- Explanation of heuristics
    
- Parameter values & tuning notes
    

---

###  Test Results

- Single moving person test
    
- Multiple moving objects test
    
- Target loss & reacquisition test
    

---

## Testing Checklist (Must Pass)

-  Stable tracking when one person walks
    
-  No rapid target switching
    
-  Target lost → confidence drops to 0
    
-  No dependency on motor or safety modules
    
-  Output rate is consistent
    

If these pass → **integration-ready**.

---

##  How Your Module Fits the System

```
LiDAR → YOUR MODULE → (θ, r, confidence) → Decision Module
```

You provide **where**, not **what**.

---

##  Final Note (Read This)

Your module is **not judged** by accuracy alone.

It is judged by:

- stability
    
- clarity
    
- explainability
    

If your outputs are clean,  
the entire robot becomes easy to control.

That’s why this module is critical.
