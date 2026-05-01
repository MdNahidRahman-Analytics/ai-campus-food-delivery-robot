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

[📄 View Presentation](Campus_Food_Delivery_Presentation.pdf) ·  [📬 Contact Team](#contact)

</div>

---

## 📸 Project Gallery

| Robot Platform | Sensor Array | Line Tracking |
|:-:|:-:|:-:|
| ![Robot with Raspberry Pi](Autonomous%20Delivery%20Robot.jpeg) | ![Ultrasonic Sensor](Ultrasonic%20sensor.png) | ![Line Tracking](Infrared%20Line%20Tracking%20Sensor.png) |
| *Raspberry Pi 4 on FreeNove 4WD Mecanum chassis* | *HC-SR04 ultrasonic sensor + Pi Camera on servo mount* | *3-channel infrared tracking sensor array* |

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

| Capability | Description |
|---|---|
| **Object Detection & Tracking** | Identifies pedestrians, obstacles, vehicles, and landmarks in real-time |
| **Semantic Segmentation** | Distinguishes navigable surfaces — sidewalks vs. grass vs. roads vs. restricted zones |
| **Scene Understanding** | Recognizes building entrances, delivery points, and campus-specific landmarks |

### 2. Localization & Mapping — Recurrent Neural Network (RNN)

- **Sensor Fusion**: Merges ultrasonic, infrared, camera, and GPS data into a unified world model
- **Real-Time Mapping**: Continuously updates the campus map as the robot navigates
- **GPS Integration**: Uses GPS waypoints as high-level anchors while sensors handle fine-grained correction

### 3. Path Planning & Decision Making — Reinforcement Learning (RL)

- **Reward Function**: Positive for efficient delivery; negative for collisions, blocked paths, or ADA violations
- **Behavior Prediction**: Anticipates pedestrian and cyclist movement to proactively adjust trajectory
- **Dynamic Rerouting**: Responds in real-time to unexpected obstacles or blocked routes

---

## ⚙️ Hardware Specifications

| Component | Specification |
|---|---|
| **Compute** | Raspberry Pi 4 Model B |
| **Chassis** | FreeNove 4WD with Mecanum wheels (omnidirectional) |
| **Drive System** | 4 independent DC motors |
| **Primary Sensor** | HC-SR04 Ultrasonic Sensor (servo-mounted, 180° sweep) |
| **Line Tracking** | 3-channel Infrared Tracking Sensor Array |
| **Vision** | Raspberry Pi Camera Module (servo pan/tilt mount) |
| **Light Sensing** | Dual photoresistor light-tracing array |
| **LED System** | 8x individually addressable RGB LEDs |
| **Control Software** | FreeNove GUI (Python-based, WiFi via IP) |
| **Power** | Rechargeable onboard battery pack |

---

## 🔧 Sensor Systems

### 🔊 Ultrasonic Obstacle Avoidance (HC-SR04)

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

```
Signal: Black = HIGH  |  White = LOW

Left = HIGH           → Slight Left Turn
Left + Middle = HIGH  → Full Left Turn
Middle = HIGH         → Go Straight
Right = HIGH          → Slight Right Turn
Middle + Right = HIGH → Full Right Turn
```

### 💡 Light Tracing

```
Both sensors < 2.99V  → Go Forward
Left V > Right V      → Turn Left
Right V > Left V      → Turn Right
```

### 🎮 Motor Control

```python
setMotorModel(A, B, C, D)
# A = Left Front | B = Right Front | C = Left Rear | D = Right Rear
# Range: -4096 (full reverse) to +4096 (full forward) | 0 = stop
```

---

## 💼 Business Model

### Target Markets
- 🎓 **College & University Campuses** — Student meal delivery between classes
- 🏥 **Medical Campuses & Hospitals** — Staff meal delivery without corridor congestion
- 🏢 **Corporate Campuses** — Employee food delivery at scale

### Competitive Advantages

| Advantage | Detail |
|---|---|
| **Compact & Maneuverable** | Mecanum wheels navigate narrow pathways omnidirectionally |
| **Fully Autonomous** | CNN + RL handles all navigation — no human operator needed |
| **Low Operating Cost** | Electric, lightweight, zero labor scaling |
| **Safe & Contactless** | Smart obstacle avoidance + sealed compartment |
| **Scalable** | API-ready for existing food apps; more units = no extra staffing |

---

## ⚖️ Legal & Ethical Considerations

- **Liability**: Clear responsibility frameworks for collision and property damage
- **Food Safety**: Temperature control, contamination prevention, health department compliance
- **ADA Compliance**: Never obstructs accessible ramps, entrances, or exits
- **Pedestrian Safety**: Human safety is the primary constraint in all autonomous behaviors
- **Privacy by Design**: Camera data used only for navigation — no facial recognition

---

## 🛠️ Tech Stack

| Technology | Role |
|---|---|
| **Python** | Primary robot programming language |
| **Raspberry Pi OS (Linux)** | Embedded operating system |
| **OpenCV** | Computer vision and image processing |
| **ROS 2** | Sensor communication and mapping layer |
| **Docker** | Containerized AI model deployment |
| **FreeNove SDK** | Motor, servo, and sensor hardware abstraction |
| **WiFi / TCP-IP** | Real-time control and telemetry |

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
