---
layout: default
title: Specifications
parent: Hardware
nav_order: 3
---

# Technical Specifications
{: .no_toc }

Complete technical specifications for the AeroCogito H7-Digital flight controller.
{: .fs-6 .fw-300 }

---

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Processor

### Main Microcontroller

| Specification | Details |
|---------------|---------|
| **Model** | STM32H743VIT6 |
| **Architecture** | ARM Cortex-M7 |
| **Clock Speed** | 480MHz |
| **Flash Memory** | 2MB on-chip |
| **RAM Total** | 1MB |

{: .note }
> The STM32H743 is one of the most powerful microcontrollers available for flight control applications, providing ample processing headroom for advanced features like companion computer communication, complex flight modes, and high-rate sensor fusion.

---

## Sensors

### IMU: ICM-42688-P

| Specification | Details |
|---------------|---------|
| **Type** | 6-axis (3-axis gyro + 3-axis accelerometer) |
| **Manufacturer** | TDK InvenSense |
| **Interface** | SPI1 (high-speed) |
| **Gyro Range** | ±15.625 to ±4000 dps (8 programmable settings) |
| **Gyro Resolution (Standard)** | 16-bit ADC (0.0610 dps/LSB at ±2000 dps) |
| **Gyro Resolution (High-Res)** | 19-bit FIFO (0.0076 dps/LSB at ±2000 dps) - **8x improvement** |
| **Accel Range** | ±2g to ±32g (4 programmable settings) |
| **Accel Resolution (Standard)** | 16-bit ADC (0.488 mg/LSB at ±16g) |
| **Accel Resolution (High-Res)** | 18-bit FIFO (0.122 mg/LSB at ±16g) - **4x improvement** |
| **Max ODR** | 32 kHz (gyro and accel) hardware maximum |
| **Firmware ODR** | Gyro: 8 kHz (Betaflight), 8 kHz fast sampling → 1 kHz backend (ArduPilot) / Accel: 1 kHz (both) |
| **Power Supply** | Isolated 3.3V rail with enhanced filtering |

**Key Features**:

**Performance Specifications**:
- Industry-leading low noise (2.8 mdps/√Hz gyro, 65-70 µg/√Hz accel)
- Excellent temperature stability (±5 mdps/°C gyro offset)
- Extended measurement ranges (±4000 dps gyro, ±32g accel)

**Resolution Modes** (ArduPilot vs Betaflight):
- **Industry-first 20-bit FIFO format** (19-bit gyro + 18-bit accel + fractional bits)
- **High-resolution mode** (ArduPilot): 19-bit gyro, 18-bit accel via FIFO - 8x/4x better resolution
- **Standard 16-bit mode** (Betaflight): Direct register reads for minimum latency

**Advanced Features**:
- On-chip FIFO buffer (2048 bytes, ~105 high-res samples or 128 standard samples)
- Real-Time Clock (RTC) for sample synchronization
- Configurable Anti-Alias Filter (AAF): 258Hz, 536Hz, 997Hz, 1962Hz cutoff frequencies

**H7-Digital Hardware Enhancements**:
- Isolated 3.3V power rail eliminates electrical noise
- CLKIN support (Betaflight): 32 kHz external clock on PE5 for improved timing accuracy

{: .tip }
> The ICM-42688-P is considered one of the best IMUs for flight control, offering exceptional noise performance and stability. The isolated power rail on the H7-Digital further enhances performance by greatly reducing power supply noise.

{: .note }
> **Firmware Defaults**: Both Betaflight and ArduPilot use **±2000 dps gyro** and **±16g accelerometer** ranges (not the full ±4000 dps / ±32g hardware maximum). See [Sensors - Understanding Programmable Range Modes]({{ site.baseurl }}/docs/hardware/sensors#understanding-programmable-range-modes) for detailed explanation of why these settings provide the optimal balance between resolution and range.

**Driver References:**
- [ArduPilot Invensensev3 Driver](https://github.com/ArduPilot/ardupilot/blob/master/libraries/AP_InertialSensor/AP_InertialSensor_Invensensev3.cpp) - Lines 234-235: `GYRO_SCALE_2000DPS` and `ACCEL_SCALE_16G` defaults; Lines 267-270: high-resolution modes (`GYRO_SCALE_HIGHRES_2000DPS`, `ACCEL_SCALE_HIGHRES_16G`)
- [Betaflight ICM426XX Driver](https://github.com/betaflight/betaflight/blob/master/src/main/drivers/accgyro/accgyro_spi_icm426xx.c) - Lines 413-421: `GYRO_SCALE_2000DPS` and 16g accel configuration; Lines 52-58: 24 MHz SPI max, 32 kHz CLKIN; Lines 145-151: AAF filter options
- [ICM-42688-P Datasheet](https://product.tdk.com/system/files/dam/doc/product/sensor/mortion-inertial/imu/data_sheet/ds-000347-icm-42688-p-v1.6.pdf) - TDK InvenSense official datasheet (Section 4.1: Programmable Full-Scale Range)

For sensor mounting, calibration procedures, and high-resolution sampling details, see [Sensors]({{ site.baseurl }}/docs/hardware/sensors).

### Barometer: DPS368

| Specification | Details |
|---------------|---------|
| **Type** | High-precision capacitive pressure sensor |
| **Manufacturer** | Infineon |
| **Interface** | I2C1 (internal bus) |
| **I2C Address** | 0x76 |
| **Pressure Range** | 300-1200 hPa |
| **Pressure Resolution** | 0.06 Pa RMS (24-bit output) |
| **Pressure Accuracy** | ±0.002 hPa (±0.02m) in high precision mode |
| **Absolute Accuracy** | ±1 hPa (±8m) |
| **Hardware Max Rate** | 200 Hz |
| **Firmware Sample Rate** | 32 Hz (both firmwares, 16x oversampling) |
| **Temperature Range** | -40°C to +85°C |
| **Package** | Ruggedized, waterproof (IPx8 certified - 50m for 1hr) |

**Key Features**:
- Extremely low noise (±0.02m altitude resolution)
- High measurement rate (up to 200Hz)
- Integrated 32-measurement FIFO buffer
- On-chip temperature compensation
- Capacitive sensing for high precision during temperature changes
- Low power consumption (1.7 μA @ 1Hz, 0.5 μA standby)

{: .note }
> **Driver Compatibility**: The DPS368 is register-compatible with the DPS310 and uses the same driver code in both ArduPilot and Betaflight. The primary difference is the ruggedized, waterproof package (IPX8 certified). The DPS368 maintains the same communication protocol and register map as the DPS310.

**Driver References:**
- [ArduPilot AP_Baro_DPS280 Driver](https://github.com/ArduPilot/ardupilot/blob/master/libraries/AP_Baro/AP_Baro_DPS280.cpp) - Supports DPS280/DPS310/DPS368, configured for 32 Hz sampling with 16x oversampling
- [Betaflight barometer_dps310.c Driver](https://github.com/betaflight/betaflight/blob/master/src/main/drivers/barometer/barometer_dps310.c) - `USE_BARO_DPS310`, configured for 32 Hz sampling with 16x oversampling, I2C address 0x76
- [DPS368 Datasheet](https://www.infineon.com/dgdl/Infineon-DPS368-DataSheet-v01_01-EN.pdf?fileId=5546d46269e1c019016a0c45105d4b40) - Infineon official datasheet

### External Sensors (Not Included)

**GPS Module** (via GPS connector):
- Connection: UART8 + I2C4
- Recommended: u-blox M8N/M9N or better
- Protocols: NMEA, UBX
- Compass: External compass via I2C (required for ArduPilot)

---

## Physical Specifications

| Dimension | Measurement |
|-----------|-------------|
| **Length** | 41mm |
| **Width** | 41mm |
| **Height** | 8mm (max component height) |
| **Mounting Holes** | M3 (4 holes) |
| **Mounting Pattern** | 30.5mm |
| **Weight** | 12g (without cables) |

{: .note }
> Rubber dampers (included) provide vibration isolation while maintaining secure mounting.

---

## Power System

### Input Power

| Parameter | Specification |
|-----------|---------------|
| **Input Voltage Range** | 9.9V - 25.2V (3S-6S LiPo) |
| **Absolute Maximum** | 28V |
| **Minimum Operating** | 8V |
| **Typical Voltage** | 16.8V (4S LiPo) or 25.2V (6S LiPo) |
| **Input Connector** | Via ESC connector (VBAT pin) |

### 5V Buck Regulator (BEC)

| Parameter | Specification |
|-----------|---------------|
| **Output Voltage** | 5.0V |
| **Continuous Current** | 2A |
| **Peak Current** | 2.5A (<10s) |
| **Efficiency** | ~85% typical |
| **Output Ripple** | ~10mV worst-case |

**Powers**:
- Flight controller logic
- GPS/compass module (4.5V)
- RC receiver (4.5V)
- Servo outputs (5V)

### 10V Buck Regulator (VTX BEC)

| Parameter | Specification |
|-----------|---------------|
| **Output Voltage** | 10.0V |
| **Continuous Current** | 2A |
| **Peak Current** | 2.5A (<10s) |
| **Efficiency** | ~85% typical |
| **Output Ripple** | ~10mV worst-case |
| **Software Control** | Yes (**ArduPilot** RELAY2 (GPIO 80), **Betaflight** PINIO1 `10V BEC`) |
| **Default State** | ON |
| **Output Connector** | VTX connector only |

**Powers**:
- Digital VTX only (DJI, Walksnail, HDZero, OpenIPC)

{: .warning }
> **10V Output for VTX Only**
>
> The 10V output is designed exclusively for video transmitters. Current is rated to 2A continuous - 2.5A Burst. Below ~10.5V VIN, the VTX BEC tracks VIN (pass-through).

### Battery Monitoring

| Parameter | Specification |
|-----------|---------------|
| **Voltage Sensing** | Yes |
| **Voltage Range** | 0-28V |
| **Voltage Resolution** | 12-bit ADC (4096 steps) |
| **Voltage Pin** | PA0 (ADC1) |
| **Current Sensing** | Yes |
| **Current Range** | 0-180A (default scale) |
| **Current Resolution** | 12-bit ADC (4096 steps) |
| **Current Pin** | PA1 (ADC1) |
| **Sensing Location** | Via **T** on the ESC connector |

**Default Scales**:
- Voltage: 11.0:1 divider
- Current: 18.0 A/V scale

{: .note }
> Battery monitoring scales may need calibration for your specific build. See [ArduPilot Power Module Configuration](https://ardupilot.org/copter/docs/common-power-module-configuration-in-mission-planner.html) or Betaflight Configurator's Power tab for calibration procedures.

---

## Motor Outputs

**10 PWM outputs**: 8 motor outputs (M1-M8) + 2 servo outputs (S1-S2)

**Output Groups**:
- **Group 1** (M1-M4): TIM1, all must use same protocol
- **Group 2** (M5-M8): TIM4, all must use same protocol
- **Group 3** (S1-S2): TIM3, all must use same protocol

**Bidirectional DShot Support**:
- **Betaflight**: M1-M7 supported (M8 lacks DMA channel)
- **ArduPilot**: M1, M3, M5, M7 only (timer CH1/CH3 limitation)

For complete motor pin assignments, timer channels, and DMA configuration, see [Pinout - Motor Outputs]({{ site.baseurl }}/docs/hardware/pinout#motor-outputs-m1-m8).

### Supported Protocols

| Protocol | Motors 1-8 | Servos 1-2 | Notes |
|----------|------------|-------------|-------|
| **DShot600** | ✓ | ✓ | Recommended (most tested, required for BiDir) |
| **DShot300** | ✓ | ✓ | Large aircraft / long cables (BiDir compatible) |
| **DShot150** | ✓ | ✓ | Maximum noise immunity (no BiDir support) |
| **DShot1200** | ArduPilot only | ArduPilot only | Not recommended - see warning below |
| **OneShot125** | ✓ | ✓ | Legacy protocol |
| **OneShot42** | ✓ | ✓ | Legacy protocol |
| **Multishot** | ✓ | ✓ | Legacy protocol |
| **PWM (50-490Hz)** | ✓ | ✓ | For servos |
| **Bidirectional DShot (Betaflight)** | M1-M7 | ✗ | M8 has no DMA |
| **Bidirectional DShot (ArduPilot)** | M1,M3,M5,M7 | ✗ | Timer CH1/CH3 only |

{: .tip }
> **DShot Protocol Selection Guide**:
>
> - **DShot600**: Recommended for most vehicles. Most widely tested and used. Ties up DMA channels for less time. **Required for bidirectional DShot on most systems.**
> - **DShot300**: Recommended for larger aircraft with longer cable runs. Less susceptible to noise than DShot600. **Also supports bidirectional DShot.**
> - **DShot150**: Best for very long cable runs (larger aircraft). Most noise-resistant. **Does NOT support bidirectional DShot.**
>
> **Bidirectional DShot Requirements**: Requires DShot300 or higher (longer pulse width needed to wait for ESC response). DShot150 is too slow for bidirectional operation. Only supported on BLHeli_32 or AM32 ESCs. Set `MOT_PWM_TYPE` to 4 (DShot300), 5 (DShot600), or 6 (DShot1200) in ArduPilot, though DShot300/600 are recommended.
>
> For comprehensive comparison of ESC telemetry methods (UART, Bidirectional DShot, Extended DShot Telemetry), see [Understanding ESC Telemetry Methods]({{ site.baseurl }}/docs/hardware/pinout#understanding-esc-telemetry-methods).

{: .warning }
> **DShot1200 Not Recommended**:
>
> While the STM32H743 hardware supports DShot1200, **it is not recommended** for the following reasons:
>
> **Betaflight**: DShot1200 was **officially removed in Betaflight 4.1** and is no longer available. DShot600 is the maximum supported protocol for 8kHz gyro/PID loops. Reasons for removal:
> - Only needed for 32kHz looptime (no longer supported - max is 8kHz)
> - Not stable with bidirectional DShot (required for RPM filtering)
> - Poor ESC firmware support (BLHeli_S, BLHeli_32)
>
> **ArduPilot**: DShot1200 is technically supported (`MOT_PWM_TYPE = 6`) but **strongly discouraged**:
> - Much less tested than DShot600 (limited real-world validation)
> - More susceptible to noise and signal integrity issues
> - Ties up DMA channels for marginally less time than DShot600 (minimal benefit)
> - No performance benefit for standard flight controller operations
>
> **If you experience motor issues with DShot600, downgrade to DShot300, NOT DShot1200.** Use DShot1200 only if you have a specific technical requirement and understand the risks.

{: .important }
> **Output Group Restrictions**
>
> All outputs in the same group must use the same protocol:
> - **Group 1** (M1-M4): Must all use the same protocol
> - **Group 2** (M5-M8): Must all use the same protocol
> - **Group 3** (S1-S2): Must all use the same protocol
>
> Example: If M1 uses DShot600, then M2, M3, and M4 must also use DShot600.

---

## Communication Interfaces

### UART Ports

**6 UART ports** available with configurable baud rates up to 1.5 Mbps (ArduPilot) / 1 Mbps (Betaflight):

| Port | Default Use | Connector |
|------|-------------|-----------|
| **UART2** | VTX (DisplayPort MSP) | VTX connector |
| **UART3** | ESC Telemetry (RX only) | ESC connector pin 4 |
| **UART4** | RC Input (SBUS/CRSF/ELRS) | RC INPUT connector |
| **UART5** | Companion Computer (MAVLink2) | COMPUTER connector |
| **UART7** | DJI SBUS (RX only) | VTX connector R7 pin |
| **UART8** | GPS | GPS connector |

For complete UART mapping with TX/RX pin assignments, DMA configuration, and connector pinouts, see [Pinout - UART Mapping]({{ site.baseurl }}/docs/hardware/pinout#uart-mapping).

{: .tip }
> UART5 is optimized for high-bandwidth companion computer communication (Raspberry Pi, Jetson).

### I2C Ports

| Port | SCL Pin | SDA Pin | Hardware Support | Firmware Default | Usage |
|------|---------|---------|------------------|------------------|-------|
| **I2C1** | PB8 | PB9 | Up to 1MHz (Fm+) | 400kHz (ArduPilot), 800kHz (Betaflight) | Internal (Barometer) |
| **I2C4** | PB6 | PB7 | Up to 1MHz (Fm+) | 400kHz (ArduPilot), 800kHz (Betaflight) | External (Compass) |

**Hardware Capabilities**:
- **Standard Mode**: 100 kHz
- **Fast Mode (Fm)**: 400 kHz
- **Fast Mode Plus (Fm+)**: 1 MHz (STM32H743 hardware supported)

**Firmware Defaults**:
- **ArduPilot**: 400 kHz (Fast Mode) - conservative, broadly compatible
- **Betaflight**: 800 kHz default - can be adjusted via CLI (`i2c1_clockspeed_khz`, `i2c4_clockspeed_khz`)

**Features**:
- 3.3V logic levels
- On-board pull-up resistors
- I2C4 available on GPS connector for external compass
- Configurable speed for sensor compatibility

{: .note }
> **I2C Speed and Sensor Compatibility**: The DPS368 barometer supports up to 3.4 MHz (High-Speed mode per I2C spec v2.1). External compass modules typically support up to 400 kHz. If experiencing sensor detection issues in Betaflight, try reducing I2C speed: `set i2c4_clockspeed_khz = 400` in CLI.

### SPI Ports

| Port | SCK | MISO | MOSI | CS | Usage |
|------|-----|------|------|----|-------|
| **SPI1** | PA5 | PA6 | PA7 | PB0 | Internal (IMU) |

**Features**:
- High-speed SPI communication to IMU
  - **ArduPilot**: 16 MHz (2 MHz initialization, 16 MHz operation)
  - **Betaflight**: 24 MHz maximum (default, configurable via `ICM426XX_CLOCK`)
  - **ICM-42688-P Hardware Maximum**: 24 MHz SPI clock
- DMA support for efficient CPU usage
- Hardware CS control

{: .note }
> **SPI Frequency vs IMU ODR**: The SPI bus frequency (how fast the MCU communicates with the IMU chip) is independent from the IMU Output Data Rate/ODR (how often the sensor generates new samples). ArduPilot uses 16 MHz SPI with 1 kHz gyro backend, while Betaflight uses 24 MHz SPI with 8 kHz gyro sampling.

### USB

| Specification | Details |
|---------------|---------|
| **Type** | USB-C connector |
| **Standard** | USB 2.0 Full Speed |
| **Speed** | 12 Mbps |
| **Power** | Bus-powered (5V input) or self-powered |
| **Protocol** | Virtual COM port (VCP) |

**Uses**:
- Firmware flashing
- Configuration (Mission Planner, QGC, Betaflight Configurator)
- Telemetry
- Parameter adjustment
- Log download

---

## Storage

### MicroSD Card Slot

| Specification | Details |
|---------------|---------|
| **Interface** | SDMMC1 (4-bit mode) |
| **Speed** | Up to 50MB/s (UHS-I) |
| **Capacity** | Up to 256GB tested |
| **Card Type** | MicroSD, MicroSDHC, MicroSDXC |
| **Recommended** | Class 10 or UHS-I |
| **Voltage** | 3.3V |

**Uses**:
- Blackbox logging (Betaflight)
- DataFlash logging (ArduPilot)
- Terrain data storage (ArduPilot)
- Firmware updates (.abin files)
- Parameter backup

{: .tip }
> Use a quality Class 10 or UHS-I card for reliable logging. Cheap or counterfeit cards may fail during high-write-rate logging operations.

---

## GPIO and Special Functions

### LEDs

| LED | Color | Pin | Location (By USB port) | Preview |
|-----|-------|-----|------------------------|---------|
| LED0 | Blue | PD8 | Left | ![]({{ site.baseurl }}/assets/images/led/01_blue_solid.gif) |
| LED1 | Green | PB15 | Center | ![]({{ site.baseurl }}/assets/images/led/10_green_solid.gif) |
| LED2 | Amber | PB14 | Right | ![]({{ site.baseurl }}/assets/images/led/09_amber_solid.gif) |

#### ArduPilot LED Quick Reference

| LED Color / Pattern       | Meaning                                 | Preview |
|---------------------------|-----------------------------------------|---------|
| Blue solid                | Armed                                   | ![]({{ site.baseurl }}/assets/images/led/01_blue_solid.gif) |
| Blue single flash         | Disarmed, pre-arm OK                    | ![]({{ site.baseurl }}/assets/images/led/02_blue_single_flash.gif) |
| Blue double flash         | Disarmed, pre-arm failure               | ![]({{ site.baseurl }}/assets/images/led/03_blue_double_flash.gif) |
| Blue/Amber alternating    | Initialising (boot)                     | ![]({{ site.baseurl }}/assets/images/led/04_blue_amber_alternating.gif) |
| Blue → Green → Amber chase| Save trim / ESC calibration in progress | ![]({{ site.baseurl }}/assets/images/led/05_bga_chase.gif) |
| Amber off                 | No GPS detected                         | ![]({{ site.baseurl }}/assets/images/led/06_all_off.gif) |
| Amber fast blink (~2 Hz)  | GPS present, no lock                    | ![]({{ site.baseurl }}/assets/images/led/07_amber_fast_blink.gif) |
| Amber slow blink (~1 Hz)  | 2D lock (incomplete)                    | ![]({{ site.baseurl }}/assets/images/led/08_amber_slow_blink.gif) |
| Amber solid               | GPS 3D lock or better                   | ![]({{ site.baseurl }}/assets/images/led/09_amber_solid.gif) |

{: .tip }
> **Viewing Detailed Pre-Arm Messages**: Connect to Mission Planner or QGroundControl to see specific pre-arm failure reasons. The ground station displays exact error messages that the LEDs cannot convey.

#### Betaflight LED Quick Reference

See [Betaflight FC-LEDs Documentation](https://www.betaflight.com/docs/development/FC-LEDs) for complete LED status codes.

| LED Color / Pattern | Meaning | Preview |
|---------------------|---------|---------|
| Blue solid | **Armed** (motors can spin) | ![]({{ site.baseurl }}/assets/images/led/01_blue_solid.gif) |
| Blue flashing | Warning condition, serial/ESC passthrough, or USB MSC activity | ![]({{ site.baseurl }}/assets/images/led/02_blue_single_flash.gif) |
| Blue/Green flashing 5 times | Initialising (boot) | ![]({{ site.baseurl }}/assets/images/led/12_blue_green_flash.gif) |
| Amber normally on | Normal operation (default state) | ![]({{ site.baseurl }}/assets/images/led/09_amber_solid.gif) |
| Flash pattern (1-16 pulses) | Hardware fault / error code - see [Betaflight FC-LEDs Documentation](https://www.betaflight.com/docs/development/FC-LEDs) for specific codes | ![]({{ site.baseurl }}/assets/images/led/13_all_flash.gif) |

{: .tip }
> **Viewing Detailed Arming Issues**: Connect to Betaflight Configurator and check the CLI `status` command to see specific arming disable flags.

### Boot Button

| Feature | Details |
|---------|---------|
| **Pin** | BOOT0 |
| **Function** | Enter DFU mode |
| **Usage** | Hold during power-on for firmware flashing |
| **Notes** | Only amber LED will illuminate in DFU mode |
| **Preview** | ![]({{ site.baseurl }}/assets/images/led/09_amber_solid.gif) |

### VTX Power Control

| Feature | Details |
|---------|---------|
| **Pin** | PE2 |
| **Function** | 10V BEC enable/disable |
| **Default** | ON (HIGH) |
| **ArduPilot** | RELAY2 (GPIO: 80) |
| **Betaflight** | PINIO1 `10V BEC`|
| **Usage** | RC switch or mission command control |

{: .note }
> **Firmware-Specific Configuration**:
>
> - **ArduPilot**: Configure as `RELAY2` (GPIO 80). Assign to RC channel via `RCx_OPTION = 34` (Relay2 On/Off) or use in missions with `DO_SET_RELAY` command.
> - **Betaflight**: Configure as `PINIO1` in the Resource Remapping tab or via CLI: `resource PINIO 1 E02`. Map to switch via `pinio_box` setting.

---

## Environmental Specifications

### Operating Conditions

| Parameter | Range |
|-----------|-------|
| **Operating Temperature** | -40°C to +85°C |
| **Storage Temperature** | -40°C to +100°C |
| **Humidity** | 5% to 95% (non-condensing) |

{: .note }
> The H7-Digital is rated for industrial-grade operating conditions (-40°C to +85°C). All components (STM32H743, ICM-42688-P, DPS368) are specified for this temperature range.

{: .warning }
> **LiPo Battery Cold Weather Considerations**: While the H7-Digital can operate down to -40°C, LiPo battery performance degrades significantly at low temperatures. Batteries below 15°C cannot sustain high power loads and may show voltage sag before takeoff. Consider battery heating for cold-weather operations below -10°C.

---

## Compliance and Certifications

| Standard | Status |
|----------|--------|
| **NDAA Section 848** | Operating Software compliant (with secure firmware) |
| **DIU Blue List Framework** | Compliant (Pending) |

{: .note }
> The H7-Digital hardware is designed, manufactured, and assembled in the USA with an auditable supply chain for NDAA and DIU Blue List Framework compliance. Contact [support@aerocogito.com](mailto:support@aerocogito.com) for compliance documentation.

---

## Pin Capabilities Summary

### High-Level Pin Count

| Feature | Count |
|---------|-------|
| **PWM Outputs** | 10 (M1-M8 + S1-S2) |
| **UART Ports** | 6 (2/3/4/5/7/8) |
| **I2C Ports** | 2 (1 internal, 1 external) |
| **SPI Ports** | 1 (IMU) |
| **ADC Inputs** | 2 (voltage, current) |
| **GPIO** | 1 (VTX control) |
| **USB** | 1 (USB-C) |
| **SD Card** | 1 (MicroSD) |

---

## Performance Benchmarks

### Sensor Update Rates

| Sensor | Hardware Max ODR | Firmware Default | Typical Usage |
|--------|------------------|------------------|---------------|
| **Gyroscope (ICM-42688-P)** | 32kHz | 8kHz (BF) / 1kHz (AP) | 8kHz (Betaflight), 1kHz (ArduPilot) |
| **Accelerometer (ICM-42688-P)** | 32kHz | 1kHz | 1kHz both firmwares |
| **Barometer (DPS368)** | 200Hz | 32Hz | 32Hz both firmwares |
| **GPS** | Sensor dependent | 5-10Hz | 5-10Hz (depends on GPS module) |

{: .note }
> **ICM-42688-P Maximum ODR**: The ICM-42688-P has a hardware maximum output data rate (ODR) of **32 kHz** for both gyroscope and accelerometer (with RTC support). However, firmware implementations use lower rates: Betaflight samples at 8 kHz (gyro) / 1 kHz (accel), while ArduPilot uses 1 kHz backend with 8 kHz fast sampling for the gyro.

{: .note }
> **Fast Sampling & High-Resolution Modes Explained**:
>
> **ArduPilot**: The ICM-42688-P supports ArduPilot's fast sampling feature where raw sensor data is collected at 8kHz internally using high-resolution modes (`GYRO_SCALE_HIGHRES_2000DPS`, `ACCEL_SCALE_HIGHRES_16G`), then averaged and downsampled to the configured gyro backend rate (controlled by `INS_GYRO_RATE`: 0=1kHz, 1=2kHz, 2=4kHz). This prevents aliasing of high-frequency vibrations while providing enhanced resolution from the 19-bit/18-bit FIFO data. The gyro filtering (notch/low-pass) runs at the backend rate, not the raw 8kHz. This architecture provides cleaner data for autonomous flight while managing CPU load. See: [ArduPilot IMU Batch Sampling](https://ardupilot.org/copter/docs/common-imu-batchsampling.html)
>
> **Betaflight**: Reads the ICM-42688-P at its native 8kHz rate (configurable to 4kHz, 2kHz, 1kHz) and runs all gyro filtering at the same rate (no downsampling). Uses standard 16-bit ADC mode with configurable Anti-Alias Filter (AAF) cutoff frequencies (258Hz, 536Hz, 997Hz, or 1962Hz). This provides minimum latency for racing/freestyle.

### Loop Rates

| Firmware | Main Loop | PID/Attitude Loop | EKF Loop | Gyro Backend Rate | Notes |
|----------|-----------|-------------------|----------|-------------------|-------|
| **Betaflight** | 8kHz | 8kHz | N/A | 8kHz | Gyro and PID locked to same rate (configurable 1-8kHz) |
| **ArduPilot** | 400Hz (default) | 400Hz (default) | 400Hz | 1kHz (default) | Configurable via `SCHED_LOOP_RATE` and `INS_GYRO_RATE` |

{: .note }
> **ArduPilot Loop Rates**: ArduPilot supports faster attitude rates on H7 hardware. The `INS_GYRO_RATE` parameter allows gyro backend rates of 1kHz (default), 2kHz, or 4kHz. The `SCHED_LOOP_RATE` parameter controls main loop rate (default 400Hz, experimental up to 2kHz).

{: .note }
> **Why ArduPilot Uses 1kHz Default**: ArduPilot prioritizes reliability, CPU headroom for advanced features (EKF, missions, companion computers), and broad hardware compatibility. The 1kHz gyro backend rate is sufficient for most autonomous operations, mapping, surveying, and larger aircraft. Increasing to 2kHz or 4kHz is only beneficial for small, agile racing/freestyle quads and requires H7-class hardware with available CPU cycles. Most ArduPilot users should keep the default 1kHz setting.

{: .tip }
> The STM32H743 has more than enough processing power to handle maximum loop rates with CPU usage typically below 30%. This leaves plenty of headroom for companion computer communication and advanced features.

---

## Related Documentation

- [Pinout & Connectors]({{ site.baseurl }}/docs/hardware/pinout) - Detailed pin assignments and connector pinouts
- [Power System]({{ site.baseurl }}/docs/hardware/power) - Power architecture details
- [Sensors]({{ site.baseurl }}/docs/hardware/sensors) - Sensor details and calibration

---

## Support

For specification clarifications or technical questions:

- **Email**: [support@aerocogito.com](mailto:support@aerocogito.com)

---

[← Back to Hardware]({{ site.baseurl }}/docs/hardware/){: .btn }
[Pinout Reference →]({{ site.baseurl }}/docs/hardware/pinout){: .btn .btn-purple }
