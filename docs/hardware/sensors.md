---
layout: default
title: Sensors
parent: Hardware
nav_order: 5
---

# Sensors
{: .no_toc }

Complete guide to the H7-Digital onboard sensors and external sensor connections for GPS and compass modules.
{: .fs-6 .fw-300 }

---

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Onboard Sensors

### IMU: ICM-42688-P

The H7-Digital features the **TDK InvenSense ICM-42688-P**, a high-performance 6-axis MEMS motion tracking device combining a 3-axis gyroscope and 3-axis accelerometer. The ICM-42688-P is considered one of the best IMUs for flight control, offering exceptional noise performance and stability with an isolated 3.3V power rail.

For complete technical specifications, hardware capabilities, and firmware configuration details, see [Specifications - IMU]({{ site.baseurl }}/docs/hardware/specifications#imu-icm-42688-p).

**Connection:**
- **SPI Bus**: SPI1 (PA5=SCK, PA6=MISO, PA7=MOSI)
- **Chip Select**: PB0 (GYRO_CS)
- **Interrupt**: PB1 (GYRO_EXTI)
- **Clock Input**: PE5 (GYRO_CLKIN - Betaflight only)

{: .note }
> **Isolated Power Rail**: The ICM-42688-P uses a dedicated ultra-low-noise 3.3V LDO regulator, isolated from MCU digital switching noise. This ensures clean gyroscope readings and reduces false vibrations in sensor data. See [Power System]({{ site.baseurl }}/docs/hardware/power#33v-imu-isolated-power-rail) for details.

{: .note }
> **External Clock (Betaflight)**: The H7-Digital routes the gyro external clock (CLKIN) to PE5 for ultra-precise timing. Betaflight uses this for reduced gyro jitter and improved loop timing. ArduPilot does not currently support external gyro clocking.

---

**Understanding Programmable Range Modes:**

The ICM-42688-P offers eight programmable gyroscope ranges (±15.625 to ±4000 dps) and four accelerometer ranges (±2g to ±32g). However, **both Betaflight and ArduPilot use fixed settings**:

- **Gyroscope**: ±2000 dps (both firmwares)
- **Accelerometer**: ±16g (both firmwares)

{: .note }
> **Why ±2000 dps / ±16g?** This is the industry-standard "sweet spot" that provides 2-4x safety margin above typical flight maneuvers while maintaining excellent resolution (0.0610 dps/LSB gyro, 0.488 mg/LSB accel in standard 16-bit mode) for precise PID control. Lower ranges risk saturation during aerobatics; higher ranges sacrifice resolution with no practical benefit.

**The Resolution vs. Range Trade-off:**

The ICM-42688-P uses 16-bit ADCs (-32,768 to +32,767 range). Increasing the full-scale range decreases resolution per bit:

{: .note }
> **Resolution values below are for standard 16-bit mode.** ArduPilot uses high-resolution FIFO mode (19-bit gyro, 18-bit accel) which provides 8x/4x better resolution at the same ±2000 dps / ±16g ranges. See [High-Resolution Sampling](#high-resolution-sampling-ardupilot-vs-betaflight) section below.

| Gyro Range | Resolution (dps/LSB) | Firmware Use | Trade-off |
|------------|---------------------|--------------|-----------|
| ±125 dps | 0.0038 (16x better) | ❌ Not used | Saturates during basic maneuvers |
| ±500 dps | 0.0153 (4x better) | ❌ Not used | Saturates during fast rolls/flips |
| ±1000 dps | 0.0306 (2x better) | ❌ Not used | Marginal for aggressive flight |
| **±2000 dps** | **0.0610** | ✅ **Standard** | **Optimal balance** |
| ±4000 dps | 0.1225 (2x worse) | ❌ Not used | Unnecessary - halves resolution |

**Why Both Firmwares Chose ±2000 dps:**
- ✅ Racing quads peak at ~1500 dps (extreme flips) - 33% safety margin
- ✅ Autonomous aircraft stay below 500 dps - 4x safety margin
- ✅ Maintains excellent resolution for precise PID control
- ✅ Industry standard - tuning parameters designed for this range
- ✅ Proven with billions of flight hours

**Accelerometer: Why ±16g Standard:**

{: .note }
> **Resolution values below are for standard 16-bit mode.** ArduPilot's high-resolution mode provides 18-bit accel (0.122 mg/LSB at ±16g).

| Accel Range | Resolution (mg/LSB) | Use Case | Trade-off |
|-------------|---------------------|----------|-----------|
| ±8g | 0.244 (2x better) | ❌ | Risk of saturation during hard maneuvers |
| **±16g** | **0.488** | ✅ **Standard** | **Covers aerobatics + crash detection** |
| ±32g | 0.977 (2x worse) | ❌ | Unnecessary - halves resolution |

Typical flight: 1-4g normal, 4-8g aerobatics, 10-20g crashes. The ±16g range handles all scenarios with good resolution.

{: .note }
> For driver source code references, firmware implementation details, and the official datasheet, see [Specifications - IMU Driver References]({{ site.baseurl }}/docs/hardware/specifications#imu-icm-42688-p).

**Additional Reading:**
- [IMU Specifications Explained](https://www.vectornav.com/resources/inertial-navigation-primer/specifications--and--error-budgets/specs-imuspecs) - VectorNav (Resolution vs. Range Trade-off Theory)

---

#### High-Resolution Sampling: ArduPilot vs Betaflight

The ICM-42688-P features an **industry-first 20-bit FIFO format** that provides higher resolution data than standard 16-bit ADC output. **ArduPilot and Betaflight use this capability differently** based on their design priorities.

**ArduPilot: High-Resolution FIFO Mode Enabled ✅**

ArduPilot enables the ICM-42688-P's 20-bit FIFO format (FIFO_HIRES_EN bit) for enhanced resolution:

**Data Structure (20 bytes per sample):**
- **19-bit gyroscope** (16-bit MSB + 3-bit fractional) = 0.0076 dps/LSB at ±2000 dps
- **18-bit accelerometer** (16-bit MSB + 2-bit fractional) = 0.122 mg/LSB at ±16g
- Header, temperature (16-bit), timestamp (16-bit)

**Resolution Improvement:**
- Gyro: 16-bit → 19-bit = **8x better resolution**
- Accel: 16-bit → 18-bit = **4x better resolution**

**Sampling Strategy:**
1. Hardware samples at 8 kHz with 19/18-bit resolution
2. Data buffered in 2048-byte FIFO (~105 high-res samples)
3. Software reads FIFO, averages 8 samples → 1 kHz backend (default)
4. EKF runs at 400 Hz on downsampled data

**Why High-Res Mode Benefits ArduPilot:**
- **Precision hover**: At 10 dps slow drift, 19-bit = 0.076% error vs 16-bit = 0.6% error (8x improvement)
- **EKF fusion**: Cleaner sensor data improves GPS/optical flow fusion for position hold
- **Anti-aliasing**: 8 kHz → 1 kHz downsampling prevents 600-4000 Hz vibration aliasing
- **CPU headroom available**: 400 Hz main loop uses ~30-50% CPU, +15-20% overhead acceptable

**Trade-offs:**
- ⚠️ +0.5-1ms latency from FIFO buffering
- ⚠️ +15-20% CPU overhead for bit extraction and averaging
- ⚠️ More complex driver implementation

---

**Betaflight: Standard 16-bit Direct Register Mode**

Betaflight reads **directly from 16-bit output registers** without using FIFO or high-resolution mode:

**Data Structure (12 bytes per sample):**
- **16-bit gyroscope** = 0.0610 dps/LSB at ±2000 dps
- **16-bit accelerometer** = 0.488 mg/LSB at ±16g
- Direct register read: `GYRO_DATA_X1` (0x25), `ACCEL_DATA_X1` (0x1F)

**Sampling Strategy:**
1. Hardware samples at 8 kHz (native ODR)
2. Data-ready interrupt triggers immediate register read
3. Simple `int16_t` conversion, minimal processing
4. All filtering (AAF, notch, lowpass) runs at 8 kHz
5. PID loop locked to gyro rate (8 kHz)

**Why Standard Mode Benefits Betaflight:**
- **Minimum latency**: Direct read <0.5ms vs FIFO ~1ms (critical for racing)
- **CPU efficiency**: 8 kHz PID + BiDir DShot + dynamic notch needs every cycle
- **Resolution sufficient**: At 500-1500 dps racing, 0.061 dps/LSB = 0.012-0.036% accuracy (exceeds human perception)
- **Hardware AAF**: Configurable 258-1962 Hz anti-alias filter provides clean data before sampling

**Trade-offs:**
- ⚠️ Lower resolution at slow speeds (0-100 dps) - less critical for manual control
- ⚠️ No FIFO buffering (must read every sample immediately)

---

**Technical Comparison**

| Aspect | ArduPilot (High-Res FIFO) | Betaflight (Standard Direct) |
|--------|---------------------------|------------------------------|
| **Data Source** | FIFO buffer (20-byte packets) | Direct registers (12-byte read) |
| **Gyro Resolution** | 19-bit (0.0076 dps/LSB) | 16-bit (0.0610 dps/LSB) |
| **Accel Resolution** | 18-bit (0.122 mg/LSB) | 16-bit (0.488 mg/LSB) |
| **FIFO Enable** | `FIFO_HIRES_EN` bit set | No FIFO used |
| **Sampling** | 8 kHz → avg → 1 kHz backend | 8 kHz native (no downsampling) |
| **Latency** | 0.5-1ms (FIFO buffering) | <0.5ms (immediate read) |
| **CPU Overhead** | +15-20% (bit extraction, averaging) | Baseline (most efficient) |
| **Anti-Aliasing** | Downsampling (8 kHz → 1 kHz) | Hardware AAF (258-1962 Hz) |
| **Best Use Case** | 0-100 dps slow precision | 500-1500 dps fast maneuvers |
| **Optimized For** | Autonomous, GPS fusion, hover | FPV racing, freestyle, acro |

**When High Resolution Actually Matters:**

| Motion Type | 16-bit Accuracy | 19-bit Accuracy | Benefit |
|-------------|-----------------|-----------------|---------|
| **Hover (0-10 dps)** | 0.6% error | 0.076% error | **8x improvement** - significant |
| **Cruise (100-500 dps)** | 0.012-0.06% error | 0.0015-0.008% error | Marginal benefit |
| **Racing (500-1500 dps)** | 0.004-0.012% error | 0.0005-0.0015% error | **No perceptible benefit** |

**Conclusion:**

Both implementations are **correct for their target applications**:

- **ArduPilot**: High-res mode provides superior slow-motion sensing for autonomous operations where precision position hold and GPS fusion are critical. The additional latency and CPU cost are acceptable when main loop runs at 400 Hz.

- **Betaflight**: Standard mode minimizes latency and CPU load for manual FPV control where every millisecond matters and racing speeds (500-1500 dps) already provide 0.012% accuracy with 16-bit data - far exceeding what human pilots can perceive or utilize.

The H7-Digital's ICM-42688-P supports both modes, allowing the same hardware to excel at autonomous missions (ArduPilot) and FPV racing (Betaflight).

**Driver Source Code:**
- [ArduPilot Invensensev3 Driver](https://github.com/ArduPilot/ardupilot/blob/master/libraries/AP_InertialSensor/AP_InertialSensor_Invensensev3.cpp) - Lines 267-270: `GYRO_SCALE_HIGHRES_2000DPS`, `ACCEL_SCALE_HIGHRES_16G`; FIFO high-res enable
- [Betaflight ICM426XX Driver](https://github.com/betaflight/betaflight/blob/master/src/main/drivers/accgyro/accgyro_spi_icm426xx.c) - Standard 16-bit register reads; no FIFO high-res mode
- [ICM-42688-P Datasheet](https://product.tdk.com/system/files/dam/doc/product/sensor/mortion-inertial/imu/data_sheet/ds-000347-icm-42688-p-v1.6.pdf) - Section 14.44: FIFO_CONFIG1 register, FIFO_HIRES_EN bit

---

**Mounting Orientation:**

The IMU is rotated in firmware to match the H7-Digital board orientation:

**ArduPilot Default Rotation:**
```
AHRS_ORIENTATION = 10    # ROTATION_ROLL_180_YAW_90
```

**Betaflight Default Rotation:**
```
GYRO_1_ALIGN CW90_DEG

Runtime adjustment via CLI (if needed):
  set align_board_roll = 180
  set align_board_pitch = 0
  set align_board_yaw = 90
```

{: .note }
> Hardware rotation is configured in `config.h` via `GYRO_1_ALIGN`. Runtime adjustments can be made via CLI commands shown above.

{: .tip }
> If you mount the flight controller in a non-standard orientation, adjust the rotation parameters above. For ArduPilot, see the [complete AHRS_ORIENTATION enum list](https://ardupilot.org/copter/docs/common-mounting-the-flight-controller.html). For Betaflight, set custom degrees via CLI or leave as default and physically mount the board correctly.

---

### Barometer: DPS368

The H7-Digital includes the **Infineon DPS368** digital barometric pressure sensor for altitude estimation. With ±0.02m (2cm) resolution and IPX8 waterproof rating, the DPS368 provides precise altitude measurements for flight control and position hold modes.

For complete technical specifications, measurement rates, and accuracy details, see [Specifications - Barometer]({{ site.baseurl }}/docs/hardware/specifications#barometer-dps368).

**Connection:**
- **I2C Bus**: I2C1 (internal, PB8=SCL, PB9=SDA)
- **I2C Address**: 0x76
- **I2C Speed**: 400kHz (ArduPilot), 800kHz (Betaflight default)

{: .warning }
> **Driver Naming**: Both firmwares use the DPS310 driver for the DPS368 sensor. The DPS310 and DPS368 are register-compatible - the DPS368 is the newer variant with improved IPX8 waterproofing. See [Specifications - Barometer Driver Compatibility]({{ site.baseurl }}/docs/hardware/specifications#barometer-dps368) for driver details and source code references.

{: .note }
> **I2C Speed (Betaflight)**: Betaflight defaults to 800kHz I2C bus speed for faster sensor reads and reduced loop jitter. If experiencing I2C sensor detection issues, reduce speed to 400kHz via CLI: `set i2c1_clockspeed_khz = 400`. See [Pinout Reference]({{ site.baseurl }}/docs/hardware/pinout#i2c-ports) for details.

{: .tip }
> **Calibration**: Barometers measure relative altitude changes accurately but require ground-level calibration for absolute altitude. Always set ground elevation before flight or use GPS altitude for absolute reference.

---

## External Sensors

{: .tip }
> **Bench Testing with USB Power**: The GPS and compass connectors receive 4.5V when powered via USB alone (no battery required). This allows you to test GPS lock, compass calibration, and firmware sensor detection during bench setup - useful for indoor configuration before installing sensors on the aircraft.

### GPS Module (Not Included)

An external GPS module is **recommended** for autonomous flight modes, position hold, and return-to-launch functionality.

**Connection:**
- **Connector**: GPS/Compass (6-pin JST-SH)
- **UART**: UART8 (PE0=RX, PE1=TX)
- **I2C**: I2C4 (PB6=SCL, PB7=SDA for compass)
- **Power**: 5V (4.5V via USB - see USB power note above)

**Firmware-Specific Configuration:**

| Feature | ArduPilot | Betaflight |
|---------|-----------|------------|
| **Serial Port** | SERIAL8 (UART8) | UART8 |
| **Protocol** | GPS (default) | Set in Ports tab |
| **Baud Rate** | 115200 (auto-detect) | 115200 (UBLOX), 9600 (NMEA) |
| **DMA** | Enabled | Enabled |

**ArduPilot Parameters:**
```
SERIAL8_PROTOCOL = 5     (GPS)
SERIAL8_BAUD = 115       (115200 for most GPS modules)
GPS_TYPE = 1             (Auto-detect u-blox/NMEA)
```

**Betaflight Configuration:**
- **Ports Tab**: Enable GPS on UART8, set baud to 115200
- **Configuration Tab**: GPS protocol (UBLOX, NMEA, etc.)

**Recommended GPS Modules:**

| Model | GPS Chipset | Compass | Notes |
|-------|-------------|---------|-------|
| **Holybro M9N** | u-blox M9N | Integrated | Recommended for ArduPilot |
| **Matek M10-5883** | u-blox M10 | QMC5883L | Latest generation, excellent accuracy |
| **Beitian BN-880** | u-blox M8N | HMC5883L | Budget option, widely available |
| **SpeedyBee GPS** | u-blox M8/M9 | Integrated | Good value, multiple variants |

{: .note }
> **GPS Generation Comparison**: u-blox M10 (newest, 2020+) offers faster fix times and better multipath rejection than M9 (2019) and M8 (2014). All three generations are supported by ArduPilot and Betaflight. M10 modules are recommended for new builds but M8/M9 remain excellent choices.

**Mounting Recommendations:**
- **Location**: Mount away from motors and ESCs (reduce electromagnetic interference)
- **Height**: Elevated above frame (better satellite visibility)
- **Orientation**: Arrow pointing forward (verify in ground station)
- **Distance from compass**: Keep 10+ cm from high-current wires and motors

---

### External Compass (Not Included)

An external compass (magnetometer) is **highly recommended** for ArduPilot autonomous flight.

**Why External Compass?**

The H7-Digital does not have an onboard compass. External compasses are preferred because:
- ✅ Can be mounted away from electromagnetic interference
- ✅ Better heading accuracy (less distortion from motors/ESCs/wires)
- ✅ Essential for compass-dependent flight modes (Loiter, Auto, Guided)
- ✅ Reduces compass calibration issues

**Connection:**
- **Connector**: GPS/Compass (6-pin JST-SH)
- **I2C Bus**: I2C4 (PB6=SCL, PB7=SDA)
- **I2C Speed**: 400kHz (ArduPilot), 800kHz (Betaflight default)
- **Power**: 5V (4.5V via USB - see USB power note above)

**Firmware-Specific Configuration:**

| Feature | ArduPilot | Betaflight |
|---------|-----------|------------|
| **I2C Bus** | I2C4 | I2CDEV_4 (`MAG_I2C_INSTANCE`) |
| **I2C Speed** | 400kHz | 800kHz (default) |
| **Detection** | Auto on boot | Auto on boot (limited) |
| **Required** | Recommended for autonomous modes | Not used for stabilization |
| **Allow Arming** | Without compass (`ALLOW_ARM_NO_COMPASS`) | N/A |

**Common Compass ICs:**
- **QMC5883L** - Most common in GPS combo modules, good performance
- **HMC5883L** - Older generation, still widely used and supported
- **IST8310** - High accuracy, good interference resistance
- **LIS3MDL** - Newer, ST Microelectronics, ArduPilot support

**ArduPilot Configuration:**
```
COMPASS_ENABLE = 1
COMPASS_USE = 1
COMPASS_EXTERNAL = 1
COMPASS_AUTODEC = 1      (Auto magnetic declination)
```

ArduPilot will auto-detect most I2C compass modules on boot.

**Betaflight:**
- Betaflight **does not use compass** for stabilization
- Compass support is limited to GPS rescue mode heading (optional)
- Compass auto-detection may be less reliable than ArduPilot

---

## Sensor Calibration

### When to Calibrate

**Accelerometer:**
- ✅ First-time setup
- ✅ After firmware update
- ✅ After physical impact or crash
- ✅ If horizon drifts in level flight

**Compass (ArduPilot only):**
- ✅ First-time setup
- ✅ After moving compass location
- ✅ If heading drifts or "toilet bowling" occurs
- ✅ When flying in new geographic location (magnetic declination changes)

**Barometer:**
- Auto-calibrated on boot (ArduPilot)
- Set ground elevation in ground station before flight

---

### Accelerometer Calibration

**ArduPilot (via Mission Planner):**

1. Connect to flight controller via USB or telemetry
2. Go to **Initial Setup → Mandatory Hardware → Accel Calibration**
3. Click **Calibrate Accel**
4. Follow on-screen prompts to position aircraft in 6 orientations:
   - Level
   - Left side
   - Right side
   - Nose down
   - Nose up
   - Upside down
5. Keep aircraft **perfectly still** during each measurement
6. Click **Done** when complete

**Betaflight (via Configurator):**

1. Place flight controller on **level surface**
2. Go to **Setup Tab**
3. Click **Calibrate Accelerometer**
4. Keep board perfectly still during calibration (~5 seconds)
5. Verify horizon is level in artificial horizon display

{: .warning }
> **Level Surface Required**: Calibrate accelerometer on a perfectly level surface (use spirit level if available). Poor accelerometer calibration causes drift, toilet bowling, and unstable flight.

---

### Compass Calibration (ArduPilot)

**Large Vehicle Method** (Recommended for quads):

1. Connect to Mission Planner
2. Go to **Initial Setup → Mandatory Hardware → Compass**
3. Select **Onboard Mag Calibration**
4. Arm throttle and take off
5. Fly in **large circles** for 30-60 seconds
6. Perform aggressive **yaw maneuvers** (rotate aircraft on all axes)
7. Land and disarm - calibration saves automatically

**Manual Method** (Ground calibration):

1. Go to **Initial Setup → Mandatory Hardware → Compass**
2. Click **Start** (Live Calibration or Onboard Calibration)
3. Rotate aircraft slowly in **all directions**:
   - Rotate 360° around each axis (pitch, roll, yaw)
   - Move smoothly and continuously
   - Cover all orientations (imagine aircraft inside a sphere)
4. Continue until progress bars fill or "Calibration Successful" appears
5. Click **Done**

{: .tip }
> **Interference Check**: After calibration, check compass health in **Flight Data** screen. "Compass variance" should be < 0.5. Higher values indicate magnetic interference from motors, ESCs, or wires.

---

## Vibration Isolation

### Why Vibration Matters

Excessive vibration causes:
- ❌ Poor gyro data quality
- ❌ False vibration detection in logs
- ❌ Ineffective dynamic notch filters
- ❌ Reduced flight performance
- ❌ "Toilet bowling" in GPS modes (vibration interferes with accelerometer)

### Soft Mounting Recommendations

**Good Vibration Isolation:**
- ✅ Use soft-mount grommets or standoffs (included with board)
- ✅ Ensure grommets are not over-compressed
- ✅ Avoid metal-to-metal contact between FC and frame
- ✅ Use 3M double-sided foam tape for additional damping (optional)

**Avoid:**
- ❌ Hard-mounting FC directly to carbon frame
- ❌ Over-tightening mounting screws (compresses grommets too much)
- ❌ Mounting near high-vibration sources (ESC, motors)

### Checking Vibration Levels

**ArduPilot (via Mission Planner):**

1. Fly and record Blackbox log
2. Download log via **Flight Data → DataFlash Logs**
3. Analyze in **Log Review**
4. Check **VIBE** messages:
   - VibeX, VibeY, VibeZ should be < 30 (ideally < 15)
   - Clip counts should be 0 or very low

**Betaflight (via Blackbox Explorer):**

1. Record Blackbox log during flight
2. Download and open in **Blackbox Explorer**
3. Check **gyroADC** traces:
   - Should be smooth with minimal high-frequency noise
   - Sharp spikes indicate vibration issues
4. Check **Debug → Gyro Scaled** for vibration amplitude

{: .note }
> **Good Vibration Levels**: ArduPilot VIBE < 15, Betaflight gyro traces smooth without excessive spikes. If vibration is high, check motor balance, propeller condition, and soft-mount integrity.

---

## Troubleshooting Sensor Issues

### IMU Not Detected

**Symptoms**: "IMU not detected" error, no gyro data in ground station

**Check:**
1. ✓ Firmware flashed correctly (verify board target)
2. ✓ SPI1 connections (no physical damage)
3. ✓ Power to IMU (check 3.3V isolated rail)
4. ✓ Re-flash firmware (may fix SPI initialization issues)

**Solutions:**
- Reflash latest firmware
- Check for cold solder joints on IMU (manufacturing defect)
- Contact support if hardware failure suspected

---

### Barometer Not Detected

**Symptoms**: "Baro not detected" error, no altitude data

**Check:**
1. ✓ I2C1 bus initialized correctly
2. ✓ Firmware includes DPS368 driver
3. ✓ No I2C address conflicts

**Solutions:**
- Verify `BARO DPS310 I2C:0:0x76` in hwdef.dat (firmware definition)
- Re-flash firmware
- Contact support if persistent

---

### GPS No Fix / No Satellites

**Symptoms**: GPS shows "No Fix", 0 satellites, or HDOP > 2.0

**Check:**
1. ✓ Outdoor location (GPS does not work indoors)
2. ✓ Clear view of sky (no buildings, trees, metal roofs)
3. ✓ GPS antenna orientation (ceramic patch facing up)
4. ✓ UART8 configured for GPS protocol
5. ✓ GPS module powered (4.5V on GPS connector)
6. ✓ TX/RX not swapped

**Solutions:**
- Wait 2-5 minutes for cold start (first GPS lock)
- Move to open area away from buildings
- Verify GPS UART configuration (SERIAL8_PROTOCOL = 5)
- Check GPS LED (should blink when searching, solid when locked)

---

### Compass Interference / Poor Calibration

**Symptoms**: "Compass variance" errors, toilet bowling, heading drift

**Check:**
1. ✓ Compass mounted away from motors/ESCs (> 10cm)
2. ✓ High-current wires not near compass
3. ✓ Compass orientation set correctly
4. ✓ Magnetic declination set (auto or manual)

**Solutions:**
- Re-calibrate compass (large vehicle method)
- Move compass further from interference sources
- Check for metal screws/fasteners near compass
- Verify compass orientation matches physical mounting
- Set `COMPASS_AUTODEC = 1` for automatic magnetic declination

---

### High Vibration Levels

**Symptoms**: VIBE > 30 in logs, poor flight performance, toilet bowling

**Check:**
1. ✓ Soft-mount grommets installed and not over-compressed
2. ✓ Motor balance (check for bent shafts, damaged bearings)
3. ✓ Propeller condition (no nicks, cracks, or imbalance)
4. ✓ Frame rigidity (no loose screws, cracked carbon)
5. ✓ ESC mounting (not touching frame directly)

**Solutions:**
- Replace damaged propellers
- Balance motors (or replace if bearings worn)
- Use dynamic notch filters (enabled by default in modern firmware)
- Add additional soft-mount damping (foam tape under FC)
- Reduce motor/propeller imbalance sources

---

## Related Documentation

- [Power System]({{ site.baseurl }}/docs/hardware/power) - 3.3V isolated IMU power rail
- [Pinout & Connectors]({{ site.baseurl }}/docs/hardware/pinout) - Sensor pin assignments
- [Specifications]({{ site.baseurl }}/docs/hardware/specifications) - Complete sensor specifications

## External Resources

- [ArduPilot Compass Setup](https://ardupilot.org/copter/docs/common-compass-setup-advanced.html) - Official calibration guide
- [Betaflight Setup Tab](https://betaflight.com/docs/wiki/configurator/setup-tab) - Sensor calibration and board alignment

---

## Support

For sensor-related questions:
- **Email**: [support@aerocogito.com](mailto:support@aerocogito.com)
- **Documentation**: [H7-Digital Docs](https://aerocogito.github.io/h7-digital-docs)
