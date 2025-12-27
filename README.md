# WBGT Heat Stress Monitor

![Microcontroller](https://img.shields.io/badge/MCU-PIC18F4321-blue?style=flat-square)
![Language](https://img.shields.io/badge/language-C-A8B9CC?style=flat-square&logo=c&logoColor=white)
![Interface](https://img.shields.io/badge/interface-I2C%20%7C%20ADC-orange?style=flat-square)
![Status](https://img.shields.io/badge/status-Functional-green?style=flat-square)

An embedded systems project that monitors environmental conditions and calculates the Wet Bulb Globe Temperature (WBGT) index to assess heat stress risk levels for athletic training and outdoor work safety.

---

## Overview

This project implements a portable WBGT heat stress monitor using a PIC18F4321 microcontroller with multiple environmental sensors. The system continuously measures temperature, humidity, light intensity, and wind speed to calculate the WBGT index and display real-time safety recommendations on an LCD screen.

WBGT is a critical metric used by athletic trainers, military personnel, and industrial safety officers to prevent heat-related illnesses during physical activity in hot conditions.

**Course:** ECE 3301 - Microcontroller Systems  
**Institution:** Cal Poly Pomona  
**Date:** Fall 2024  
**Team:** Daniel Romero & Isaias Rafael-Sepulveda

---

## Project Motivation

Heat stress is a serious concern in athletics, military training, and outdoor labor. The WBGT index provides a more accurate assessment of heat stress than simple air temperature by accounting for humidity, radiant heat, and wind speed. Commercial WBGT monitors cost $200-$500; this project demonstrates a functional prototype at a fraction of the cost.

**WBGT Formula:**
```
WBGT = (0.7 × Wet Bulb Temp) + (0.2 × Globe Temp) + (0.1 × Dry Bulb Temp)
```

Where:
- **Wet Bulb Temperature** ≈ Humidity + Wind + Radiation
- **Globe Temperature** ≈ Radiation + Wind  
- **Dry Bulb Temperature** = Air Temperature

---

## Features

**Environmental Sensing**
- Temperature and humidity measurement (DHT11 sensor)
- Light intensity measurement (BH1750 I2C sensor)
- Wind speed measurement (analog voltage sensor via ADC)
- Real-time WBGT index calculation

**Safety Classification**
- Five-level heat stress classification (No Risk, Low, Moderate, High, Extreme)
- Activity-specific safety recommendations
- Automatic threshold-based alerts

**Display Interface**
- 20×4 character LCD display
- Real-time sensor readings
- Heat stress level indication
- Safety recommendations based on WBGT index

---

## Hardware Components

**Microcontroller**
- PIC18F4321 (8-bit, 40 MHz capable)
- Internal 4 MHz oscillator
- 10-bit ADC for analog sensors
- Hardware I2C (MSSP module)

**Sensors**
- DHT11 - Temperature & Humidity (1-wire protocol)
- BH1750 - Light Intensity (I2C interface)
- Analog wind speed sensor (0-1V output to ADC)

**Display & Interface**
- HD44780-compatible 20×4 LCD
- 4-bit parallel interface
- Custom LCD driver library

**Additional Components**
- Breadboard prototyping setup
- Pull-up resistors for I2C bus
- Decoupling capacitors
- Regulated 5V power supply

---

## Technical Implementation

### Sensor Interfacing

**DHT11 (Temperature & Humidity)**
- 1-wire digital protocol
- 40-bit data transmission (humidity + temperature + checksum)
- Custom bit-banging implementation
- Checksum validation for data integrity

**BH1750 (Light Sensor)**
- I2C communication (address: 0x23)
- Continuous high-resolution mode (1 lux resolution)
- Hardware I2C using PIC's MSSP module
- 16-bit light intensity reading

**Wind Speed Sensor**
- Analog voltage output (proportional to wind speed)
- PIC18F4321 ADC (10-bit, right-justified)
- External VREF for accuracy
- Conversion: ADC value → voltage → wind speed

### LCD Driver

Custom LCD library with full HD44780 support:
- 4-bit parallel communication
- Configurable cursor and display modes
- Integer and float display functions
- Custom character support (CGRAM)

**Library Features:**
- `LCD_write_string()` - String display
- `LCD_write_variable()` - Integer display with offset
- `LCD_write_float()` - Floating-point display
- `LCD_cursor_set()` - Position control

### WBGT Calculation & Classification

The system calculates WBGT from sensor inputs and classifies heat stress into five categories:

| WBGT Range (°F) | Classification | Recommendation |
|-----------------|----------------|----------------|
| < 80 | No Risk | Normal activity permitted |
| 80 - 84.9 | Low | 5-min water break every 25 min |
| 85 - 87.9 | Moderate | Frequent breaks, ice pool available |
| 88 - 89.9 | High | Constant observation required |
| ≥ 90 | Extreme | Cancel outdoor practice/work |

---

## Software Architecture

**Main Loop:**
1. Initialize sensors and LCD
2. Read DHT11 (temperature/humidity)
3. Read BH1750 (light intensity)
4. Read ADC (wind speed)
5. Calculate WBGT index
6. Classify heat stress level
7. Update LCD display
8. Delay 2 seconds, repeat

**I2C Communication:**
- Hardware MSSP module configuration
- Start/Stop condition generation
- ACK/NACK handling
- Multi-byte read/write operations

**ADC Configuration:**
- External VREF for precision
- Right-justified 10-bit result
- FOSC/32 conversion clock
- 4 TAD acquisition time

---

## Development Tools

- **IDE:** MPLAB X IDE
- **Compiler:** XC8 (Microchip C Compiler)
- **Programmer:** PICkit 3 (or compatible)
- **Hardware:** Breadboard prototype
- **Oscillator:** Internal 4 MHz RC oscillator

---

## Project Status

**Working Features:**
- ✅ DHT11 temperature and humidity reading
- ✅ ADC-based wind speed measurement
- ✅ LCD display with custom driver
- ✅ WBGT calculation and classification
- ✅ Real-time safety recommendations
- ✅ Multi-level heat stress alerts

**Known Issues:**
- ⚠️ BH1750 light sensor intermittent communication (I2C timing issue)
- ⚠️ Occasional DHT11 checksum errors in high-humidity conditions

**Future Improvements:**
- Debug I2C timing for reliable BH1750 operation
- Add data logging to EEPROM
- Implement audible alarm for extreme conditions
- Calibrate sensors against commercial WBGT monitor
- Add battery power and portable enclosure

---

## Pin Configuration

| PIC Pin | Function | Connection |
|---------|----------|------------|
| RA4-RA7 | LCD D4-D7 | LCD data lines |
| RD0 | LCD_RS | LCD register select |
| RD2 | LCD_EN | LCD enable |
| RC1 | DHT11 Data | Temperature/humidity sensor |
| RC3/RC4 | I2C SCL/SDA | BH1750 light sensor |
| AN0 | ADC Input | Wind speed sensor |

---

## Code Structure

```
3301_Final_Project/
├── final_project.c          # Main application code
├── LCD.c                     # Custom LCD driver implementation
├── LCD.h                     # LCD driver header
├── pic18f4321-Config.h      # PIC configuration bits
└── README.md
```

---

## Learning Outcomes

This project provided hands-on experience with:

**Embedded Systems Programming**
- PIC18F microcontroller architecture
- XC8 compiler and MPLAB X workflow
- Bit manipulation and register configuration
- Real-time embedded system design

**Communication Protocols**
- I2C master mode implementation
- Custom 1-wire protocol (DHT11)
- 4-bit parallel LCD interface
- Protocol timing and signal integrity

**Sensor Integration**
- Multi-sensor system design
- ADC configuration and calibration
- Digital sensor interfacing
- Data validation and error handling

**Application Development**
- Safety-critical system design
- User interface design (LCD menus)
- Threshold-based alert systems
- Real-world measurement applications

---

## Testing & Validation

**Sensor Verification:**
- Compared DHT11 readings against calibrated thermometer
- Validated humidity readings in controlled environment
- Cross-referenced light sensor with smartphone lux meter

**System Testing:**
- Tested in various environmental conditions
- Verified WBGT calculations against published tables
- Confirmed safety recommendations match OSHA guidelines

---

## References

**WBGT Standards:**
- OSHA Technical Manual: Heat Stress
- American College of Sports Medicine (ACSM) Guidelines
- Military Heat Stress Prevention Standards

**Datasheets:**
- PIC18F4321 Datasheet (Microchip)
- DHT11 Humidity & Temperature Sensor Datasheet
- BH1750 Digital Light Sensor Datasheet
- HD44780 LCD Controller Datasheet

**Application Notes:**
- Microchip AN1142: Using the MSSP Module (I2C)
- Microchip AN546: Practical ADC Usage

---

## Acknowledgments

**Course:** ECE 3301 - Microcontroller Systems  
**Instructor:** Sasoun Torousian  
**Partner:** Isaias Rafael-Sepulveda  
**Institution:** California Polytechnic State University, Pomona

**Third-Party Code:**
- LCD library based on work by Ahmet Burak Irmak (MIT License)

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Contact

**Daniel Romero**  
Email: daniel.romero@ieee.org  
Portfolio: [electricalromero.com](https://electricalromero.com)  
LinkedIn: [linkedin.com/in/daniel-romero-ee](https://www.linkedin.com/in/daniel-romero-ee/)

---

*A practical embedded systems project demonstrating sensor integration, I2C communication, and real-world safety monitoring applications.*

*Last Updated: December 2025*
