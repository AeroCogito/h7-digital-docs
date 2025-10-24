---
layout: default
title: Choosing Firmware
parent: Getting Started
nav_order: 2
---

# Choosing Your Firmware
{: .no_toc }

Understanding the differences between ArduPilot and Betaflight to make the right choice for your application.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## The Two Options

The H7-Digital supports two popular open-source flight control firmware options:

### ArduPilot
Open-source autopilot system designed for autonomous vehicles. Excellent for waypoint missions, companion computer integration, and professional UAS operations.

**→** [Jump to ArduPilot Installation](#installing-ardupilot)

### Betaflight
High-performance flight controller firmware optimized for manual FPV flight, racing, and freestyle. Industry standard for acrobatic multirotor flight.

**→** [Jump to Betaflight Installation](#installing-betaflight)

---

## Quick Decision Guide

### Choose ArduPilot if you need:
- ✅ **Autonomous flight** with waypoint missions
- ✅ **GPS navigation** and robust return-to-home
- ✅ **Companion computer** integration (Raspberry Pi, Jetson)
- ✅ **Mission planning** software (QGroundControl, Mission Planner)
- ✅ **NDAA compliance** with secure signed firmware
- ✅ **Professional operations** requiring audit trails
- ✅ **Advanced sensors** (optical flow, rangefinders, RTK GPS)
- ✅ **MAVLink protocol** for telemetry and control

### Choose Betaflight if you need:
- ✅ **Manual FPV flight** with precise control
- ✅ **Racing and freestyle** acrobatics
- ✅ **Simple configuration** via GUI
- ✅ **Quick setup** and tuning
- ✅ **Blackbox logging** for PID tuning
- ✅ **OSD customization** for FPV display
- ✅ **No GPS required** for operation (but useful for GPS rescue functionality)
- ✅ **Active racing community** and support

---

## Detailed Comparison

| Feature | ArduPilot | Betaflight |
|---------|-----------|------------|
| **Primary Use Case** | Autonomous/semi-autonomous flight | Manual FPV racing/freestyle |
| **GPS Requirement** | Required for auto modes (except guided-no-gps) | Optional, used for GPS rescue |
| **Companion Computer** | Excellent (MAVLink, DroneCAN) | Limited support |
| **Configuration Method** | Parameter files, GCS software | Betaflight Configurator GUI |
| **Learning Curve** | Moderate to steep | Beginner to moderate |
| **Mission Planning** | Full support (QGC, MP) | Not applicable |
| **Acrobatic Flight** | Basic flip/roll modes | Industry-leading performance |
| **Tuning Complexity** | Many parameters, PIDs, auto-tune available | Streamlined |
| **Failsafe Options** | Extensive (RTH, land, loiter) | Basic (motor stop, GPS rescue) |
| **Telemetry** | MAVLink (rich data) | MSP, CRSF telemetry |
| **OSD Support** | Built-in (various styles) | Highly customizable |
| **NDAA Compliance** | Yes (secure firmware builds) | Standard open-source |
| **Community** | Professional/hobbyist mix | Primarily FPV racing |
| **Update Frequency** | Stable releases (quarterly) | Stable releases (semi-annually) |

---

## Learn More

For detailed technical specifications and capabilities:

**ArduPilot**: [Flight modes](https://ardupilot.org/copter/docs/flight-modes.html) • [Advanced features](https://ardupilot.org/copter/docs/common-advanced-configuration.html) • [Companion computers](https://ardupilot.org/dev/docs/companion-computers.html)

**Betaflight**: [Features overview](https://www.betaflight.com/docs/wiki/getting-started) • [Flight performance](https://betaflight.com/docs/development/PID-tuning) • [OSD setup](https://betaflight.com/docs/wiki/configurator/osd-tab)

---

## NDAA Compliance Considerations

If you require NDAA Section 848 compliance:

### ArduPilot with Secure Firmware
- ✅ **Signed firmware builds** from AeroCogito repository
- ✅ **Verifiable supply chain** for software
- ✅ **Audit logs** for all operations

**→** [Download Secure Firmware](https://github.com/AeroCogito/h7-digital-firmware)

### Betaflight Standard Builds
- ⚠️ **Community builds** (no official signed firmware)
- ⚠️ **Limited audit trail** capabilities

{: .note }
> For federal/defense use requiring NDAA compliance, ArduPilot with secure firmware is the recommended option.

---

## Installing Firmware

Once you've decided which firmware to use, follow the installation instructions below.

{: .important }
> **First Time Setup**: The H7-Digital ships with Betaflight pre-installed. If you want to use ArduPilot instead, you'll need to flash the ArduPilot firmware first.

---

## Installing ArduPilot

### Step 1: Download Firmware

Choose one of the following firmware sources:

**Option A: AeroCogito Secure Firmware (NDAA Compliant)**

{: .note }
> **Recommended for federal/defense use**: Cryptographically signed firmware for NDAA Section 848 compliance.

1. Visit the [AeroCogito H7-Digital Firmware Repository](https://github.com/AeroCogito/h7-digital-firmware)
2. Download the latest firmware file from **Releases**:
   - **First-time installation**: Download the `*-with-bootloader-signed.hex` file (includes bootloader)
   - **Subsequent updates**: Download the `*-signed.apj` file (faster, no bootloader needed)
3. **Optional**: Verify the cryptographic signature (instructions in repository README)

**Option B: Official ArduPilot Firmware**

{: .note }
> **Standard open-source builds**: Latest official ArduPilot firmware from the ArduPilot project.

1. Visit the [ArduPilot Firmware Server](https://firmware.ardupilot.org/Copter/latest/AeroCogito-H7Digital/)
2. Download firmware file:
   - **First-time installation**: Download `arducopter_with_bl.hex` (includes bootloader)
   - **Subsequent updates**: Download `arducopter.apj` (no bootloader needed)

### Step 2: Install Ground Control Software

Choose one of the following ground control stations:

**Mission Planner** (Windows - Recommended for Windows users):
- Download: [ardupilot.org/planner/](https://ardupilot.org/planner/)
- Full-featured, mature, extensive documentation

**QGroundControl** (Cross-platform - Mac/Linux/Windows):
- Download: [qgroundcontrol.com](https://qgroundcontrol.com/)
- Modern interface, works on all platforms

### Step 3: Flash Firmware

{: .important }
> **Installation Type**:
> - **First-time installation** (from Betaflight): Use `.hex` file with DFU mode
> - **Updating ArduPilot**: Use `.apj` file with Mission Planner/QGroundControl (easier, no DFU)

#### First-Time Installation (Using .hex file with bootloader)

{: .note }
> The H7-Digital ships with Betaflight pre-installed. To switch to ArduPilot, you must use DFU mode and flash a `.hex` file that includes the ArduPilot bootloader.

1. **Enter DFU Mode**:
   - Power off the H7-Digital completely
   - Hold the **BOOT** button (on the board)
   - Connect USB cable while holding BOOT
   - Release BOOT button
   - The amber LED will illuminate (DFU mode active)

2. **Flash Using Betaflight Configurator** (recommended method):
   - Open [Betaflight Configurator Web App](https://app.betaflight.com)
   - Connect H7-Digital in DFU mode
   - Click "Firmware Flasher" tab
   - Click "Load Firmware [Local]"
   - Select the downloaded `.hex` file (`with-bootloader-signed.hex` or `arducopter_with_bl.hex`)
   - Uncheck "Full Chip Erase" (not needed)
   - Click "Flash Firmware"
   - Wait for completion (board will reboot into ArduPilot)

3. **Alternative: Flash Using STM32CubeProgrammer**:
   - Download [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html) from ST Microelectronics
   - Connect H7-Digital in DFU mode
   - Select USB connection, click "Connect"
   - Click "Open file" and select `.hex` file
   - Click "Download" to flash
   - Disconnect and reconnect USB (board will boot into ArduPilot)

#### Updating ArduPilot (Using .apj files)

{: .note }
> Once ArduPilot is installed, future updates are much easier - no DFU mode required!

**Using Mission Planner**:
1. Connect H7-Digital via USB
2. Click "Setup" → "Install Firmware"
3. Click "Load custom firmware"
4. Select the `.apj` file
5. Wait for upload to complete

**Using QGroundControl**:
1. Connect H7-Digital via USB
2. Go to "Vehicle Setup" → "Firmware"
3. Select "Advanced settings"
4. Choose "Custom firmware file"
5. Select the `.apj` file
6. Click "OK" to flash

**Using SD Card** (no computer required):
- See [ArduPilot SD Card Firmware Loading](https://ardupilot.org/plane/docs/common-install-sdcard.html)
- Rename `.apj` file to appropriate name (see guide)
- Copy to SD card root directory
- Insert SD card and power on
- Firmware flashes automatically

### Step 4: Initial Configuration

After flashing ArduPilot, perform initial setup:

1. **Connect to Ground Station**:
   - Connect via USB
   - Select correct COM port
   - Baud rate: 115200
   - Click "Connect"

2. **Mandatory Setup Wizard** (Mission Planner / QGroundControl):
   - **Frame Type**: Select your frame configuration (Quad X, Hexa, etc.)
   - **Accelerometer Calibration**: Follow on-screen instructions (place board level, on each side)
   - **Compass Calibration**: If using external compass, calibrate (rotate vehicle in all axes)
   - **RC Calibration**: Move all sticks/switches through full range
   - **Flight Modes**: Assign flight modes to RC switch positions
   - **Failsafes**: Configure battery, radio, and GPS failsafes

3. **H7-Digital Specific Parameters** (already set by default, verify if needed):
   - `SERIAL5_PROTOCOL = 2` (MAVLink2 on UART5 for companion computer)
   - `SERIAL2_PROTOCOL = 42` (MSP DisplayPort on UART2 for OSD)
   - `SERIAL8_PROTOCOL = 5` (GPS on UART8)
   - `SERIAL4_PROTOCOL = 23` (RC Input on UART4)
   - `SERIAL3_PROTOCOL = 16` (ESC Telemetry on UART3)
   - `OSD_TYPE = 5` (MSP DisplayPort)
   - `BATT_MONITOR = 4` (Voltage and Current)
   - `INS_GYRO_FILTER = 20` (for ICM-42688-P)

For complete H7-Digital UART mapping with connector locations, see [Hardware Pinout - UART Mapping]({{ site.baseurl }}/docs/hardware/pinout#uart-mapping).

4. **Motor Setup**:
   - Verify motor order matches your frame
   - Test motor direction (props off!)
   - Configure ESC protocol (DShot300/600 recommended)

5. **Pre-Flight Checks**:
   - Verify GPS lock (if installed)
   - Check battery voltage reading
   - Confirm RC input works correctly
   - Verify failsafe triggers properly

### ArduPilot Resources

**Official Documentation**:
- [ArduPilot Copter Home](https://ardupilot.org/copter/)
- [First Time Setup Guide](https://ardupilot.org/copter/docs/initial-setup.html)
- [Loading Firmware Guide](https://ardupilot.org/copter/docs/common-loading-firmware-onto-chibios-only-boards.html)
- [Configuration Parameters](https://ardupilot.org/copter/docs/parameters.html)
- [Companion Computers](https://ardupilot.org/dev/docs/companion-computers.html)

**Community & Support**:
- [ArduPilot Forum](https://discuss.ardupilot.org/)
- [ArduPilot Discord](https://ardupilot.org/discord)

---

## Installing Betaflight

### Step 1: Open Betaflight Configurator

1. Visit [Betaflight Configurator Web App](https://app.betaflight.com)
2. The web app runs directly in your browser (Chrome/Edge recommended)
3. No installation required - works on all operating systems

### Step 2: Enter DFU Mode

{: .note }
> **Pre-Installed Betaflight**: If your H7-Digital ships with Betaflight pre-installed, you can skip DFU mode and flash directly from the Configurator using the "Flash Firmware" button without entering DFU.

To manually enter DFU mode:
1. Power off the H7-Digital completely
2. Hold the **BOOT** button on the board
3. Connect USB cable while holding BOOT
4. Release BOOT button
5. The amber LED will illuminate (DFU mode active)

### Step 3: Flash Firmware

1. **Connect in Configurator**:
   - Launch Betaflight Configurator
   - Connect USB (in DFU mode or normal mode)
   - Configurator should show "DFU" or the board name

2. **Select Firmware**:
   - Click the "Firmware Flasher" tab (on the left sidebar)
   - **Board**: Should auto-detect as "AEROH7DIGITAL"
   - **Firmware Version**: Select latest stable release
   - **Options**:
     - ✅ "Full Chip Erase" (recommended for clean install)
     - ✅ "Flash on connect" (optional, for convenience)

3. **Flash**:
   - Click "Load Firmware [Online]" (to download latest from Betaflight)
   - **Alternative**: Click "Load Firmware [Local]" to flash a downloaded `.hex` file
   - Click "Flash Firmware"
   - Wait for progress bar to complete (30-60 seconds)
   - Board will reboot automatically

4. **Verify**:
   - Disconnect and reconnect USB
   - Configurator should now show the Setup tab
   - Verify firmware version in top-right corner

### Step 4: Initial Configuration

After flashing Betaflight, configure the board:

1. **Ports Tab** (verify defaults):
   - **UART2**: MSP (for VTX/OSD) - DisplayPort protocol
   - **UART4**: Serial RX (for receiver input)
   - **UART5**: (Available for telemetry/VTX control)
   - **UART8**: GPS (if using GPS rescue)

2. **Configuration Tab**:
   - **Board Alignment**: Usually 0/0/0 (adjust if mounting non-standard orientation)
   - **Receiver**: Select your receiver type (SERIAL, SBUS, CRSF, etc.)
   - **ESC/Motor Protocol**: Select DShot300 or DShot600 (recommended)
   - **Motor Idle**: 4-5% (adjust based on ESC)
   - **Features**: Enable what you need:
     - ✅ **OSD** (for on-screen display)
     - ✅ **TELEMETRY** (if using CRSF/Crossfire)
     - ✅ **ESC_SENSOR** (for ESC telemetry on UART3)

3. **Receiver Tab**:
   - **Configure receiver type** (matches Configuration tab)
   - **Map channels** (AETR1234... default for most receivers)
   - **Verify stick movement** in configurator (move sticks, verify bars move correctly)
   - **Set endpoint values** (usually 1000-2000)

4. **Modes Tab**:
   - **ARM**: Assign to a switch (required to arm motors)
   - **Flight Modes**: Configure Acro, Angle, Horizon modes
   - **GPS Rescue**: If using GPS (UART8)

5. **Motors Tab** (PROPS OFF!):
   - ⚠️ **REMOVE PROPELLERS BEFORE TESTING**
   - Enable motor test (slider at bottom)
   - Test each motor spins in correct direction:
     - **M1** (rear-right): CW
     - **M2** (front-right): CCW
     - **M3** (rear-left): CCW
     - **M4** (front-left): CW
   - If wrong direction: Change motor direction in BLHeli configurator or flip motor wires

6. **OSD Tab**:
   - Customize OSD elements (battery voltage, current draw, flight time, etc.)
   - Position elements on screen
   - Enable warnings (battery, RSSI, etc.)

7. **Save Configuration**:
   - Click "Save and Reboot" (bottom right)
   - Board will reboot with new settings

### Step 5: BLHeli ESC Configuration (if needed)

If using DShot and need to configure ESCs:

1. **Connect ESCs**:
   - Ensure ESCs support DShot (most modern 4-in-1 ESCs do)
   - Connect via the ESC connector (8-pin JST-SH)

2. **BLHeli Passthrough**:
   - In Betaflight Configurator, go to "Motors" tab
   - Scroll to bottom, click "BLHeli/Bluejay Passthrough"
   - This opens BLHeli Configurator
   - Configure ESC settings (motor direction, timing, etc.)

### Betaflight Resources

**Official Documentation**:
- [Betaflight Wiki](https://betaflight.com/docs/wiki)
- [Getting Started Guide](https://www.betaflight.com/docs/wiki/getting-started)
- [Configurator Manual](https://www.betaflight.com/docs/category/configurator)

**Community Resources**:
- [Joshua Bardwell's Betaflight Tutorials](https://www.youtube.com/@JoshuaBardwell)
- [Oscar Liang Betaflight Guides](https://oscarliang.com/betaflight/)
- [Betaflight Discord](https://discord.gg/betaflight)

---

## Troubleshooting Installation

### Board Not Recognized (Windows)

**Problem**: Computer doesn't detect H7-Digital when connected via USB

**Solutions**:
1. **Install USB Drivers**:
   - Download [ImpulseRC Driver Fixer](https://impulserc.com/pages/downloads)
   - Run as Administrator
   - Click "Fix" to install all necessary drivers
   - Reconnect board

2. **Try Different USB Cable**:
   - Ensure cable supports **data** (not just charging)
   - Try a different USB port (USB 3.0 ports sometimes have issues, try USB 2.0)

3. **Verify DFU Mode**:
   - Amber LED should illuminate when in DFU mode
   - If not, retry entering DFU (hold BOOT before connecting USB)

### Firmware Flash Failed

**Problem**: Flash operation fails or gets stuck

**Solutions**:
1. **Full Chip Erase**: Enable "Full Chip Erase" option and try again
2. **Different Tool**: Try flashing with STM32CubeProgrammer instead of Betaflight Configurator
3. **DFU Mode**: Ensure you're in DFU mode (amber LED on)
4. **USB Power**: Use a powered USB hub or different port (some ports don't provide enough power)

### Wrong Firmware Loaded

**Problem**: Flashed wrong target or firmware

**Solution**: Re-flash correct firmware:
- **For H7-Digital**: Target is **AEROH7DIGITAL** (Betaflight) or **AeroCogito-H7Digital** (ArduPilot)
- Enter DFU mode and flash correct firmware

### No GPS Lock (ArduPilot)

**Problem**: GPS not working after ArduPilot installation

**Check**:
- GPS module connected to GPS connector (6-pin JST-SH)
- Verify TX/RX connections are not switched
- `SERIAL3_PROTOCOL = 5` (GPS)
- GPS has clear view of sky (not indoors)
- Wait 1-2 minutes for initial lock

### No OSD Display

**Problem**: No on-screen display on FPV goggles

**Check**:
- VTX connected to VTX connector (6-pin JST-SH)
- **ArduPilot**: `OSD_TYPE = 5`, `SERIAL2_PROTOCOL = 42`
- **Betaflight**: OSD feature enabled, UART2 set to MSP
- VTX receiving power (10V BEC enabled - see [VTX Power Control]({{ site.baseurl }}/docs/hardware/power#vtx-power-control-10v-bec))

---

## Next Steps

Now that firmware is installed, proceed to:

[First Flight Checklist →]({{ site.baseurl }}/docs/getting-started/first-flight){: .btn .btn-primary .mr-2 }
[Hardware Pinout Reference →]({{ site.baseurl }}/docs/hardware/pinout){: .btn .btn-blue }

---

## Questions?

Need help with firmware installation?

- **H7-Digital Hardware Issues**: [support@aerocogito.com](mailto:support@aerocogito.com)
- **ArduPilot Support**: [ArduPilot Forum](https://discuss.ardupilot.org/)
- **Betaflight Support**: [Betaflight Discord](https://discord.gg/betaflight)