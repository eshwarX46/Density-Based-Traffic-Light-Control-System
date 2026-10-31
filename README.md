# 🚦 Density Based Traffic Light Control System
### Adaptive 4-Way Traffic Signal Controller Using Arduino Mega and IR Sensors

![Platform](https://img.shields.io/badge/Platform-Arduino%20Mega%202560-blue)
![Sensors](https://img.shields.io/badge/Sensors-12%20IR%20Sensors-orange)
![Display](https://img.shields.io/badge/Display-16x2%20I2C%20LCD-purple)
![Control](https://img.shields.io/badge/Control-Adaptive%20Traffic-success)
![Status](https://img.shields.io/badge/Project-Completed-brightgreen)

---

## 📖 Overview

The **Density Based Traffic Light Control System** is an adaptive traffic-management prototype designed to improve conventional fixed-time traffic signals.

The system monitors traffic density across four approaches:

- North
- South
- East
- West

Each road uses **3 IR sensing zones**, giving a total of **12 IR sensors**.

The Arduino Mega 2560 continuously monitors the sensors, estimates the traffic density of each road, assigns priority to the most congested approach, and dynamically adjusts the green-signal duration.

The system also provides live traffic information through a **16×2 I²C LCD display**.

---

## 📸 Project Image

Add your final project image here:

```html
![Uploading image 1.jpg…]()

```

---

## ✨ Features

- 🚦 4-Way Adaptive Traffic Signal Control
- 📡 12 IR Traffic Sensing Zones
- 🧠 Real-Time Traffic Density Estimation
- ⏱ Dynamic Green Signal Timing
- 🔴🟡🟢 Independent Red, Yellow and Green Signals
- 📟 16×2 I²C LCD Monitoring
- ⚖ Density-Based Traffic Priority
- 🔄 Fair Rotating Priority for Equal Traffic Density
- 🛑 All-Red Safety Interval
- 🟡 Yellow Transition Phase
- 🔍 Sensor Reading Stabilization
- 🚘 Sensor Activation Order Independent
- ♻ Continuous Automatic Operation
- ✅ Only One Direction Receives Green at a Time

---

# System Architecture

```text
                    ┌─────────────────────┐
                    │    5V Power Supply   │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Arduino Mega 2560   │
                    │                     │
                    │ Density Estimation  │
                    │ Priority Selection  │
                    │ Signal Timing       │
                    └───────┬─────────────┘
                            │
          ┌─────────────────┼──────────────────┐
          │                 │                  │
          ▼                 ▼                  ▼
  ┌──────────────┐   ┌──────────────┐   ┌──────────────┐
  │ 12 IR Sensor │   │ Traffic LEDs │   │ 16x2 I2C LCD│
  │    Inputs    │   │ R / Y / G    │   │ Live Status  │
  └──────────────┘   └──────────────┘   └──────────────┘

       North ── 3 IR Sensors
       South ── 3 IR Sensors
       East  ── 3 IR Sensors
       West  ── 3 IR Sensors
```

---

# Project Highlights

✔ Arduino Mega 2560 Based Controller

✔ 12 IR Sensors

✔ 4-Way Intersection

✔ Adaptive Green Signal Timing

✔ Density-Based Priority Control

✔ Equal-Density Fair Rotation

✔ LCD Live Monitoring

✔ Sensor Noise Filtering

✔ All-Red Safety Interval

✔ Real-Time Traffic Status

---

# Traffic Density Logic

Traffic density is estimated using the **number of active IR sensing zones**.

The sensor activation order does not matter.

| Active Sensors | Example Patterns | Traffic Density | Green Time |
|---|---|---|---:|
| 0 | `000` | Empty | No Green Request |
| 1 | `100`, `010`, `001` | Low | 7 Seconds |
| 2 | `110`, `101`, `011` | Medium | 10 Seconds |
| 3 | `111` | High | 15 Seconds |

Example:

```text
North Sensor State = 101
```

Two sensors are active, therefore:

```text
Traffic Density = MEDIUM
Green Time = 10 Seconds
```

---

# Signal Timing

| Signal Condition | Duration |
|---|---:|
| Low Density Green | 7 Seconds |
| Medium Density Green | 10 Seconds |
| High Density Green | 15 Seconds |
| Yellow Signal | 3 Seconds |
| All-Red Safety Interval | 1 Second |

Signal transition:

```text
GREEN
  │
  ▼
YELLOW
3 Seconds
  │
  ▼
ALL RED
1 Second
  │
  ▼
NEXT PRIORITY ROAD
```

---

# Hardware Used

| Component | Quantity |
|---|---:|
| Arduino Mega 2560 | 1 |
| IR Obstacle Sensor Modules | 12 |
| Red LEDs | 4 |
| Yellow LEDs | 4 |
| Green LEDs | 4 |
| 220Ω / 330Ω Resistors | 12 |
| 16×2 I²C LCD Display | 1 |
| Breadboard | As Required |
| Jumper Wires | As Required |
| 5V / 2A Power Supply | 1 |
| Toy Vehicles | As Required |
| Project Base Board | 1 |

---

# Pin Configuration

## IR Sensors

| Direction | IR1 | IR2 | IR3 |
|---|---:|---:|---:|
| North | 22 | 23 | 24 |
| South | 25 | 26 | 27 |
| East | 28 | 29 | 30 |
| West | 31 | 32 | 33 |

Each IR sensor:

```text
VCC → 5V
GND → GND
OUT → Assigned Arduino Pin
```

The sensors used in this prototype are active-low:

```text
LOW  = Object Detected
HIGH = No Object
```

---

## Traffic Signal LEDs

| Direction | Red | Yellow | Green |
|---|---:|---:|---:|
| North | 34 | 35 | 36 |
| South | 37 | 38 | 39 |
| East | 40 | 41 | 42 |
| West | 43 | 44 | 45 |

LED connection:

```text
Arduino Pin
     │
     ▼
220Ω / 330Ω Resistor
     │
     ▼
LED
     │
     ▼
GND
```

---

## LCD Connection

| LCD Pin | Arduino Mega |
|---|---|
| VCC | 5V |
| GND | GND |
| SDA | Pin 20 |
| SCL | Pin 21 |

I²C address used:

```text
0x27
```

---

# Software & Technologies

- Arduino IDE
- Embedded C / C++
- Arduino Mega 2560
- Digital I/O
- I²C Communication
- Finite State Machine
- Priority Scheduling
- Adaptive Timing
- IR Sensor Interfacing
- LCD Interface

---

# Libraries Used

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
```

The `Wire` library is included with Arduino IDE.

Install the `LiquidCrystal_I2C` library using the Arduino Library Manager.

---

# Working Principle

The system continuously reads all 12 IR sensors.

For each road, the number of active sensors is calculated.

```text
0 Sensors → EMPTY
1 Sensor  → LOW
2 Sensors → MEDIUM
3 Sensors → HIGH
```

The controller then determines the road with the highest traffic density.

Priority:

```text
HIGH > MEDIUM > LOW > EMPTY
```

The selected road receives a green signal according to its density.

After the green period:

```text
Green
  ↓
Yellow for 3 Seconds
  ↓
All Red for 1 Second
  ↓
Next Road
```

Every occupied road is allowed one turn during the current service cycle.

---

# Priority Logic

Example traffic condition:

```text
North = HIGH
South = LOW
East  = MEDIUM
West  = EMPTY
```

Expected priority:

```text
North
  ↓
East
  ↓
South
```

Corresponding timing:

```text
North  → Green 15 Seconds
East   → Green 10 Seconds
South  → Green 7 Seconds
```

West is skipped because no traffic is detected.

---

# Equal Density Handling

If multiple roads have the same density, the controller uses a **rotating priority system**.

Example:

```text
North = HIGH
South = HIGH
East  = HIGH
West  = HIGH
```

Service sequence:

```text
North
  ↓
South
  ↓
East
  ↓
West
```

The next complete cycle again evaluates the latest sensor conditions.

This prevents one road from repeatedly receiving priority while other occupied roads are waiting.

---

# Sensor Filtering

IR sensors can occasionally generate unstable readings due to:

- Object reflectivity
- Sensor alignment
- Electrical noise
- Environmental lighting

To improve reliability, each IR sensor is sampled multiple times.

```text
Samples per Reading = 7
```

The sensor is considered active when at least:

```text
4 out of 7 readings
```

detect an object.

This majority-voting method improves stability during operation.

---

# LCD Monitoring

The 16×2 I²C LCD provides live traffic information.

### Overall Density

```text
N:H S:L E:M W:0
N GREEN 15s
```

Where:

```text
H = High
M = Medium
L = Low
0 = Empty
```

### Detailed Sensor Status

Example:

```text
N:101 MED
S:001 LOW
```

and:

```text
E:110 MED
W:111 HIGH
```

The display therefore shows:

- Traffic density
- Raw sensor status
- Active road
- Current signal phase
- Remaining signal time

---

# Project Workflow

```text
Start
  │
  ▼
Initialize Arduino Mega
  │
  ▼
Set All Signals RED
  │
  ▼
Read 12 IR Sensors
  │
  ▼
Calculate Density
for N / S / E / W
  │
  ▼
Update LCD
  │
  ▼
Traffic Detected?
  │
  ├── No ──► Keep All Signals RED
  │
  ▼ Yes
Select Highest Density Road
  │
  ▼
Apply Green Timing
  │
  ▼
Yellow - 3 Seconds
  │
  ▼
All Red - 1 Second
  │
  ▼
Mark Road as Served
  │
  ▼
Select Next Occupied Road
  │
  ▼
Repeat
```

---

# Experimental Results

The completed prototype was tested under multiple traffic conditions.

| Test | Result |
|---|---|
| North Traffic Detection | ✅ Passed |
| South Traffic Detection | ✅ Passed |
| East Traffic Detection | ✅ Passed |
| West Traffic Detection | ✅ Passed |
| 12 IR Sensor Operation | ✅ Passed |
| Low Density Detection | ✅ Passed |
| Medium Density Detection | ✅ Passed |
| High Density Detection | ✅ Passed |
| Sensor Order Independence | ✅ Passed |
| Adaptive Green Timing | ✅ Passed |
| Yellow Transition | ✅ Passed |
| All-Red Safety Interval | ✅ Passed |
| Density Priority | ✅ Passed |
| Equal Density Rotation | ✅ Passed |
| LCD Live Monitoring | ✅ Passed |
| One Green Direction at a Time | ✅ Passed |
| Continuous Operation | ✅ Passed |

---

# Demonstration Scenario

One of the main test cases:

```text
North = HIGH
South = LOW
East  = MEDIUM
West  = EMPTY
```

The controller responds as:

```text
North Green 15s
      ↓
North Yellow 3s
      ↓
All Red 1s
      ↓
East Green 10s
      ↓
East Yellow 3s
      ↓
All Red 1s
      ↓
South Green 7s
```

This demonstrates the adaptive nature of the controller.

---

## 🎥 Demonstration Video

Add your YouTube working-video link here:

```markdown
[▶ Watch Project Demonstration](YOUR_YOUTUBE_VIDEO_LINK)
```

The video demonstrates:

- Empty traffic condition
- Low traffic density
- Medium traffic density
- High traffic density
- Mixed traffic conditions
- Priority selection
- Adaptive signal timing
- Yellow transition
- All-red safety interval
- LCD monitoring

---

# Applications

- Smart Traffic Management
- Adaptive Traffic Signal Systems
- Smart City Research
- Embedded Systems Education
- Engineering Mini Projects
- Traffic Management Demonstrations
- Intelligent Transportation Research
- Arduino-Based Control Systems

---

# Advantages

- Adaptive signal timing
- Simple hardware architecture
- Low-cost implementation
- Real-time traffic-density estimation
- Automatic priority control
- Fair equal-density scheduling
- Clear LCD monitoring
- Reduced unnecessary green time
- Modular design
- Easy to demonstrate and modify

---

# Limitations

- IR sensors estimate sensing-zone occupancy rather than exact vehicle count
- IR sensors may be affected by environmental conditions
- Prototype uses simplified one-direction-at-a-time control
- No emergency vehicle priority
- No pedestrian crossing system
- No cloud monitoring
- No camera-based vehicle classification

---

# Future Scope

- 📷 Camera-Based Traffic Detection
- 🤖 AI Traffic Density Estimation
- 🚑 Emergency Vehicle Priority
- 🚶 Pedestrian Signal Control
- 🌐 IoT-Based Remote Monitoring
- ☁ Cloud Dashboard
- 📊 Traffic Data Logging
- 📱 Mobile Application
- 🧠 Machine Learning Traffic Prediction
- 🚘 Vehicle Counting and Classification
- 🌙 Day/Night Adaptive Operation
- 🗺 Multi-Intersection Coordination
- 🔋 Solar-Powered Traffic Control

---

# Author

**Eshwar Prasad Y**

Electronics & Communication Engineering

Embedded Systems | IoT | Automotive Electronics | Arduino | Microcontrollers

Email: eshwargowda313@gmail.com

LinkedIn: linkedin.com/in/eshwar-prasad-y-a3043b370

---

# Acknowledgements

Special thanks to:

- Arduino Community
- Open Source Hardware Community
- Embedded Systems Community
- Developers and contributors of the LiquidCrystal_I2C library

---

# Project Status

```text
✅ Hardware Completed
✅ Software Completed
✅ Sensor Testing Completed
✅ Four-Way Integration Completed
✅ LCD Integration Completed
✅ Adaptive Priority Logic Completed
✅ Working Prototype Demonstrated
```

---

## ⭐ Support

If you find this project useful, consider giving the repository a **Star ⭐**.

Feel free to fork the repository and extend the system with IoT, AI, camera-based traffic detection, emergency vehicle priority, or smart-city connectivity.

---

### 🚦 Density Based Traffic Light Control System

**Arduino Mega 2560 | 12 IR Sensors | 4-Way Adaptive Traffic Control | Real-Time LCD Monitoring**
