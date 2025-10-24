---
layout: default
title: Building Your Drone
parent: Getting Started
nav_order: 3
---

# Building Your FPV Drone
{: .no_toc }

Physical assembly instructions and best practices for building with the H7-Digital flight controller.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Learn from the Experts

Rather than recreate excellent existing content, we recommend Joshua Bardwell's comprehensive FPV build series. Joshua is one of the most respected voices in the FPV community and his tutorials are thorough, clear, and beginner-friendly.

### Joshua Bardwell's FPV Build Series

**[Complete FPV Drone Build Tutorial Playlist](https://www.youtube.com/playlist?list=PLwoDb7WF6c8l24IM83wIS94XzhuMVC2gx)**

This playlist covers everything from basic tools to advanced build techniques:

- **Selecting Components**: Choosing compatible parts for your build
- **Assembly Process**: Step-by-step frame assembly and component installation
- **Wiring Best Practices**: Clean, reliable wiring techniques
- **Soldering Techniques**: Proper soldering for flight controllers and ESCs
- **Cable Management**: Professional-looking and crash-resistant builds
- **Testing Procedures**: How to test before your first flight

{: .note }
> While Joshua's videos use various flight controllers, the fundamental principles apply directly to building with the H7-Digital. The connector standard (JST-SH) makes the H7-Digital even easier to build with minimal soldering!

---

## H7-Digital Specific Considerations

When following build tutorials with your H7-Digital, keep these points in mind:

### Mounting

- **Soft mounting recommended**: Use the included rubber dampers for vibration isolation
- **Orientation matters**: The arrow on the board must point forward
- **30.5mm mounting pattern**: Standard spacing compatible with most frames

See the [Unboxing page]({{ site.baseurl }}/docs/getting-started/unboxing) for mounting details.

### Connectors

The H7-Digital uses **JST-SH connectors** following the [Betaflight Standard](https://www.betaflight.com/docs/development/manufacturer/connector-standard):

**Benefits:**
- ✅ **No soldering required** for most connections
- ✅ **Quick assembly** with plug-and-play peripherals
- ✅ **Easy maintenance** - swap components without desoldering
- ✅ **Cleaner builds** - no wire mess

**What this means for you:**
- ESCs using [Betaflight Standard](https://www.betaflight.com/docs/development/manufacturer/connector-standard#standard-esc-pin-configuration) connect via 8-pin JST-SH (cable included)
- GPS, VTX, and other peripherals use JST-SH connectors
- Only solder if you're adding custom components

{: .warning }
> Always verify ESC pinout matches the flight controller before connecting - serious damage may occur with incorrect pinout. Different manufacturers have different pinouts. The H7-Digital follows the [Betaflight Standard](https://www.betaflight.com/docs/development/manufacturer/connector-standard#standard-esc-pin-configuration) for maximum compatibility.

### Power System

- **Input voltage**: 3S-6S (11.1V - 25.2V)
- **5V BEC**: 2A continuous, 2.5A peak
- **10V VTX power**: 2A continuous, 2.5A peak

See [Hardware Power System]({{ site.baseurl }}/docs/hardware/power) for wiring details.

---

## Build Process Overview

Here's the high-level build process when using the H7-Digital:

### 1. Frame Assembly
- Assemble frame
- Install motors on arms
- Route ESC wires through arms

### 2. ESC Installation
- Mount 4-in-1 ESC or individual ESCs
- Connect ESC signal wires to H7-Digital via JST-SH connector

### 3. flight controller installation
- Install rubber dampeners on H7-Digital mounting holes
- Mount H7-Digital on frame (arrow pointing forward)
- Secure with screws through dampers

### 4. Peripheral Connections
Using JST-SH connectors, attach:
- **RC Receiver** → RC connector
- **GPS/Compass module** → GPS connector
- **Digital HD VTX** → VTX connector
- **Companion Computer** (optional) → COMPUTER connector
- **Servos** (optional) → SRVO connector

### 5. Power Connections
- Connect main battery power to ESC or PDB
- Verify voltage regulator outputs (5V, 10V)
- Double-check polarity on all connections

### 6. Cable Management
- Route cables away from propellers
- Use zip ties or velcro straps
- Leave slack for vibration (soft mount)
- Protect USB port

---

## Common Build Mistakes to Avoid

### Power Issues
- ❌ **Wrong polarity**: Always verify positive/negative before connecting
- ❌ **Undervoltage**: Ensure you use at least a 3S (11.1V) battery
- ❌ **Overloading BECs**: Don't exceed 2A continuous on either the 5V or 10V rails (2A/2.5A continuous/burst for each)

### Mechanical Issues
- ❌ **Hard mounting on noisy frame**: Use soft mounts for vibration isolation
- ❌ **Wrong orientation**: Arrow must point forward
- ❌ **Loose screws**: Use threadlocker on frame screws
- ❌ **Cables near props**: Always route away from spinning parts

### Connector Issues
- ❌ **Forcing connectors**: JST-SH connectors are delicate - align carefully
- ❌ **Reversed connectors**: Match pin labels to connector pins **(Tx → Rx, Rx → Tx)**
- ❌ **Damaged pins**: Inspect connectors before insertion

---

## Testing Before Flight

After completing your build:

### Bench Test Checklist

- [ ] **Visual inspection**: All connections secure, no exposed wires
- [ ] **Power test**: Connect battery (props off!), verify power on and successful ESC calibration (**Recommended**: use a [smoke stopper](https://www.getfpv.com/vifly-shortsaver-2-smart-smoke-stopper-xt60-xt30.html))
- [ ] **Voltage test**: Measure 5V and 10V outputs with multimeter
- [ ] **USB connectivity**: Connect to computer, verify recognition
- [ ] **Firmware install**: Load appropriate firmware (see next section)
- [ ] **Motor test**: With props off, test motor direction/rotation
- [ ] **Receiver test**: Verify RC input in configurator
- [ ] **Sensor test**: Check IMU, baro, GPS (if installed)
- [ ] **Configuration**: Set up modes, calibrate, configure failsafes

{: .warning }
> **Never test motors with propellers installed on a bench!** Propellers should only be installed in a safe outdoor area immediately before flight.

---

## Additional Resources

### Video Tutorials & Guides
- **[Joshua Bardwell's Channel](https://www.youtube.com/@JoshuaBardwell)**: Comprehensive FPV tutorials

### FPV Communities
- **[Oscar Liang](https://oscarliang.com/)**: Written guides and reviews
- **[ArduPilot Forum](https://discuss.ardupilot.org/)**: ArduPilot-specific help
- **[Betaflight Discord](https://discord.gg/betaflight)**: Real-time Betaflight support

### Connector Standards
- **[BetaFlight Connector Standard](https://www.betaflight.com/docs/development/manufacturer/connector-standard)**: Official specification

---

## Next Steps

Once your physical build is complete, you're ready to choose and install firmware:

[Choose Your Firmware →]({{ site.baseurl }}/docs/getting-started/choosing-firmware){: .btn .btn-primary }

Or review hardware specifications:

[Hardware Documentation →]({{ site.baseurl }}/docs/hardware/){: .btn }

---

## Need Help?

Having trouble with your build?

- **Hardware questions**: [support@aerocogito.com](mailto:support@aerocogito.com)
- **General build help**: Check the FPV communities listed above
- **Wiring questions**: See [Hardware Pinout]({{ site.baseurl }}/docs/hardware/pinout)