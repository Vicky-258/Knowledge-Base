# MODULE GUIDE — MEMBER 1

## Mechanical Setup & Motor Control Module

---

## Module Purpose (Read This First)

Your responsibility is to ensure **the robot moves exactly as commanded** —  
**no more, no less**.

You are **not deciding where the robot goes**.  
You are responsible for **executing movement commands accurately, safely, and predictably**.

Even if higher-level logic fails, **your module must behave deterministically**.

---

##  What You Are Responsible For 

###  Hardware & Mechanical Setup

- Assemble the robot chassis
    
- Mount motors securely and symmetrically
    
- Mount motor driver properly
    
- Ensure clean cable routing and strain relief
    
- Ensure wheels rotate freely without friction
    

---

###  Power & Wiring

- Connect motors to the motor driver
    
- Connect motor driver to **Arduino (motor controller)**
    
- Provide correct power to:
    
    - motors
        
    - motor driver
        
    - Arduino
        
    - Jetson (separately if required)
        
- Ensure **no loose or exposed connections**
    
- Add an emergency power cutoff (if available)
    

---

###  Motion Control (Core Task)

Implement **low-level motion primitives** on the **Arduino** only:

- Move forward
    
- Move backward
    
- Turn left
    
- Turn right
    
- Stop immediately
    

These are **execution primitives**, not behaviors or decisions.

---

##  What You Must NOT Do ❌ (Very Important)

- ❌ Do NOT read LiDAR data
    
- ❌ Do NOT decide direction or speed on your own
    
- ❌ Do NOT implement obstacle avoidance
    
- ❌ Do NOT add intelligence, logic, or state machines
    
- ❌ Do NOT modify other modules
    

If the robot crashes **because it followed an incorrect command**, that is **not your fault**.  
If it crashes **because your module failed to stop when commanded**, that **is your fault**.

---

##  Input / Output Contract (Non-Negotiable)

###  Inputs (from Jetson → Arduino)

You will receive **only these** commands over serial:

```text
linear_velocity   (float)
angular_velocity  (float)
```

Interpretation:

- `linear_velocity > 0` → move forward
    
- `linear_velocity = 0` → stop linear motion
    
- `angular_velocity > 0` → rotate left
    
- `angular_velocity < 0` → rotate right
    

Jetson decides **what to do**.  
Arduino decides **how to drive the motors**.

---

###  Outputs

- Physical robot movement only
    
- No sensor data or feedback required
    

---

##  Behavioral Guarantees You Must Provide

Your module **must guarantee**:

- Robot **stops immediately** when `linear_velocity = 0`
    
- Robot does **not drift** when stopped
    
- Robot rotates **in place** for angular commands
    
- Motor speeds respect **safe limits**
    
- Smooth start/stop without jerks
    

Reliability matters more than speed.

---

##  Tools & Technologies

- DC motors
    
- Motor driver (L298N / Cytron / equivalent)
    
- **Arduino (motor controller)**
    
- Serial communication (Jetson → Arduino)
    
- Arduino C/C++
    
- Multimeter (recommended)
    

Jetson **must not** directly drive motors.

---

##  Deliverables (Required)

### Hardware

- Fully assembled robot base
    
- Secure motor mounting
    
- Clean, labeled wiring
    

---

###  Code

- Arduino motor control firmware implementing:
    
    - `forward()`
        
    - `backward()`
        
    - `left()`
        
    - `right()`
        
    - `stop()`
        
- Serial command parsing
    
- Clearly commented code
    

---

###  Documentation

- Wiring diagram (hand-drawn is acceptable)
    
- Speed calibration table:
    
    - PWM value → observed movement behavior
        

---

##  Testing Checklist (Before Integration)

You must verify:

-  Robot moves straight when commanded forward
    
-  Left and right turns are symmetric
    
-  STOP command halts robot instantly
    
-  Robot does not move on startup
    
-  Power draw does not brown-out Jetson
    

If all checks pass → **integration-ready**.

---

##  How Your Module Fits Into the System

```
Decision Module (Jetson)
        ↓
Serial Commands (v, ω)
        ↓
Arduino Motor Controller (YOUR MODULE)
        ↓
Motor Driver → Motors
```

You are the **final execution layer** of the system.

---

##  Final Note (Read Carefully)

Your module is considered **correct** if:

> “The robot always obeys commands exactly and safely.”

This module is **not judged by intelligence**.

**Reliability > Speed**  
**Predictability > Cleverness**

That’s real robotics engineering.