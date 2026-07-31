
# 🌿🛡️ EcoSentinel: AI-Powered Environmental & Safety Monitor

> **Submission Track:** AI for Humanitarian Tech  
> **Core Stack:** ESP32 | Raspberry Pi Zero 2 W | PlatformIO (C++) | Python | Flask | Google Sheets API | Gemini AI

---

## 📌 Overview

**EcoSentinel** is a multi-sensor, edge-to-cloud environmental monitoring and hazard prevention system designed for smart homes, agriculture, and industrial safety. It continuously tracks critical environmental indicators—including air quality, gas leaks, ambient temperature, humidity, rainfall, soil moisture, and proximity hazards—processing data locally at the edge while streaming telemetry to the cloud.

When sensor readings breach safe operating thresholds, EcoSentinel triggers immediate local hardware alarms (high-decibel buzzer and I2C LCD alerts) alongside automated camera captures. Integrated with **Gemini AI**, the system automatically generates actionable, real-time safety assessment reports to protect lives and infrastructure.

---

## 🏗️ System Architecture


```
+-----------------------+
|   5V Power Supply     |
| (LT1117 Regulator)    |
+-----------+-----------+
|
v
+-----------------------------+   +-----------+-----------+   +-----------------------------+
|        SENSOR ARRAY         |   |                       |   |       LOCAL OUTPUTS         |
|                             |   |                       |   |                             |
| * DHT11 (Temp & Humidity)   +-->|                       +-->| * 16x2 I2C LCD Display      |
| * Rain Sensor (Intensity)   +-->|     ESP32 DevKit      |   | * High-Decibel Alarm Buzzer |
| * Soil Moisture Sensor      +-->| (Central Microcontroller)| |                             |
| * MQ Gas Sensor             +-->|                       |   +-----------------------------+
| * Ultrasonic (Proximity)    +-->|                       |
+-----------------------------+   +-----------+-----------+
|
| USB Serial / OTG
v
+-----------+-----------+
|  Raspberry Pi Zero 2 W|
|   (Edge Gateway)      |
+-----------+-----------+
|
+---> [ Local Flask Dashboard ]
|
v
+-----------+-----------+
| Google Sheets (Cloud) |
+-----------+-----------+
|
v
+-----------+-----------+
| Gemini AI Safety Engine|
+-----------------------+
```

---

## ⚙️ Sensor Alert Thresholds

EcoSentinel actively monitors environmental conditions and triggers local alarms and AI reports when parameters cross these calibrated boundaries:

| Sensor Module | Metric Tracked | Safety Threshold | Trigger Condition |
| :--- | :--- | :--- | :--- |
| **MQ Gas Sensor** | Gas Concentration | **2000 Analog Units** | `Reading > 2000` (Hazardous Gas) |
| **DHT11** | Ambient Temperature | **18°C – 36°C** | `Temp < 18°C` OR `Temp > 36°C` |
| **DHT11** | Relative Humidity | **30% – 80%** | `Humidity < 30%` OR `Humidity > 80%` |
| **Rain Sensor** | Rainfall Intensity | **1800 Analog Units** | `Reading < 1800` (Active Rain Detected) |
| **Soil Moisture** | Soil Hydration Level | **3000 Analog Units** | `Reading > 3000` (Dry / Sub-optimal) |
| **Ultrasonic (HC-SR04)**| Proximity / Distance | **30 cm** | `Distance < 30 cm` (Obstacle Hazard) |

---

## 🔧 Hardware Components & Circuitry

* **Microcontroller:** ESP32 DevKit
* **Edge Processing Unit:** Raspberry Pi Zero 2 W
* **Sensors:**
  * DHT11 (Temperature & Humidity)
  * Rain Intensity Sensor Module
  * Capacitive/Resistive Soil Moisture Sensor
  * MQ-Series Gas Detection Sensor
  * Ultrasonic Distance Sensor (HC-SR04)
* **Outputs:**
  * 16x2 Liquid Crystal Display with I2C Backpack Module
  * Active Piezo Alarm Buzzer
* **Power Regulation Circuit:**
  * Low Dropout Voltage Regulator (LT1117-ADJ)
  * Filtering Capacitors ($C1 = 10\mu\text{F}$, $C2 = 10\mu\text{F}$)
  * Output Adjustment Resistors ($R2 = 240\,\Omega$, $R3 = 390\,\Omega$)
  * Load Resistor ($R1 = 100\,\Omega$)
  * Input Source: 5V Regulated Supply

---

## 💻 Tech Stack & Software

* **Microcontroller Firmware:** C++ compiled using **PlatformIO** / Arduino Framework
* **Edge Server & Scripting:** Python 3, **Flask** (Local Dashboard), OpenCV / Camera Modules
* **Cloud & Automation:** **Google Apps Script** (Web App Endpoint), Google Sheets API
* **AI Intelligence:** **Gemini API** for real-time natural language safety analysis and hazard summary generation

---

## 📂 Repository Structure


```
EcoSentinel/
├── src/
│   └── main.cpp             # ESP32 Main Firmware (PlatformIO)
├── pi_scripts/
│   ├── camera_alert.py      # Raspberry Pi Camera & Hardware Trigger Script
│   └── app.py               # Local Flask Web Dashboard Server
├── apps_script/
│   └── Code.gs              # Google Apps Script Endpoint & Gemini AI Integration
├── platformio.ini           # PlatformIO Environment Configuration
└── README.md                # Project Documentation
```

---

## 🚀 Quick Setup & Run Guide

### 1. ESP32 Setup (PlatformIO)
1. Open the project root in **VS Code** with the **PlatformIO** extension installed.
2. Build the firmware:
   ```bash
   pio run

```
 3. Upload to your connected ESP32:
   ```bash
   pio run --target upload
   
   ```
 4. Monitor serial logs:
   ```bash
   pio device monitor
   
   ```
### 2. Raspberry Pi Setup
 1. SSH into your Raspberry Pi:
   ```bash
   ssh pi@raspberrypi.local
   
   ```
 2. Launch the camera alert service:
   ```bash
   python3 pi_scripts/camera_alert.py
   
   ```
### 3. Google Apps Script & Gemini API
 1. Deploy apps_script/Code.gs as a Web App in Google Drive.
 2. Configure your API_KEY with a valid key generated from Google AI Studio.
 3. Link the Web App URL inside your ESP32 / Raspberry Pi network request settings.
## 🔮 Future Work
 * **LoRaWAN Integration:** Enable long-range, low-power telemetry transmission for remote rural installations.
 * **Solar Power Station:** Integrate dedicated battery charging and solar power management for 100% off-grid operation.
 * **On-Device Edge AI:** Implement lightweight micro-quantized models directly on the Raspberry Pi for offline threat detection.
## 🤝 Acknowledgments & License
Built with passion for the **AI for Humanitarian Tech Track**. Released under the MIT License.
```

```
