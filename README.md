# ⚡ VOLT PILOT | ESP32-S3 Smart Solar & Battery Energy Hub

A high-efficiency, real-time IoT power management and monitoring system built using **ESP32-S3**. Features live telemetry for Solar input, Battery health/SOC, Grid status, ACS712 current sensing, dynamic system efficiency tracking, and an interactive Web Dashboard UI.

---

## 🚀 Key Features Integrated

* **Dual ACS712 Current Sensing:** Live current measurements on both Solar (GPIO 3) and Battery/Load (GPIO 6) lines.
* **Real-time Power Calculation:** Dynamic calculation ($P = V \times I$) for Solar and Load channels.
* **Smart Efficiency & Power Factor:** Real-time system efficiency calculation (resolving 0% efficiency issues).
* **Battery State of Health (SOH) & SOC:** Automated State of Health tracking (**EXCELLENT / GOOD / WEAK**) and smooth SoC estimation.
* **CO₂ Carbon Offset Tracker:** Real-time tracking of clean energy usage and carbon emissions saved in grams ($g\,\text{CO}_2$).
* **Over-Voltage Protection (OVP):** Auto-cutoff threshold at 4.25V to safeguard Li-ion batteries from overcharging.
* **Grid Simulation:** Auto-detects grid availability via jumper/logic pin for zero-import energy routing.
* **Live Web Dashboard:** Modern, responsive UI with real-time JSON API polling, dynamic state pills, and remote MOSFET switch controls.

---

## 🛠️ Hardware Setup & Pin Mapping

| Component | ESP32-S3 Pin | Purpose |
| :--- | :--- | :--- |
| **Solar Voltage Input** | `GPIO 1` | Solar Panel Voltage Sensing |
| **Battery Voltage Input** | `GPIO 2` | Battery Voltage Sensing |
| **Solar ACS712 (OUT)** | `GPIO 3` | Solar Current Sensing |
| **Battery ACS712 (OUT)** | `GPIO 6` | Battery / Load Current Sensing |
| **Grid Simulation** | `GPIO 5` | Jumper Switch for Grid State |
| **Load Switching (MOSFET)** | `GPIO 4` | Load Relay / MOSFET Trigger |
| **5V Sensor Bus** | `VIN / 5V` | Shared 5V Rail via Breadboard |

---

## 📊 Circuit Wiring Diagram (ACS712 Integration)

```text
               +-------------------+
               |  ESP32-S3 BOARD   |
               +---------+---------+
                         |
           +-------------+-------------+
           | (5V Rail)        (GND Rail) |
           v                             v
  +-----------------+           +-----------------+
  | Solar ACS712    |           | Battery ACS712  |
  | VCC: 5V Rail    |           | VCC: 5V Rail    |
  | GND: GND Rail   |           | GND: GND Rail   |
  | OUT: GPIO 3     |           | OUT: GPIO 6     |
  +--------+--------+           +--------+--------+
           |                             |
      (In Series)                   (In Series)
           |                             |
     Solar Panel (+)               Battery / Load (+)
