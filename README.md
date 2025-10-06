#  Vision-Based Dynamic Landing on a Moving Platform (Parrot Mambo Drone)

This project demonstrates a **vision-guided autonomous landing system** developed in **MATLAB/Simulink** for the **Parrot Mambo drone**.  
The drone identifies a **moving RGB landing platform** (mounted on a line-following robot) and performs a **controlled descent and landing** using **computer-vision processing**, **state-machine logic**, and a **Simulink-based flight control system**.

---

## Objective
Unlike a normal stationary landing, this experiment focuses on **dynamic landing** — where both the drone and the landing platform are in motion.  
The goal was to detect the red landing pad in real time, maintain stability, and land smoothly using visual feedback without the need for external sensors or motion capture.

---

## System Architecture
![Main Block Diagram](Docs/images/block_diagram_main.png)

The complete system is divided into four major subsystems:

### 1. Flight Commands
- Provides high-level input commands (altitude, forward velocity, yaw rate) using a **Signal Builder** block.
- These commands mimic pilot inputs or autonomous mission scripts.

### 2️. Flight Control System
<img width="2048" height="981" alt="image" src="https://github.com/user-attachments/assets/3ef58e62-550a-4d36-bd86-30f66ded4bb4" />
- Contains the **Control System**, **Image Processing System**, and **Stateflow decision logic**.
- Receives:
  - Drone sensor data (altitude, orientation, velocities)
  - Image data from the downward-facing camera
- Outputs motor actuator commands that control the drone.
- Built for **auto code generation** using Embedded Coder (optional).

### 3️. Simulation Model
- Models the **drone physics**, **environment**, and **sensor dynamics**.
- Includes the **Nonlinear Airframe** block and **Environment models** for wind, gravity, and lighting.
- Allows realistic simulation of drone motion and sensor behavior before deploying to hardware.

### 4️. Flight Visualization
- Uses the **Simulink 3D Animation toolbox** to visualize drone movement and platform motion in real time.

---

##  How It Works

###  Step 1: Image Acquisition and Preprocessing
![Image Processing System](Docs/images/image_processing.png)
- The drone’s camera captures a YUV image stream.
- Converted to RGB using the Parrot Minidrone support package.
- A custom MATLAB Function block (`createMask`) isolates **red pixels** through color thresholding.
- Binary images (white = red region) are filtered to remove noise.
<img width="2048" height="985" alt="image" src="https://github.com/user-attachments/assets/6ef562ab-7d53-49b6-bd96-f706e456231f" />

###  Step 2: Platform Detection Logic
![Path Detection](Docs/images/path_detection.png)
- The binary image is divided into **9 regions** (Left/Centre/Right × Top/Mid/Bot).
- Pixel counts are computed in each region.
- A region is marked “active” if the pixel count ≥ 50.
- The **detectLandingPlatformLatched.m** script validates detection:
  - At least **350 white pixels** detected for **12 consecutive frames**
  - Centroid within ±30 px (X) and ±20 px (Y) from frame center
- Once confirmed, it raises `startDescendingFlag = true`.
<img width="2048" height="1013" alt="image" src="https://github.com/user-attachments/assets/bdaff3db-04b2-4896-bc32-691abef96618" />

###  Step 3: Stateflow Decision Logic
![Decision Chart](Docs/images/decision_chart.png)
- Core flight behavior is implemented as a **Stateflow chart** with states:
  - **Forward**
  - **TurnLeft**
  - **TurnRight**
  - **Backward**
  - **Land**
- Before platform detection → drone navigates following path cues.
- After detection → descent speed reduced (`z = –0.5`) for smooth touchdown.
- When `isLanded = true`, motor thrust = 0, ending simulation.
<img width="2047" height="1050" alt="image" src="https://github.com/user-attachments/assets/755c0d76-3a7d-4866-825c-2a6d6603b6da" />

###  Step 4: Simulation & Visualization
- Entire control loop runs in Simulink simulation.
- Flight trajectory and landing are visualized using **3D Animation viewer**.
- The final simulation video demonstrates the drone detecting the platform, aligning, and descending steadily onto it.

---

## Tools & Technologies
| Category | Tools / Libraries Used |
|-----------|------------------------|
| **Software Environment** | MATLAB R2023b / R2024a |
| **Toolboxes** | Simulink, Stateflow, Computer Vision Toolbox, Aerospace Blockset |
| **Hardware (Simulated)** | Parrot Mambo Drone |
| **Support Package** | MATLAB Support Package for Parrot Minidrone |
| **Programming Language** | MATLAB (scripts + function blocks) |
| **Modeling Concepts** | Dynamic system modeling, decision state machines, code generation |
| **Visualization** | Simulink 3D Animation Viewer |

---

##  Results
-  Reliable platform detection from various angles and distances  
-  Dynamic descent triggered after consistent detection  
-  Smooth, stable touchdown with minimal drift  
-  Average descent speed reduced by 50% during landing  
-  Video demonstration confirms accuracy and robustness





---

## 📦 Repository Structure
