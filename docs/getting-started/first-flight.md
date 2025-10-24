---
layout: default
title: First Flight Checklist
parent: Getting Started
nav_order: 4
---

# First Flight Checklist
{: .no_toc }

A comprehensive pre-flight checklist and safety guide to ensure your first flight with the H7-Digital is safe and successful.
{: .fs-6 .fw-300 }

---

## Table of Contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Before Your First Flight

{: .warning }
> **CRITICAL SAFETY NOTICE**
>
> - **NEVER** test with propellers installed indoors
> - **ALWAYS** remove propellers for bench testing
> - **VERIFY** all safety features before arming
> - **TEST** in an open area away from people and property
> - **FOLLOW** all local regulations and airspace restrictions

Flying a drone carries inherent risks. This checklist is designed to minimize those risks, but you are ultimately responsible for safe operation.

---

## Pre-Flight Checklist

### Hardware Inspection

Before each flight, physically inspect your aircraft:

- [ ] **Flight controller secure** - No loose mounting, all screws tight
- [ ] **All connectors secure** - ESC, receiver, GPS, VTX properly connected
- [ ] **Wiring secure** - No loose wires, good cable management
- [ ] **Propellers undamaged** - No cracks, chips, or deformation
- [ ] **Motors secure** - No loose motor screws or bell wobble
- [ ] **Frame intact** - No cracks or structural damage
- [ ] **Antennas secure** - GPS, RC receiver, VTX antennas properly mounted
- [ ] **Battery secure** - Properly strapped, balance lead secured
- [ ] **Battery voltage** - Fully charged, cells balanced
- [ ] **SD card inserted** - For logging (if desired)

---

### Firmware Configuration

Verify your firmware is properly configured:

- [ ] **Firmware installed** - Correct version for your application
- [ ] **Board orientation correct** - Arrow points forward
- [ ] **Accelerometer calibrated** - On level surface
- [ ] **Motor outputs configured** - Correct protocol (DShot300/600 recommended)
- [ ] **Motor direction correct** - All spinning correct direction (tested without props)
- [ ] **RC receiver bound** - Transmitter connected and communicating
- [ ] **RC inputs calibrated** - Full stick range recognized
- [ ] **Failsafe configured** - RC loss behavior set appropriately
- [ ] **Battery monitoring configured** - Voltage and current reading correctly
- [ ] **Low battery warnings set** - Appropriate thresholds for your battery

---

### ArduPilot-Specific Checks

If using ArduPilot firmware:

- [ ] **GPS lock achieved** - Minimum 8 satellites, HDOP < 2.0
- [ ] **Compass calibrated** - External compass calibrated outdoors
- [ ] **EKF healthy** - No variance errors in GCS
- [ ] **Pre-arm checks pass** - All pre-arm checks green
- [ ] **Flight modes configured** - Mode switch working correctly
- [ ] **Geofence configured** - If using autonomous modes
- [ ] **RTL altitude set** - Return-to-launch height appropriate

**Minimum flight mode setup**:
- Position 1: Stabilize (manual flight)
- Position 2: Alt Hold (altitude hold)
- Position 3: Loiter (position hold with GPS)
- Always have RTL (Return to Launch) on a switch

---

### Betaflight-Specific Checks

If using Betaflight firmware:

- [ ] **Receiver tab shows inputs** - All channels responding correctly
- [ ] **Modes tab configured** - Arm switch assigned and working
- [ ] **Angle/Horizon mode set** - For first flights (not Acro)
- [ ] **Motor direction verified** - In Motors tab (props OFF)
- [ ] **OSD functional** - defaults to digital HD OSD
- [ ] **Arming disabled flags clear** - Check CLI: `status` command
- [ ] **Blackbox enabled** - If using SD card for logging

**Recommended first flight mode**: Angle mode (self-leveling)

---

## Flight Environment

### Location Requirements

Choose your first flight location carefully:

- [ ] **Open area** - Minimum 50m (150ft) clearance in all directions
- [ ] **Away from people** - No bystanders within 50m
- [ ] **No obstacles** - No trees, buildings, power lines overhead
- [ ] **Legal to fly** - Check local regulations and airspace
- [ ] **Good visibility** - Clear weather, good lighting
- [ ] **Low wind** - Less than 15 mph for first flight
- [ ] **Flat ground** - Level takeoff and landing area
- [ ] **No interference** - Away from WiFi, cell towers, power lines

{: .note }
> **USA Operators**: Check airspace restrictions using a [B4UFLY-approved app](https://www.faa.gov/uas/getting_started/b4ufly) (such as AutoPylot, Aloft, AirMap, or Kittyhawk). Recreational flights require compliance with [FAA Part 107](https://www.faa.gov/uas) or the Exception for Recreational Flyers.

---

### Safety Equipment

Have these items on hand:

- [ ] **Fire extinguisher** - ABC-rated, for LiPo fires
- [ ] **First aid kit** - For minor injuries
- [ ] **Charged transmitter** - Fresh batteries in radio
- [ ] **Spotter** - Another person to watch for hazards (recommended)

---

## Bench Test (Props OFF)

{: .warning }
> **REMOVE PROPELLERS** for all bench tests. Never arm with props installed indoors.

### Power Test

1. **Connect battery** - Listen for ESC startup tones
2. **Check LEDs** - Verify power LED and status LED patterns
3. **Connect ground station** (ArduPilot) or Configurator (Betaflight)
4. **Verify telemetry** - All sensors reading correctly
5. **Check voltage** - Battery voltage displayed correctly

### Motor Test

{: .warning }
> **PROPS MUST BE OFF** - Double-check before proceeding!

1. **Enable motor test mode** - Use GCS or Configurator
2. **Test each motor individually** - See [Motor Outputs]({{ site.baseurl }}/docs/hardware/pinout#motor-outputs-m1-m8) for H7-Digital motor mapping:
   - Motor 1 (rear right) - Verify correct motor spins
   - Motor 2 (front right) - Verify
   - Motor 3 (rear left) - Verify
   - Motor 4 (front left) - Verify
   - Additional motors if hex/octo
3. **Verify direction** - Each motor spins correct direction (props in or props out)
4. **Check ESC response** - No stuttering or unusual sounds

{: .note }
> Motor numbering differs between ArduPilot and Betaflight depending on the frame type selected. Verify against your firmware's documentation. 
- [ArduPilot Motor Order](https://ardupilot.org/copter/docs/connect-escs-and-motors.html) 
- [BetaFlight Motor Order](https://www.betaflight.com/docs/wiki/configurator/motors-tab)

### Radio Test

1. **Power on transmitter** - Before or with aircraft
2. **Verify RC link** - Check RSSI or link quality
3. **Test all sticks** - Roll, pitch, yaw, throttle respond correctly
4. **Test switches** - Arm switch, mode switch, etc.
5. **Test failsafe** - Turn off transmitter, verify failsafe engages
6. **Re-enable transmitter** - Verify reconnection

---

## Final Preparation

### Installing Propellers

{: .important }
> **Install propellers ONLY at the flight site**, not at your workbench!

1. **Verify motor direction** - One last check without props
2. **Install correct propeller on each motor**:
   - Check rotation direction (CW vs CCW)
   - Propeller orientation (leading edge forward)
   - Tighten securely (but don't overtighten)
3. **Double-check all four/six/eight props** - Common mistake!
4. **Spin test** - Gently spin each prop by hand, should rotate freely

### Pre-Arm Checklist

Standing at your flight location, ready to arm:

- [ ] **Clear area confirmed** - No people, animals, or obstacles
- [ ] **Transmitter on** - Full signal strength
- [ ] **Flight mode correct** - Start in Stabilize/Angle mode
- [ ] **Throttle at zero** - Stick at minimum
- [ ] **GPS lock** (ArduPilot only) - Solid 3D fix
- [ ] **Battery fully charged** - Voltage confirmed
- [ ] **Camera/goggles ready** - If FPV flying

---

## First Flight Procedure

### Hover Test (5 minutes)

Your first flight should be simple and short:

1. **Arm the aircraft**
   - Arm switch

2. **Slowly increase throttle**
   - Lift off 1-2 feet (0.5m) only
   - Hover in place for 30 seconds

3. **Check stability**
   - Aircraft should hold position (with GPS) or hover steadily
   - No oscillations or wobbles
   - Controls respond correctly

4. **Land smoothly**
   - Reduce throttle gradually
   - Disarm immediately after landing

5. **Power cycle check**
   - Disconnect battery
   - Check for hot motors (warm is OK, too hot to touch is not)
   - Check for hot ESCs
   - Verify no loose components

### Basic Flight Test (10 minutes)

If hover test was successful:

1. **Takeoff to 10 feet (3m)**
2. **Test basic controls**:
   - Gentle forward/backward (pitch)
   - Gentle left/right (roll)
   - Gentle yaw left/right
   - Altitude changes (climb/descend)
3. **Practice landing** - Several takeoffs and landings
4. **Test flight modes** (ArduPilot):
   - Switch to Alt Hold - verify altitude holds
   - Switch to Loiter - verify position holds
   - Return to Stabilize
5. **Land and disarm**

---

### What to Watch For

**Good signs** ✅:
- Smooth, stable flight
- Quick response to controls
- Holds altitude (with baro/GPS)
- Motors sound consistent
- No unusual vibrations

**Warning signs** ⚠️:
- Drifting in GPS modes (compass calibration issue)
- Oscillations/wobbles (tuning needed)
- Toilet bowling (compass or EKF issue)
- Sluggish response (PID tuning needed)
- Motor stuttering (ESC or power issue)

{: .warning }
> If you see warning signs, land immediately and troubleshoot before continuing!

---

## Post-Flight Checklist

After landing and disarming:

- [ ] **Disconnect battery** - Remove power
- [ ] **Remove propellers** - For safe transport
- [ ] **Check for hot components** - Motors, ESCs, battery
- [ ] **Check for damage** - Visual inspection
- [ ] **Check for loose screws** - Vibration can loosen hardware
- [ ] **Download logs** - For analysis

### Log Analysis

**ArduPilot**:
1. Download .bin log file from SD card or via MAVLink
2. Upload to [UAV Log Viewer](https://plot.ardupilot.org) or use Mission Planner
3. Check for:
   - Vibration levels (should be < 30 m/s²)
   - EKF variance (should be green)
   - Battery voltage sag
   - GPS HDOP and satellite count

**Betaflight**:
1. Remove SD card and copy .BFL files
2. Open in [Blackbox Explorer](https://github.com/betaflight/blackbox-log-viewer)
3. Check for:
   - Gyro noise levels
   - PID performance
   - Motor output saturation
   - ESC telemetry (if enabled)

---

## Troubleshooting Common First Flight Issues

### Won't Arm

**ArduPilot**:
- Check pre-arm messages in Mission Planner/QGC
- Common issues: No GPS lock, compass not calibrated, accel not calibrated
- See [ArduPilot Pre-Arm Checks](https://ardupilot.org/copter/docs/common-prearm-safety-checks.html)

**Betaflight**:
- Check CLI command: `status`
- Look for arming disable flags
- Common issues: Accelerometer not calibrated, RX not connected, CLI mode active
- See [Betaflight Arming Issues](https://www.betaflight.com/docs/wiki/guides/current/Arming-Sequence-And-Safety)

### Drifting in Flight

- **Cause**: Poor compass calibration, interference, or GPS issues
- **Fix**: Re-calibrate compass outdoors, away from metal/electronics
- **ArduPilot**: Check compass offsets (should be < 600)

### Motor Stuttering

- **Cause**: ESC issues, power supply, DShot configuration
- **Fix**: Check battery voltage, try different DShot speed, check ESC connections

### Unstable Flight / Oscillations

- **Cause**: PID tuning too aggressive, vibration issues
- **Fix**: Reduce PID gains, improve vibration isolation
- **Check**: Propellers balanced, motor screws tight, soft-mount installed

---

## Emergency Procedures

### Loss of Control

1. **Release sticks to center** - Let aircraft stabilize
2. **Switch to self-level mode** - Angle/Stabilize mode
3. **Reduce altitude slowly**
4. **Land as soon as safe**

### Flyaway

If aircraft flies away uncontrolled:

1. **ArduPilot**: Activate [RTL](https://ardupilot.org/plane/docs/rtl-mode.html) (Return to Launch) mode immediately
2. **Betaflight**: Switch to Angle mode, try to regain control or engage [GPS Rescue](https://www.betaflight.com/docs/wiki/guides/current/GPS-Rescue-v4-5)
3. **Last resort**: Disarm (aircraft will fall/crash)

{: .warning }
> Only disarm as a last resort if aircraft is heading toward people or danger!

### Battery Fire

LiPo fires are rare but serious:

1. **Land immediately** if fire occurs in flight
2. **Move away from aircraft** - Minimum 20 feet
3. **Use ABC fire extinguisher** - Or sand/dirt (never water!)
4. **Call emergency services** if fire spreads
5. **Do not touch** - LiPo fires are extremely hot

---

## Building Experience

### Recommended Progression

Don't rush into advanced flying:

- **Flight 1-5**: Hover practice, basic controls, landing practice
- **Flight 6-10**: Figure-8 patterns, altitude changes, orientation practice
- **Flight 11-20**: GPS modes (ArduPilot), rate mode practice (Betaflight)
- **Flight 21+**: Advanced maneuvers, autonomous missions, or acrobatics

### Flight Hour Recommendations

- **10 hours**: Basic competency in manual flight
- **25 hours**: Comfortable with all flight modes/conditions
- **50 hours**: Proficient for professional operations
- **100+ hours**: Expert-level capability

{: .tip }
> Keep a flight log! Track flight time, conditions, issues, and improvements.

---

## Regulatory Compliance

### United States (FAA)

**Recreational Flyers**:
- [Register](https://www.faa.gov/uas/getting_started/register_drone) aircraft if > 0.55 lbs (250g)
- Fly with a [Remote ID](https://www.faa.gov/uas/getting_started/remote_id) (if required)
- Follow [The Exception for Recreational Flyers](https://www.faa.gov/uas/recreational_flyers)
- Fly in uncontrolled airspace (Class G)
- Respect altitude limits (400 ft AGL)
- Check airspace using a [B4UFLY-approved app](https://www.faa.gov/uas/getting_started/b4ufly) (AutoPylot, Aloft, AirMap, Kittyhawk, etc.)

**Commercial/Professional Operations**:
- Obtain Part 107 Remote Pilot Certificate
- Register aircraft
- Follow Part 107 regulations
- Waivers required for some operations

### Other Jurisdictions

Check local regulations:
- **Canada**: Transport Canada drone regulations
- **Europe**: EASA drone categories (Open/Specific/Certified)
- **Australia**: CASA drone rules
- **UK**: CAA regulations

{: .important }
> **You are responsible** for knowing and following all applicable regulations in your area!

---

## Next Steps

After successful first flights:

### ArduPilot Users
- Configure [advanced flight modes](https://ardupilot.org/copter/docs/flight-modes.html)
- Set up [companion computer](https://ardupilot.org/dev/docs/companion-computers.html) for autonomous operations
- Plan autonomous missions with [Mission Planner](https://ardupilot.org/planner/) or [QGroundControl](https://qgroundcontrol.com/)
- Fine-tune PID values for your airframe [Common Tuning](https://ardupilot.org/copter/docs/common-tuning.html)

### Betaflight Users
- Optimize [PID tuning](https://www.betaflight.com/docs/development/PID-tuning) for your flying style
- Set up [blackbox logging](https://www.betaflight.com/docs/wiki/guides/current/Black-Box-logging-and-usage) for performance analysis
- Learn advanced techniques from [Oscar Liang](https://oscarliang.com/) and [Joshua Bardwell](https://www.youtube.com/@JoshuaBardwell)
- Progress to rate/acro mode for full manual control

### All Users
- Join community forums and share experiences
- Keep firmware updated
- Practice, practice, practice!

---

## Support

If you encounter issues during your first flight:

- **Email**: [support@aerocogito.com](mailto:support@aerocogito.com)
- **ArduPilot Community**: [Forum](https://discuss.ardupilot.org/) • [Discord](https://ardupilot.org/discord)
- **Betaflight Discord**: [betaflight.com/community](https://betaflight.com/community)

---

**Fly safe, fly legal, have fun!** 🚁

[← Back to Getting Started]({{ site.baseurl }}/docs/getting-started/){: .btn }
[Hardware Specifications →]({{ site.baseurl }}/docs/hardware/specifications){: .btn .btn-purple }
