# Human-Following Robot — System Design Document

## 1. Objective

Design and implement a **robust human-following robot** using edge-AI hardware. The robot should be able to:

- Detect a human in front of it
    
- Estimate the human’s distance and direction
    
- Follow the human smoothly
    
- Stop or pause safely on command or unsafe conditions
    

The focus is on **system architecture, reliability, and explainability**, not just raw AI accuracy.

---

## 2. Selected Hardware

### 2.1 NVIDIA Jetson Orin Nano

**Role:** Central processing unit (Main Brain)

- Runs Linux
    
- Executes perception, tracking, and decision logic
    
- Handles sensor fusion and control flow
    

Why chosen:

- Sufficient GPU for real-time vision
    
- Stable SDK ecosystem
    
- Designed for edge AI robotics
    

---

### 2.2 Intel RealSense D435f

**Role:** Primary perception sensor (Eyes)

- Provides RGB frames
    
- Provides depth information per pixel
    

Why chosen:

- Combines vision + distance in one sensor
    
- Eliminates complex LiDAR–vision association
    
- Reliable indoors
    

---

### 2.3 Arduino Nicla Vision

**Role:** Auxiliary perception & safety controller (Reflex system)

- Runs lightweight vision or TinyML
    
- Performs fast, low-latency checks
    

Typical responsibilities:

- Hand gesture detection (STOP)
    
- Emergency proximity detection
    
- Redundant safety signaling
    

---

## 3. High-Level System Architecture

```
+-------------------+        +-------------------+
|  RealSense D435f  | -----> |                   |
|  (RGB + Depth)   |        |                   |
+-------------------+        |                   |
                             |  Jetson Orin Nano |
+-------------------+        |  (Decision Brain) |
|  Nicla Vision     | -----> |                   |
|  (Safety / Assist)|        |                   |
+-------------------+        +-------------------+
                                      |
                                      v
                           +-------------------+
                           | Motor Controller  |
                           | (PWM / Drivers)   |
                           +-------------------+
```

Jetson Orin Nano is the **single authority** for motion decisions.

---

## 4. Functional Responsibility Split

### 4.1 RealSense D435f — Perception Layer

Outputs:

- RGB frame (image)
    
- Depth frame (distance per pixel)
    

No processing or decision-making is done on the camera.

Used by Jetson to:

- Detect humans in the frame
    
- Compute distance to detected human
    
- Determine left/right alignment
    

---

### 4.2 Jetson Orin Nano — Decision Layer

Runs the following components:

#### a) Human Detection

- Algorithm options:
    
    - YOLO (preferred)
        
    - MediaPipe
        
    - HOG + SVM (fallback)
        

Output:

- Bounding box of human
    
- Confidence score
    

---

#### b) Distance & Direction Estimation

Method:

- Take bounding box center `(cx, cy)`
    
- Query depth at `(cx, cy)`
    

Derived values:

- `distance` → forward/back decision
    
- `offset_x` → left/right turning decision
    

---

#### c) Control Logic (State Machine)

States:

- `FOLLOW`
    
- `STOP`
    
- `SEARCH`
    
- `EMERGENCY_STOP`
    

Example logic:

```
IF emergency_signal:
    state = EMERGENCY_STOP
ELSE IF human_detected:
    state = FOLLOW
ELSE:
    state = SEARCH
```

---

### 4.3 Arduino Nicla Vision — Safety & Reflex Layer

Responsibilities:

- Detect STOP hand gesture
    
- Detect very close obstacle (optional)
    
- Send simple signals to Jetson
    

Example signals:

- `STOP`
    
- `CLEAR`
    

Nicla does **not** control motors directly.

---

## 5. Communication Interfaces

### 5.1 RealSense → Jetson

- Interface: USB
    
- Protocol: RealSense SDK / ROS driver
    

---

### 5.2 Nicla Vision → Jetson

- Interface: UART / USB
    
- Protocol: Simple text or byte messages
    

Example:

```
STOP\n
```

---

### 5.3 Jetson → Motor Controller

- Interface: Serial
    
- Commands:
    
    - Speed (PWM value)
        
    - Direction (left/right/forward)
        

---

## 6. Motion Control Strategy

Jetson computes:

- Linear velocity `v`
    
- Angular velocity `w`
    

Rules:

- If human too far → move forward
    
- If human too close → stop
    
- If human left → turn left
    
- If human right → turn right
    

Motor controller translates commands into PWM signals.

---

## 7. Safety & Fail-Safe Design

Safety mechanisms:

- Emergency STOP from Nicla Vision
    
- Loss of human detection → SEARCH mode
    
- Maximum speed limits
    
- Immediate motor halt on signal loss
    

This ensures:

- No uncontrolled motion
    
- Safe indoor operation
    

---

## 8. Demo Flow (Evaluation Ready)

1. Power on system
    
2. Robot initializes sensors
    
3. Human enters camera view
    
4. Robot aligns and follows
    
5. Human raises hand → robot stops
    
6. Human moves away → robot searches
    

---

## 9. Role Distribution (Team-Friendly)

- Vision & ML: Human detection pipeline
    
- Integration & Logic: Sensor fusion + state machine
    
- Hardware & Motors: Chassis, drivers, power
    
- Safety Module: Nicla Vision logic
    

Clear ownership without hierarchy conflict.

---

## 10. Key Design Justifications

- Depth camera chosen over LiDAR for simpler human distance estimation
    
- Centralized decision-making avoids conflicts
    
- Distributed perception improves safety
    
- Simple state machine improves reliability
    

---

## 11. Conclusion

This design prioritizes:

- Explainability
    
- Reliability
    
- Clean system boundaries
    

The result is a **stable, faculty-defensible human-following robot** built using real edge-AI principles rather than demo-only tricks.