# Product Requirement Document (PRD) & Engineering Tradeoff Analysis
**Project:** EcoNode-Sentinel  
**Author:** Edge-AI Systems Engineer  

---

## 1. System Overview & Objectives
The EcoNode-Sentinel is an ultra-compact edge computing node designed to monitor micro-climates and detect sudden environmental anomalies (e.g., thermal spikes or rapid humidity drops) using hardware-accelerated machine learning directly on the silicon.

### Core Architecture Goals:
* **Form Factor:** High-density 4-layer PCB optimized for tight enclosure integration.
* **Firmware:** Bare-metal execution using the native ESP-IDF platform (no heavy Arduino wrappers).
* **Intelligence:** On-chip TinyML anomaly detection running at ultra-low latency.

---

## 2. Technical Specifications

### Power & Electrical
* **Input Voltage:** 5V DC via a standard USB-C interface.
* **Operating Voltage:** 3.3V DC regulated via a Low-Dropout (LDO) linear regulator.
* **Peak Current Draw:** ~500mA during intensive radio transmission or neural network inference blocks.

### Sensing & Communication
* **Primary Sensor:** Bosch BME280 (Temperature, Relative Humidity, Pressure).
* **Bus Architecture:** I2C Hardware Peripheral Bus running at 400kHz (Fast Mode).
* **Thermal Isolation:** Physical PCB isolation slots to prevent MCU core heat from skewing sensor data.
---

## 3. Engineering Tradeoff Analysis

| Metric Vector | Selected Design Path: ESP32-S3 + Local TinyML | Alternative Path: Raspberry Pi + Cloud AI | Engineering Justification |
| :--- | :--- | :--- | :--- |
| **Power Budget** | **Ultra-Low:** Sub-mA sleep cycles, <200mA active inference footprint. | **High Baseline:** 5V/3A peak draw. Demands massive power infrastructure. | The ESP32-S3 allows for field deployment without reliance on mains power arrays. |
| **Data Latency** | **Deterministic (<50ms):** Local inference execution right inside internal SRAM. | **Variable (100ms - 2s):** Highly dependent on network congestion and API availability. | Critical safety anomalies require instant edge mitigation, not cloud dependency. |
| **BOM Unit Cost** | **Low Cost:** System Bill of Materials (BOM) projected under $12 total. | **High Cost:** Single Board Computer alone exceeds $35+ before sensor costs. | Mass physical scaling requires aggressive optimization of per-unit silicon cost. |
