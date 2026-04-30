# 🤖 AI-Powered Campus Food Delivery Robot

<div align="center">

![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-A22846?style=for-the-badge&logo=raspberry-pi&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-27338e?style=for-the-badge&logo=OpenCV&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)
![ROS](https://img.shields.io/badge/ROS-22314E?style=for-the-badge&logo=ros&logoColor=white)
![Status](https://img.shields.io/badge/Status-Prototype%20Complete-brightgreen?style=for-the-badge)

**An autonomous 4WD robotic platform engineered for last-mile food delivery on college, medical, and corporate campuses.**  
*Combining Convolutional Neural Networks, Reinforcement Learning, and multi-sensor fusion for fully autonomous navigation.*

[📄 View Presentation](#) · [🎥 Watch Demo](#) · [📬 Contact Team](#contact)

</div>

---

## 📸 Project Gallery

| Robot Platform | Sensor Array | Live Demo |
|:-:|:-:|:-:|
| ![Robot with Raspberry Pi](photos/nahid_3.jpeg) | ![Ultrasonic + Camera](photos/nahid_1.jpeg) | ![Operational Demo](photos/nahid_2.jpeg) |
| *Raspberry Pi 4 mounted on FreeNove 4WD Mecanum chassis* | *HC-SR04 ultrasonic sensor + Pi Camera on servo mount* | *Live GUI control & demonstration* |

---

## 🧠 Project Overview

This project proposes and prototypes an **AI-powered autonomous delivery robot** designed to navigate structured campus environments — colleges, hospitals, and corporate campuses — to deliver food directly to users on demand.

Users place an order via a mobile app or web portal. The robot autonomously navigates from the pickup location to the destination using a fusion of computer vision, sensor data, and machine learning algorithms — with no human operator required.

The working prototype was built on the **FreeNove 4WD Smart Car Kit for Raspberry Pi**, which served as the physical testbed for implementing and validating all sensor systems and autonomous navigation behaviors.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        INPUTS                                   │
│   📷 Camera (CNN)  │  📡 Ultrasonic  │  🔦 IR Sensors  │  🛰 GPS │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                      PROCESSING LAYER                           │
│                                                                 │
│  ┌─────────────────┐  ┌──────────────────┐  ┌───────────────┐  │
│  │  Perception     │  │  Localization    │  │  Path Planning│  │
│  │  (CNN)          │  │  & Mapping (RNN) │  │  & Decision   │  │
│  │                 │  │                  │  │  Making (RL)  │  │
│  │ • Object detect │  │ • Sensor fusion  │  │               │  │
│  │ • Segmentation  │  │ • Map building   │  │ • Route optim │  │
│  │ • Tracking      │  │ • GPS integrate  │  │ • Behavior    │  │
│  └─────────────────┘  └──────────────────┘  │   prediction  │  │
│                                             └───────────────┘  │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                         OUTPUTS                                 │
│  🚗 Motor Control  │  🗺 Map Building  │  📦 Delivery Actions   │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🤖 AI & Neural Network Implementation

### 1. Perception — Convolutional Neural Network (CNN)

The robot's vision system uses a **Pi Camera** paired with a CNN for real-time visual understanding of the environment.

| Capability | Description |
|---|---|
| **Object Detection & Tracking** | Identifies pedestrians, obstacles, vehicles, and landmarks in real-time |
| **Semantic Segmentation** | Distinguishes navigable surfaces — sidewalks vs. grass vs. roads vs. restricted zones |
| **Scene Understanding** | Recognizes building entrances, delivery points, and campus-specific landmarks |

### 2. Localization & Mapping — Recurrent Neural Network (RNN)

The RNN layer combines heterogeneous sensor inputs to build and update a persistent map of the campus.

- **Sensor Fusion**: Merges ultrasonic distance data, infrared line signals, camera frames, and GPS coordinates into a unified world model
- **Real-Time Mapping**: Continuously updates the map as the robot navigates, handling dynamic changes like parked vehicles or temporary obstacles
- **GPS Integration**: Uses GPS waypoints as high-level navigation anchors while local sensor data handles fine-grained path correction

### 3. Path Planning & Decision Making — Reinforcement Learning (RL)

The RL agent is trained to maximize delivery success rate while minimizing risk to pedestrians and property.

- **Reward Function**: Positive rewards for reaching delivery targets efficiently; negative rewards for collisions, blocked paths, or ADA violations
- **Behavior Prediction**: Anticipates pedestrian and cyclist movement trajectories to proactively adjust the robot's path
- **Dynamic Rerouting**: Responds in real-time to unexpected obstacles or blocked routes

---

## ⚙️ Hardware — FreeNove 4WD Smart Car Platform

The prototype is built on the **FreeNove FNK0043** platform running a **Raspberry Pi 4 Model B**.

### Platform Specifications

| Component | Specification |
|---|---|
| **Compute** | Raspberry Pi 4 Model B |
| **Chassis** | FreeNove 4WD with Mecanum wheels (omnidirectional movement) |
| **Drive System** | 4 independent DC motors |
| **Primary Sensor** | HC-SR04 Ultrasonic Sensor (servo-mounted, 180° sweep) |
| **Line Tracking** | 3-channel Infrared Tracking Sensor Array |
| **Vision** | Raspberry Pi Camera Module (servo-mounted pan/tilt) |
| **Light Sensing** | Dual photoresistor light-tracing array |
| **LED System** | 8x individually addressable RGB LEDs |
| **Control Software** | FreeNove GUI (Python-based, WiFi connected via IP) |
| **Power** | Rechargeable battery pack (onboard) |

---

## 🔧 Sensor Systems — Deep Dive

### 🔊 Ultrasonic Obstacle Avoidance (HC-SR04)

The ultrasonic sensor is mounted on a **servo motor** that sweeps to three fixed angles to map the space ahead.

```
Servo sweeps:  30° (Left)  →  90° (Center)  →  150° (Right)

Decision Logic:
┌──────────────────────────────────┬──────────────────────┐
│ Sensor Reading                   │ Robot Action         │
├──────────────────────────────────┼──────────────────────┤
│ ALL positions > 30 cm            │ Move Forward ✅       │
│ ALL positions < 30 cm            │ Reverse + U-Turn 🔄   │
│ Left < 30cm, Middle < 30cm       │ Turn Right →          │
│ Middle < 30cm, Right < 30cm      │ Turn Left ←           │
└──────────────────────────────────┴──────────────────────┘

Avoidance threshold: 30 cm
```

### 🔦 Infrared Line Tracking

Three infrared sensors (Left / Middle / Right) positioned underneath the chassis detect a black guide line on the ground surface.

```
Signal Interpretation:
  ■ Black surface = HIGH signal
  □ White surface = LOW signal

Navigation Rules:
  Left = HIGH           → Slight Left Turn
  Left + Middle = HIGH  → Full Left Turn
  Middle = HIGH         → Go Straight
  Right = HIGH          → Slight Right Turn
  Middle + Right = HIGH → Full Right Turn
```

### 💡 Light Tracing

Dual photoresistor sensors in the front of the robot detect relative light intensity and steer toward the brightest source.

```
Both sensors < 2.99V   → Go Forward
Left V > Right V       → Turn Left (light is on the left)
Right V > Left V       → Turn Right (light is on the right)
```

### 🎮 Motor Control System

All movement is controlled via `setMotorModel(A, B, C, D)` where each parameter maps to one of four wheels:

```python
setMotorModel(A, B, C, D)
#              │  │  │  └── Right Rear
#              │  │  └───── Left Rear  
#              │  └──────── Right Front
#              └─────────── Left Front

# Values: -4096 (full reverse) to +4096 (full forward), 0 = stop
# Mecanum wheels enable: Forward, Backward, Strafe Left/Right, Rotate
```

---

## 💼 Business Model & Value Proposition

### Target Markets
- 🎓 **College & University Campuses** — Student meal delivery between classes
- 🏥 **Medical Campuses & Hospitals** — Staff meal delivery without corridor congestion  
- 🏢 **Corporate Campuses** — Employee food delivery at scale

### Competitive Advantages

| Advantage | Detail |
|---|---|
| **Compact & Maneuverable** | Mecanum wheels enable omnidirectional movement through narrow campus pathways and courtyards |
| **Fully Autonomous** | No human operator required — CNN + RL handles all navigation decisions |
| **Low Operating Cost** | Electric, lightweight, no labor scaling needed |
| **Safe & Contactless** | Smart obstacle avoidance + sealed delivery compartment = hygienic, accident-resistant |
| **Scalable** | Integrates directly into existing cafeteria/food apps via API; adding more units requires no additional staffing |

### Value Creation

```
For Students/Employees:  ⏱ Saves time  |  🍔 24/7 access  |  📱 App ordering
For Food Vendors:        📈 More orders  |  💰 Lower labor  |  ⚡ Faster throughput
For Campus Operations:   🔒 Reduced risk  |  📊 Predictable costs  |  ♿ ADA compliant
```

---

## ⚖️ Legal & Ethical Considerations

### Legal Framework
- **Liability**: Clear definition of responsibility in collision or property damage scenarios
- **Food Safety**: Compliance with temperature control, contamination prevention, and health department standards
- **ADA Compliance**: Robot must not obstruct accessible ramps, entrances, or exits at any time

### Ethical Commitments
- **Pedestrian Safety First**: All autonomous behaviors are designed with human safety as the primary constraint
- **Privacy by Design**: Camera and sensor data is used only for navigation; no facial recognition or behavioral profiling
- **Workforce Transparency**: Acknowledges potential impact on delivery workers; advocates for responsible deployment

---

## 🛠️ Tech Stack & Learning Resources

| Technology | Role |
|---|---|
| **Python** | Primary programming language for all robot logic |
| **Raspberry Pi OS (Linux)** | Embedded operating system |
| **OpenCV** | Computer vision and image processing |
| **ROS 2** | Robot Operating System — sensor communication, mapping |
| **Docker** | Containerized deployment of AI models |
| **FreeNove SDK** | Hardware abstraction for motors, servos, LEDs |
| **WiFi / TCP-IP** | Real-time control and telemetry |

**Getting Started with Robotics:**  
→ [ROS.org](https://ros.org) — Robot Operating System documentation  
→ [Freenove FNK0043 Docs](https://docs.freenove.com/projects/fnk0043/en/latest/) — Platform SDK  
→ [Python.org](https://python.org) — Core programming language  

---

## 👥 Team

| Name | Role |
|---|---|
| **Md Nahid Rahman** | AI/ML Implementation, Hardware Integration, Sensor Programming |
| **Rohil Bhattarai** | Business Model, Competitive Analysis, System Architecture |
| **Gabriel Wood** | Research, Legal/Ethical Framework, Presentation |

---

## 📚 References

- Ahanessians, R. "Robots Are Changing Fast Food, Delivery, and the Future of Work." *Medium*, Nov. 2024.
- Fish, K. (2025). *Intro to Robotics*. Neil Griffin College of Business, Jonesboro.
- Fish, K. (2025). *DLNNs*. Neil Griffin College of Business, Jonesboro.
- Haddon, H. "The Robots That Are Taking Over Your Food Delivery." *The Wall Street Journal*, July 2025.
- Freenove. "FNK0043 — 4WD Smart Car Kit for Raspberry Pi." [docs.freenove.com](https://docs.freenove.com/projects/fnk0043/en/latest/)
- Grubhub Staff. "How Do Food Delivery Robots Work?" *Grubhub Onsite*, June 2025.

---

## <a name="contact"></a>📬 Contact

**Md Nahid Rahman** — [LinkedIn](#) · [Email](#)  
**Rohil Bhattarai** — [LinkedIn](#) · [Email](#)  
**Gabriel Wood** — [LinkedIn](#) · [Email](#)

---

<div align="center">
<i>Built with 🤖 at Neil Griffin College of Business · Summer 2024</i>
</div>
