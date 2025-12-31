#  MODULE GUIDE — MEMBER 2

## LiDAR Sensing, Safety & STOP Gesture Module

---

##  Module Purpose (Read Carefully)

Your responsibility is **robot safety**.

Your module answers only one question:

> **“Is it safe for the robot to move right now?”**

You do **not** decide _who_ to follow  
You do **not** decide _how_ to move

You only **prevent unsafe motion**.

If your module fails, **the robot must stop**.

---

##  What You Are Responsible For 

###  LiDAR Setup & Data Access

- Install and configure **RPLIDAR A2M12**
    
- Ensure stable scan rate
    
- Visualize scans in RViz
    
- Filter noisy / invalid readings
    

---

### Obstacle Detection (Primary Safety)

Using LiDAR data, detect:

- obstacles in front of robot
    
- obstacles within unsafe distance
    

You must detect **static and dynamic obstacles**.

---

###  STOP Gesture / Emergency Stop (Important)

You are responsible for implementing a **LiDAR-based STOP gesture**, such as:

- sudden close object in front
    
- hand waved close to LiDAR
    
- rapid distance change within short time window
    

This is treated as an **emergency safety override**, not human detection.

---

##  What You Must NOT Do ❌ (Non-Negotiable)

- ❌ Do NOT track people
    
- ❌ Do NOT choose follow target
    
- ❌ Do NOT send motor commands
    
- ❌ Do NOT smooth motion
    
- ❌ Do NOT implement state machines
    

If the robot stops too often → acceptable  
If the robot fails to stop → **unacceptable**

---

##  Input / Output Contract (Strict)

###  Inputs

- Raw LiDAR scan data
    

Nothing else.

---

###  Outputs (ONLY these)

```text
obstacle_flag   = TRUE / FALSE
stop_gesture    = TRUE / FALSE
min_front_dist  = float (meters)
```

Interpretation:

- `obstacle_flag = TRUE` → robot must not move
    
- `stop_gesture = TRUE` → immediate stop, override everything
    
- `min_front_dist` → for logging & debugging only
    

You do **not** output velocity.

---

##  Safety Logic You Must Implement

###  Obstacle Detection Rules

- Define a **front sector** (e.g. ±30°)
    
- Compute minimum distance in that sector
    
- If distance < safety_threshold → obstacle_flag = TRUE
    

Threshold must be conservative.

---

###  STOP Gesture Logic (LiDAR-Only)

Examples of acceptable logic:

- Object appears suddenly within very close range
    
- Fast distance change within short time
    
- Repeated close-range motion pattern
    

You are **not detecting a human**.  
You are detecting a **deliberate interruption signal**.

Explain it as:

> “Intentional close-range motion triggers STOP.”

---

##  Behavioral Guarantees You Must Provide

Your module **must guarantee**:

- No false negatives for obstacles
    
- STOP gesture overrides all motion
    
- STOP persists until cleared
    
- Works even if tracking fails
    
- Does not depend on lighting or vision
    

Fail-safe > accuracy.

---

##  Tools & Technologies

- RPLIDAR A2M12
    
- ROS2
    
- rplidar_ros
    
- Python / C++
    
- RViz
    

---

##  Deliverables (Required)

###  Code

- LiDAR ROS node
    
- Obstacle detection logic
    
- STOP gesture detection logic
    
- Clear comments
    

---

### Documentation

- LiDAR sector diagram
    
- Safety threshold values
    
- STOP gesture explanation
    

---

###  Test Results

- Obstacle test (wall, chair)
    
- Sudden close-object STOP test
    
- Recovery after STOP
    

---

##  Testing Checklist (Must Pass)

-  Robot stops before hitting obstacle
    
-  STOP gesture halts robot immediately
    
-  STOP overrides follow logic
    
-  Removing obstacle clears flag
    
-  No dependency on other modules
    

If these pass → **integration-ready**.

---

##  How Your Module Fits the System

```
LiDAR → YOUR MODULE → Safety Flags → Decision Module
```

Your signals have **highest priority**.

If you say STOP —  
the robot **must stop**, no discussion.

---

##  Final Note (Important)

Your module is **not judged by intelligence**.

It is judged by one thing only:

> **“Did the robot ever move when it shouldn’t have?”**

If the answer is **no**, you did your job perfectly.
