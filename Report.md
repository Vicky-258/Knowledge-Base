# Project Report: Human Following Bot

## Project Overview
This project involves the design, development, and deployment of a generic **Human Following Bot**. The robot utilizes a **Jetson Orin Nano** as the central processing unit, an **RPLIDAR** as the primary sensor, and an **Arduino** with an **L298N Motor Driver** for actuation. The software stack is built on **ROS 2 Humble**.

---

## 1. Hardware Architecture

-   **Computing Unit**: NVIDIA Jetson Orin Nano (Ubuntu 22.04, ROS 2 Humble).
-   **Perception**: RPLIDAR A1/A2 (360° Laser Scanner).
-   **Actuation**: Arduino Uno/Nano communicating via Serial (USB) to the Jetson.
-   **Power**: L298N Motor Driver + 11.1V/12V LiPo Battery.
-   **Wiring**: 
    - Lidar -> Jetson USB
    - Arduino -> Jetson USB
    - Motors -> L298N -> Arduino IO

---

## 2. Software Implementation Details

The core logic is distributed across three primary ROS 2 modules (Nodes). Below is the detailed breakdown of their implementation.

### Module 1: Perception (Lidar Target Tracker)
**Package**: `lidar_target_tracker`
**File**: `src/human_following/lidar_target_tracker/lidar_target_tracker/node.py`

#### What it does:
This module acts as the "eyes" of the robot. It takes raw laser data, removes noise, groups points into clusters (potential objects), tracking them over time, and identifying which one is the human target.

#### How it is implemented:
1.  **Input**: Subscribes to `/scan` (sensor_msgs/LaserScan).
2.  **Preprocessing**: 
    -   Iterates through laser ranges.
    -   Converts Polar coordinates ($r, \theta$) to Cartesian coordinates ($x, y$) for geometry calculations.
3.  **Clustering (`clustering.py`)**: 
    -   Groups adjacent points together based on Euclidean distance. If points are close to each other, they form a "cluster".
4.  **Tracking (`tracking.py`)**: 
    -   Maintains a memory of `previous_tracks`.
    -   Uses a **Nearest Neighbor** algorithm to match current clusters with previous ones.
    -   Calculates velocity vectors ($v_x, v_y$) for each object to predict movement.
5.  **Scoring & Selection**: 
    -   Assigns a score to each track (Confidence).
    -   Selects the track with the highest score as the "Target".
6.  **Smoothing**: 
    -   Applies **Exponential Smoothing** to the target's Angle and Distance to prevent jittery robot movements.
7.  **Output**: Publishes to `/target` (geometry_msgs/Point).
    -   `x` = Target Angle (radians)
    -   `y` = Target Distance (meters)
    -   `z` = Confidence Score (0.0 - 1.0)

---

### Module 2: Decision Making (Fusion Tracking)
**Package**: `fusion_tracking`
**File**: `src/fusion_tracking/fusion_tracking/decision_node.py`

#### What it does:
This module acts as the "brain". It makes high-level decisions based on the target's position and safety signals. It determines *how* the robot should move (Speed and Turn rate).

#### How it is implemented:
1.  **Input**:
    -   `/target` (from Tracker).
    -   `/safety/stop_gesture` & `/safety/obstacle_flag` (Boolean safety flags).
2.  **State Machine**:
    -   **STOP**: If any safety flag is `True`, force velocity to 0.
    -   **SEARCH**: If Target moving confidence is 0, spin slowly (`angular.z = 0.3`) to find the person.
    -   **FOLLOW**: If Target detected, use a **P-Controller** (Proportional Controller).
3.  **Control Logic (P-Controller)**:
    -   **Linear Velocity**: Proportional to distance error.
        -   `error = current_distance - TARGET_DISTANCE (1.0m)`
        -   `cmd_linear = error * KP_LINEAR` (Moves forward if far, stops if close).
    -   **Angular Velocity**: Proportional to angle error.
        -   `cmd_angular = current_angle * KP_ANGULAR` (Turns to center the target).
4.  **Output**: Publishes to `/cmd_vel` (geometry_msgs/Twist).

---

### Module 3: Actuation (Motor Controller)
**Package**: `actuation_controller`
**File**: `src/actuation_controller/actuation_controller/motor_controller.py`

#### What it does:
This module acts as the "muscle interface". It translates abstract velocity commands (e.g., "Move forward at 0.5 m/s") into specific hardware signals for the left and right motors.

#### How it is implemented:
1.  **Input**: Subscribes to `/cmd_vel` (Twist).
2.  **Kinematics (Differential Drive)**:
    -   Converts Linear ($v$) and Angular ($w$) velocity into wheel speeds.
    -   `Left_Wheel = v - (w * Width / 2)`
    -   `Right_Wheel = v + (w * Width / 2)`
3.  **Normalization**: 
    -   Clamps values between `-1.0` (Full Reverse) and `1.0` (Full Forward) to match the Arduino's expected input range.
4.  **Communication**:
    -   Connects to Arduino via Serial Port (`/dev/ttyUSB*` or `/dev/ttyACM*` at 115200 baud).
    -   Sends a formatted string: `"L:0.5,R:0.8\n"`.
    -   The Arduino firmware receives this string, parses it, and writes PWM signals to the L298N driver.

---

## 3. Automation Scripts

To ensure easy deployment, the following standard scripts were created:
1.  **`jetson_setup.sh`**: Automates the installation of ROS 2, dependencies, and permissions.
2.  **`wsl_run_commands.txt`**: Provides the exact 4-terminal launch sequence for running the simulation or robot.

