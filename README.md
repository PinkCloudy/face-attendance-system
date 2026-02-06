# IoT-Based Face Recognition Attendance System

**Institution:** School of Electrical and Electronic Engineering (SEEE) - Hanoi University of Science and Technology (HUST)  
**Project Type:** Embedded Systems & Computer Vision Project

---

## 📋 Executive Summary

This project presents a cost-effective, scalable **Smart Face Attendance System** designed for modern office environments ("Smart Office"). Addressing the sanitary and security limitations of traditional RFID and fingerprint systems, this solution leverages a distributed **IoT Client-Server architecture**.

The system integrates an **ESP32-based embedded client** running **FreeRTOS** for efficient edge control and a central server utilizing the **InsightFace (ArcFace)** deep learning model for high-precision biometric recognition. The result is a robust system capable of real-time identification with an accuracy exceeding 99% under ideal conditions.

## 🏗 System Architecture

The system is designed as a hybrid model to optimize cost and performance:

1.  **The Client (Edge Node):**
    * **Hardware:** ESP32-DevKit V1 and ESP32-CAM (OV2640 Sensor).
    * **Role:** Image acquisition, video streaming, peripheral control (Servo, OLED), and power management via Deep Sleep.
    * **OS:** **FreeRTOS** is implemented to manage multi-threading (HTTP Server task vs. Control Logic task), ensuring a control latency of **<2ms**.

2.  **The Server (Processing Node):**
    * **Hardware:** PC/Workstation.
    * **Role:** Heavy-lifting AI inference (Face Detection & Recognition), database management, and hosting the Web Dashboard.
    * **Communication:** Data is transmitted via HTTP/WebSocket over a local WiFi network.

## 🛠 Tech Stack

### Hardware Components
| Component | Specification | Function |
| :--- | :--- | :--- |
| **MCU** | ESP32-DevKit V1 | Central processing unit for the client. |
| **Camera Module** | ESP32-CAM (OV2640) | Image capture and MJPEG streaming. |
| **Display** | OLED 0.96" (SSD1306) | I2C status visualization (IP, User Name). |
| **Actuator** | Servo SG90 | Simulates door lock mechanism. |
| **Sensor** | PIR HC-SR501 | Motion detection and Deep Sleep wake-up trigger. |
| **Power** | 12V Adapter + LM2596 | Stable power supply preventing brownout resets. |

### Software & Algorithms
* **Embedded Firmware:** C/C++ (ESP-IDF/Arduino Core), FreeRTOS.
* **Server Application:** Python 3.x, Streamlit (Web Interface).
* **Computer Vision:** OpenCV.
* **Deep Learning Model:** **InsightFace (ArcFace)**.
    * *Reason for selection:* Comparative testing showed InsightFace outperformed Dlib/HOG in handling head poses >45° and low-light conditions.
* **Database:** SQLite/MySQL.

## 🚀 Key Features

* **High-Performance Recognition:** Achieves **99.83% accuracy** using the ArcFace loss function, significantly reducing False Rejection Rates compared to Euclidean-based methods.
* **Real-Time Operation:**
    * Client Latency: < 2ms (via FreeRTOS task scheduling).
    * End-to-End Response Time: < 5 seconds.
* **Energy Efficiency:** Implements Deep Sleep mode (consuming ~5µA), activated by external PIR interrupts when no motion is detected.
* **Web Management Dashboard:** Allows administrators to view live streams, manage personnel databases, and review time-stamped attendance logs.

## 📊 Performance Evaluation

We conducted rigorous testing to validate the system's robustness:

| Metric | Face Recognition (Dlib) | InsightFace (ArcFace) |
| :--- | :--- | :--- |
| **Detection Algorithm** | HOG (CPU) / CNN | SCRFD |
| **Vector Dimension** | 128-D | 512-D |
| **Head Pose Tolerance** | < 30° | > 45° (Robust) |
| **Accuracy (Ideal)** | 99.38% | **99.83%** |
| **Failure Rate (Tested)** | 23.33% | **3.33%** |

*Data source: Experimental results on a dataset of 30 samples.*

## 🔧 Installation & Setup

### 1. Firmware Setup (Client)
1.  Open the `firmware` folder in **PlatformIO** or **Arduino IDE**.
2.  Install required libraries: `Esp32Servo`, `Adafruit_SSD1306`, `esp32-camera`.
3.  Configure WiFi credentials in `secrets.h`.
4.  Flash the code to the ESP32-DevKit V1.

### 2. Server Setup (Host)
1.  Navigate to the `server` directory.
2.  Install dependencies:
    ```bash
    pip install -r requirements.txt
    ```
3.  Run the application:
    ```bash
    streamlit run app.py
    ```

## 🔮 Future Improvements
* **Edge AI Integration:** Migrating the inference model to edge devices (e.g., ESP32-S3 or K210) to reduce server dependency.
* **Liveness Detection:** Integrating depth sensors or IR cameras to prevent 2D photo spoofing attacks.
* **Cloud Synchronization:** Syncing logs to Firebase/AWS for multi-location management.

## 👥 Contributors (Group 01)
* **Nguyen Minh Hung (20234011):** Embedded Systems & FreeRTOS Implementation.
* **Nguyen Quoc Huy (20234013):** System Analysis & Documentation.
* **Phan Nguyen Manh Loi (20234020):** Hardware Design & Camera Integration.
* **Hoang Tuan Ngoc (20234028):** AI Model & Server Backend.

---
*This project is submitted in partial fulfillment of the requirements for the Microprocessor Engineering course at Hanoi University of Science and Technology.*
