# ⚡ VOLT PILOT - Smart Solar & Battery Energy Management System

VOLT PILOT is an ESP32-S3-based smart power monitoring and load management controller designed to real-time track solar panel generation, monitor 18650 lithium-ion battery health, and dynamically switch DC loads via N-Channel MOSFETs.

---

## 🛠️ Hardware Stack & Components

* **Microcontroller:** ESP32-S3 Dev Module (Dual Core, 240MHz, Built-in Wi-Fi/BLE)
* **Power Supply:** 18650 Li-Ion Battery + DC-DC Boost Converter (3.7V to 5V Output)
* **Voltage Sensing:** 2x Analog Voltage Sensor Modules (0–25V Range)
* **Load Control:** N-Channel Power MOSFET (IRLZ44N / IRF540N)
* **Load:** 5V Mini DC Motor / Fan Assembly
* **Breadboard & Interconnects:** Shared Common Ground Rail architecture

---

## 🔌 Circuit Topology & Wiring Map

### 1. Pin Configuration (ESP32-S3)
| ESP32-S3 Pin | Connected Module / Component | Signal Type | Function |
| :--- | :--- | :--- | :--- |
| **GPIO 1** | Solar Voltage Sensor (`S` Pin) | ADC Input (12-bit) | Solar Voltage Measurement |
| **GPIO 2** | Battery Voltage Sensor (`S` Pin) | ADC Input (12-bit) | Battery Level Monitoring |
| **GPIO 4** | MOSFET Gate Pin | Digital Output | Load Switching (ON/OFF) |
| **VIN** | DC-DC Boost Converter Output (`+5V`) | Power Input | Main Board Power |
| **GND** | Breadboard Blue Rail (`-`) | System Ground | Common Ground Reference |

### 2. Common Ground Architecture
All ground references (`GND`) across the **ESP32-S3**, **Boost Converter (VIN-/VOUT-)**, **Voltage Sensors (-)**, and **MOSFET Source** are tied together on a single breadboard ground bus to ensure stable ADC reference voltages.

---

## 📈 Current Project Progress (Phase 1)

- [x] **Step 1: System Common Ground** – Established unified power and signal ground rail.
- [x] **Step 2: Solar Voltage Sensing** – Interfaced 0–25V module to GPIO 1 with ADC calibration.
- [x] **Step 3: Battery Voltage Sensing** – Connected 18650 battery telemetry to GPIO 2.
- [x] **Step 4: Power Distribution** – Stepped up 3.7V battery source to 5.0V stable rail via DC-DC Boost Converter for system power.
- [x] **Step 5: Load Control Circuit** – Integrated N-Channel MOSFET with GPIO 4 for DC Motor power gating.
- [x] **Step 6: Hardware Verification & Firmware** – Flash validated successfully via ESP32-S3 native USB interface.

---

## 💻 Firmware Implementation

The current firmware handles 12-bit analog sampling ($0 - 4095$), applies voltage divider math factor ($5:1$), and outputs real-time voltage data to the Serial Monitor at `115200` Baud, alongside automated load toggling.

```cpp
// VOLT PILOT - Phase 1 Validation Firmware
const int SOLAR_PIN = 1;     // GPIO 1 for Solar Sensor
const int BATT_PIN = 2;      // GPIO 2 for Battery Sensor
const int LOAD_MOSFET = 4;   // GPIO 4 for Load Control

void setup() {
  Serial.begin(115200);
  pinMode(LOAD_MOSFET, OUTPUT);
  digitalWrite(LOAD_MOSFET, LOW);
  analogReadResolution(12);
}

void loop() {
  int solarRaw = analogRead(SOLAR_PIN);
  int battRaw = analogRead(BATT_PIN);

  // 5:1 Divider attenuation formula
  float solarVolts = (solarRaw / 4095.0) * 3.3 * 5.0;
  float battVolts = (battRaw / 4095.0) * 3.3 * 5.0;

  Serial.print("Solar: "); Serial.print(solarVolts);
  Serial.print("V | Battery: "); Serial.print(battVolts); Serial.println("V");

  // Motor Toggle Diagnostic
  digitalWrite(LOAD_MOSFET, HIGH);
  delay(3000);
  digitalWrite(LOAD_MOSFET, LOW);
  delay(3000);
}
