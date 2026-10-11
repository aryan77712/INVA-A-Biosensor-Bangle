# Project I.N.V.A. (Integrated Non-invasive Vitals & Anomaly-tracker)

[![License: MIT](https://img.shields.io/badge/License-MIT-emerald.svg)](LICENSE)
[![Platform: ESP32-C3](https://img.shields.io/badge/Platform-ESP32--C3%20RISC--V-blue.svg)](https://www.espressif.com/)
[![Web App](https://img.shields.io/badge/Live%20Platform-inva.bolt.host-teal.svg)](https://inva.bolt.host)
[![BOM Cost](https://img.shields.io/badge/BOM%20Cost-%3C%E2%82%B9860-green.svg)](#bill-of-materials-bom)

Project **I.N.V.A.** is an ultra-low-cost, screenless biosensor wearable bangle and offline-first clinical triage ecosystem engineered specifically for low-resource communities and shared-phone households. 

By eliminating fragile touchscreens, display drivers, and power-hungry cellular radios, I.N.V.A. achieves **over 14 days of battery autonomy** on a single charge. It bridges the diagnostic data blackout between rural households and Primary Health Centres (PHCs) via an asymmetrical **Dual-Gateway Architecture** (direct Bluetooth sync or 3-second offline batch-sync for frontline ASHA community workers).

---

## 🌟 Key Innovations & Features

- **Distributed Mechanical Topology:** Eliminates the heavy central watch head. Electronics and split curved batteries are distributed along a flexible polyimide ribbon inside an ultra-slim (<22g, 4.2mm) medical-grade liquid silicone rubber (LSR) bangle.
- **Universal Modular Sizing (S to XL):** A standardized 135mm internal electronic core inserts into modular silicone sleeves, secured by an N52 magnetic micro-ratchet clasp with 2mm detents (providing 6mm of dynamic adjustment for daily wrist swelling).
- **Circadian Nadir Thermal Sensing:** Tracks core basal temperature shifts (+0.3°C to +0.5°C) strictly during the 3:00–5:00 AM deep-sleep nadir using a clinical-grade contact thermistor, validating post-ovulatory luteal phases without invasive waking routines.
- **5-Stage Edge Syncope Algorithm:** Localized motion vector pipeline running on an onboard 6-axis IMU detects sudden fainting falls from anemia or postural hypotension, complete with an onboard acoustic warning and a 30-second localized tap-cancellation window.
- **Offline-First Data Duality:** Caches up to 365 days of encrypted 64-byte daily health vectors on local SPI flash memory. Visiting ASHA workers can batch-sync an entire year of records in under 3.2 seconds without requiring local internet connectivity.
- **1-Page Standardized PHC Summary Card:** Compiles longitudinal sensor records into a clean, FIGO-compliant clinical card allowing rural medical officers to evaluate months of physiological history in seconds.

---

## 📐 Hardware Architecture

```
                  +-------------------------------------------------+
                  |      Liquid Silicone Rubber (LSR) Enclosure     |
                  +-------------------------------------------------+
                  |                                                 |
  +-------------+ |  +------------+  +-----------+  +------------+  | +---------------+
  | Li-Po Cell  | |  | ESP32-C3   |  | MAX30205  |  | Li-Po Cell |  | | Flush Clasp   |
  | 125mAh Arc  |====| RISC-V SoC |==| MAX30102  |==| 125mAh Arc |====| N52 Neodymium |
  | (Left Quad) | |  | + BLE 5.0  |  | MPU-6050  |  | (Right Qd) |  | | Micro-Ratchet |
  +-------------+ |  +------------+  +-----------+  +------------+  | +---------------+
                  |  ~~~~~~~~~~~~~~~~~ FPC Ribbon ~~~~~~~~~~~~~~~~~  |
                  +-------------------------------------------------+
                  |    Underside Micro-Ventilation Ridges (0.4mm)   |
                  +-------------------------------------------------+
```

### Sensor Cluster & Interfacing
| Sensor | Interface | Modality / Purpose | Clinical Metric |
| :--- | :--- | :--- | :--- |
| **MAX30205** | I2C Fast Mode | Clinical Skin Thermometry (0.1°C precision) | Basal body temperature shift ($\Delta T$) |
| **MAX30102** | I2C Fast Mode | Dual-wavelength optical photoplethysmography | Resting HR, HRV RMSSD, Perfusion Index |
| **MPU-6050** | I2C / INT | 6-Axis Inertial Motion Tracking | 5-stage edge syncope fall detection |

---

## 💻 Software & Platform Architecture

The companion clinical triage web platform is deployed live at **[inva.bolt.host](https://inva.bolt.host)** and includes:

1. **Patient Dashboard (`/`):** Real-time device telemetry, connection status, nadir thermal delta, resting heart rate, and remaining battery life.
2. **Vital Analytics (`/analytics`):** Interactive dual-axis graph displaying 30-day biphasic thermal curves alongside resting heart rate and FIGO regularity indices.
3. **Symptom Calendar (`/calendar`):** Contextual logger for cramp severity, flow intensity, and somatic indicators.
4. **Actionable Care Hub (`/solutions`):** Phase-specific nutritional recommendations and guided 4-7-8 parasympathetic breathing pacers.
5. **Emergency Hub (`/safety`):** State-machine syncope logs, cancellation audit trails, and direct emergency routing (108/102).
6. **ASHA Supervisor Portal (`/asha-portal`):** Secure access portal for frontline health workers to batch-sync flash memory in under 3.2 seconds.

---

## 💰 Bill of Materials (BOM)

Direct unit cost when fabricated in volume (1,000-unit batch):

| Subsystem / Component | Specifications | Est. Cost (INR) |
| :--- | :--- | :--- |
| **ESP32-C3FH4 RISC-V SoC** | 32-bit, 160MHz, 4MB Flash, BLE 5.0 | ₹160.00 |
| **MAX30205 Thermal Sensor** | Human Body Temp Sensor, 0.1°C Accuracy | ₹120.00 |
| **MAX30102 Optical Sensor** | Pulse Oximetry & Optical Biosensor (660/940nm) | ₹140.00 |
| **MPU-6050 6-Axis IMU** | Motion Tracking Accelerometer / Gyroscope | ₹90.00 |
| **Dual Curved Li-Po Battery** | 250mAh total (2x 125mAh arc cells) with PCM | ₹150.00 |
| **Medical Silicone & Clasp** | Medical LSR + N52 Neodymium Magnetic Clasps | ₹120.00 |
| **Flexible PCB Ribbon (FPC)** | 135mm double-sided polyimide flexible ribbon | ₹45.00 |
| **Passives, Antenna & Piezo** | Ceramic 2.4GHz antenna, passives, micro-buzzer | ₹35.00 |
| **TOTAL DIRECT BOM** | **Direct Raw Component Cost Per Unit** | **₹860.00 (~$10.30 USD)** |

---

## 🚀 Getting Started

### Hardware Requirements
- ESP32-C3 development board or custom FPC spine
- MAX30205 clinical temperature sensor (I2C address: `0x48`)
- MAX30102 optical pulse/oximetry sensor (I2C address: `0x57`)
- MPU-6050 6-axis IMU (I2C address: `0x68`)

### Web Platform Setup
```bash
# Clone the repository
git clone https://github.com/your-username/inva-biosensor-bangle.git

# Navigate to web dashboard directory
cd inva-biosensor-bangle/web

# Install dependencies
npm install

# Start local development server
npm run dev
```

---

## 📜 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.