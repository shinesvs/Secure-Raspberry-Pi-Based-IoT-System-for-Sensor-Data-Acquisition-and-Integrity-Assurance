# **Secure Sensor Node – Raspberry Pi Edge Monitoring System**

Real-time environmental + motion monitoring with **TLS 1.3 MQTT**, **AHT20 sensor integration**, **PIR motion detection**, and **automated video capture** on the edge.

---

## 🚀 **Overview**

This project implements a **secure edge-computing sensor node** built on a Raspberry Pi.
It collects environmental data (temperature & humidity), detects motion using a PIR sensor, records video evidence on triggers, computes a **SHA-256 hash** for integrity, and publishes all sensor readings to a **TLS-secured MQTT broker**.

Designed for applications in:

* Smart home security
* Distributed IoT sensor networks
* Smart grid or industrial anomaly detection
* Privacy-preserving edge monitoring

---

## 🔐 **Key Features**

### **1. Secure Communication**

* Uses **TLS 1.3** for encrypted MQTT communication
* Mutual authentication using:

  * `ca.crt`
  * Device certificate (`rpi1.crt`)
  * Private key (`rpi1.key`)

### **2. Sensor Integration**

* **AHT20** (I²C) for:

  * Temperature (°C)
  * Humidity (%)
* **PIR Motion Sensor** (GPIO 17)

  * Detects motion events
  * Triggers edge processing

### **3. Automated Video Capture**

* On motion detection:

  * Records a **10-second video** via `ffmpeg`
  * Stores to `/home/shine/videos/`
  * Computes **SHA-256 hash** for integrity validation

### **4. Real-Time MQTT Publishing**

Each publish event includes:

```json
{
  "timestamp": "...",
  "temperature_C": 23.5,
  "humidity_percent": 40.1,
  "motion_detected": true,
  "video_filename": "motion_20240101_120000.mp4",
  "video_hash": "a4b3...ff"
}
```

---

## 📁 **Project Structure**

```
secure_sensor_node.py
/home/shine/mqtt-certs/
    ├── ca.crt
    ├── rpi1.crt
    └── rpi1.key
/home/shine/videos/
    └── (motion recordings auto-generated)
```

---

## 🛠️ **Hardware Requirements**

* Raspberry Pi 4 / 3 (recommended)
* PIR Motion Sensor (GPIO)
* AHT20 Temperature + Humidity Sensor (I²C)
* USB Camera (e.g., `/dev/video0`)
* TLS certificates (`ca.crt`, `device.crt`, `device.key`)
* Local MQTT broker with TLS support (e.g., **Mosquitto**)

---

## 📦 **Software Requirements**

Install the required Python packages:

```bash
pip3 install adafruit-circuitpython-ahtx0 paho-mqtt RPi.GPIO
sudo apt install ffmpeg
```

---

## ▶️ **How to Run**

1. Connect sensors (PIR → GPIO17, AHT20 → I²C pins).
2. Place TLS certs inside:

   ```
   /home/shine/mqtt-certs/
   ```
3. Start your TLS MQTT broker.
4. Run the script:

```bash
python3 secure_sensor_node.py
```

---

## 🔍 **How It Works (High-Level Architecture)**

```
     ┌────────────────┐
     │ AHT20 Sensor   │─── Temperature/Humidity
     └────────────────┘
             │  I²C
             ▼
       ┌────────────┐        Motion Trigger        ┌──────────────┐
       │ Raspberry   │───────────────►────────────▶│ Video Capture │
       │ Pi Node     │                                └──────────────┘
       └────────────┘
             │
             ▼
     SHA-256 Hashing
             │
             ▼
     MQTT Publish (TLS 1.3)
             │
             ▼
     Secure Broker (Mosquitto)
```

---

## 🛡️ **Security Focus**

* End-to-end encrypted MQTT communication
* TLS 1.3 only (min/max forced)
* SHA-256 integrity verification for video evidence
* Hostname verification enabled

Perfect for demonstrating **secure IoT design**, **edge intelligence**, and **real-time embedded communication**.

---

## 📌 **Future Improvements**

* Add TinyML anomaly detection (e.g., detect unusual motion frequency)
* Implement OTA update mechanism
* Add local dashboard for live monitoring
* Support multiple topics (environment, video, integrity)
* Add message signing (HMAC)

---







