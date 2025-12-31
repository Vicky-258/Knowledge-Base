#  MODULE GUIDE — MEMBER 4

## Decision Making, Following Logic & System Integration

---

##  Module Purpose (Your North Star)

You are responsible for **making the robot behave logically**.

Your module is the **only place** where:

- information comes together
    
- decisions are made
    
- priorities are enforced
    

You answer the question:

> **“Given everything the robot knows right now — what should it do?”**

If the robot behaves strangely, **this module is the first place to inspect**.

---

## What You Are Responsible For 

###  High-Level Decision Making

- Interpret outputs from:
    
    - LiDAR Safety Module (Member 2)
        
    - LiDAR Tracking Module (Member 3)
        
- Decide:
    
    - move / stop / search
        
    - speed
        
    - turning direction
        

You do **not** sense the world —  
you **reason about it**.

---

###  State Machine Design (Critical)

You must define and implement a **finite state machine**.

Minimum required states:

- `IDLE`
    
- `SEARCH`
    
- `FOLLOW`
    
- `STOP` (safety override)
    
- `LOST`
    

Each state must have:

- clear entry conditions
    
- clear exit conditions
    
- deterministic behavior
    

No ambiguity allowed.

---

###  Following Logic

Using `(θ, r, confidence)` from Member 3:

- Decide **when to move**
    
- Decide **how fast**
    
- Decide **how sharply to turn**
    

Rules must be:

- simple
    
- explainable
    
- tunable
    

---

###  Motion Smoothing

- Implement PID or proportional control
    
- Prevent jerky movement
    
- Ensure smooth start / stop transitions
    

Smoothness > aggressiveness.

---

###  System Integration

- Integrate all ROS nodes
    
- Create launch files
    
- Ensure startup order is correct
    
- Ensure failure in one module does not crash the system
    

You are the **integration gatekeeper**.

---

##  What You Must NOT Do ❌ (Protect Yourself)

- ❌ Do NOT re-implement LiDAR logic
    
- ❌ Do NOT change safety thresholds arbitrarily
    
- ❌ Do NOT bypass STOP signals
    
- ❌ Do NOT embed motor control logic
    
- ❌ Do NOT add sensors without agreement
    

Your power comes from **coordination**, not control creep.

---

##  Inputs You Will Receive

From **Member 2 (Safety)**:

```text
obstacle_flag  (TRUE / FALSE)
stop_gesture   (TRUE / FALSE)
```

From **Member 3 (Tracking)**:

```text
target_angle      (θ)
target_distance   (r)
target_confidence (0.0 – 1.0)
```

These are **trusted inputs**.

---

##  Outputs You Must Produce

To **Member 1 (Motors)**:

```text
linear_velocity   (float)
angular_velocity  (float)
```

Guarantees:

- STOP → both velocities = 0
    
- Smooth transitions only
    
- Obey speed limits
    

---

##  Priority Rules (Non-Negotiable)

Your logic must obey this order:

1. **STOP Gesture** → immediate halt
    
2. **Obstacle Flag** → halt or slow
    
3. **Target Confidence = 0** → SEARCH
    
4. **Valid Target** → FOLLOW
    

No exception.  
This priority list is your **safety shield** in reviews.

---

##  State Machine Logic (Reference)

Example (conceptual):

```
IF stop_gesture:
    STATE = STOP
ELSE IF obstacle_flag:
    STATE = STOP
ELSE IF target_confidence == 0:
    STATE = SEARCH
ELSE:
    STATE = FOLLOW
```

Each state defines:

- output velocities
    
- transition conditions
    

---

##  Behavioral Guarantees You Must Provide

Your module must guarantee:

- Robot never moves when STOP is active
    
- Robot does not oscillate between states
    
- Robot slows down near target
    
- Robot searches calmly when target is lost
    
- Robot behavior is predictable
    

If behavior is predictable, debugging is easy.

---

##  Tools & Technologies

- ROS2
    
- Python
    
- State machines
    
- PID / proportional control
    
- Logging & debugging tools
    

No ML required here.

---

##  Deliverables (Your Accountability)

###  Code

- Decision node
    
- State machine implementation
    
- Motion control logic
    
- Clean ROS launch files
    

---

###  Documentation

- State machine diagram
    
- Priority explanation
    
- Parameter table (thresholds, speeds)
    

---

###  Test Results

- Safety override test
    
- Target loss & recovery test
    
- Smooth following test
    
- Stress test (fast movement, stop/start)
    

---

## How Your Module Fits the System

```
Safety + Tracking → YOUR MODULE → Motor Commands
```

You are the **single decision authority**.

If modules disagree —  
**you decide**.

---

##  Final Note (Read This Slowly)

You are not “just integrating”.

You are:

- defining system behavior
    
- enforcing safety
    
- translating perception into action
    

If this project feels coherent in the demo,  
it will be **because of this module**.