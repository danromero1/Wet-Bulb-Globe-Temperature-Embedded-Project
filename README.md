# WBGT Heat Stress Monitor

![MCU](https://img.shields.io/badge/MCU-PIC18F4321-blue.svg)
![Language](https://img.shields.io/badge/Language-C-A8B9CC.svg)
![Status](https://img.shields.io/badge/Status-Functional-green.svg)

> A portable embedded system for real-time heat stress monitoring using multi-sensor environmental data.

---

## Project Overview

This project implements a portable Wet Bulb Globe Temperature (WBGT) heat stress monitor using a PIC18F4321 microcontroller. The system measures temperature, humidity, light intensity, and wind speed to compute the WBGT index and display real-time safety recommendations on an LCD.

WBGT is widely used by athletic trainers, military personnel, and industrial safety officers to assess heat-related illness risk during outdoor activity.

**Course:** ECE 3301 – Microcontroller Systems  
**Institution:** Cal Poly Pomona  
**Date:** Fall 2024  
**Team:** Daniel Romero, Isaias Rafael-Sepulveda  

---

## Features

- **Multi-Sensor Environmental Monitoring**
  - Temperature & humidity (DHT11 single-wire protocol)
  - Light intensity (BH1750 via bit-banged I²C)
  - Wind speed (analog sensor via ADC)

- **WBGT Computation**
  - Fixed-point WBGT calculation
  - Threshold-based heat stress classification
  - Five risk levels with safety recommendations

- **Embedded User Interface**
  - 20×4 character LCD
  - Real-time sensor values and WBGT display
  - Continuous monitoring loop

---

## Tech Stack

### Hardware
- PIC18F4321 microcontroller
- DHT11 temperature/humidity sensor (single-wire)
- BH1750 digital light sensor (bit-banged I²C)
- Analog wind speed sensor
- HD44780-compatible LCD (parallel interface)

### Software & Tools
- **Language:** C (XC8)
- **IDE:** MPLAB X
- **Programmer:** PICkit
- **Debugging:** Oscilloscope, DMM

---

## System Design

### Sensor Interfaces

- **DHT11**
  - Vendor-defined single-wire digital protocol
  - 40-bit frame with checksum validation

- **BH1750**
  - I²C protocol implemented via software bit-banging
  - Start/stop, ACK/NACK, and multi-byte reads handled in firmware

- **Wind Speed Sensor**
  - Analog voltage proportional to wind speed
  - Sampled using PIC18F ADC with external reference

### Display Interface

- 20×4 LCD using 4-bit parallel mode
- Custom LCD driver for cursor control, formatted output, and numeric display

---

## Firmware Architecture

- Finite State Machine–based control flow
- Fixed-point arithmetic to avoid floating-point overhead
- Resource-aware memory usage to fit within PIC flash and RAM limits
- Periodic sampling loop with deterministic timing

Main loop sequence:
1. Read environmental sensors  
2. Compute WBGT index  
3. Classify heat stress level  
4. Update LCD display  

---

## Current Status

**Completed**
- Sensor integration and validation
- WBGT computation and classification
- LCD user interface
- End-to-end functional prototype

**Known Issues**
- Intermittent BH1750 timing sensitivity (bit-banged I²C)
- Occasional DHT11 checksum errors at high humidity

**Planned Improvements**
- Improve I²C timing robustness
- Add EEPROM data logging
- Audible alert for extreme WBGT levels
- Battery-powered enclosure

---

## 📊 WBGT Classification

WBGT is calculated using:
WBGT = (0.7 × Wet Bulb) + (0.2 × Globe) + (0.1 × Dry Bulb)

| WBGT (°F) | Risk Level | Recommendation |
|----------|------------|----------------|
| < 80 | No Risk | Normal activity |
| 80–84.9 | Low | Scheduled hydration breaks |
| 85–87.9 | Moderate | Increased rest and monitoring |
| 88–89.9 | High | Constant supervision |
| ≥ 90 | Extreme | Suspend activity |

---

## Learning Outcomes

This project reinforced skills in:

- Embedded C programming on resource-constrained MCUs
- Protocol-level sensor interfacing
- Bit-banged digital communication
- ADC configuration and calibration
- Safety-critical embedded system design
- Debugging timing-sensitive firmware

---

## Contact

**Daniel Romero**  
Email: daniel.romero@ieee.org  
Portfolio: https://electricalromero.com  
LinkedIn: https://www.linkedin.com/in/daniel-romero-ee  

---

**Project Status:** Complete  
**Last Updated:** December 2025
