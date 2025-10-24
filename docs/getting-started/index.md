---
layout: default
title: Getting Started
nav_order: 2
has_children: true
permalink: /docs/getting-started/
---

# Getting Started with H7-Digital
{: .no_toc }

This guide will walk you through everything you need to know to get your flight controller up and running safely.

---

## What's in This Section

This section covers the essential first steps with your H7-Digital:

### [Unboxing]({{ site.baseurl }}/docs/getting-started/unboxing)
What's included in the box, initial hardware inspection, and what additional items you'll need to get started.

### [Building Your FPV Drone]({{ site.baseurl }}/docs/getting-started/building)
Build your FPV drone with the H7-Digital flight controller.

### [Choosing Firmware]({{ site.baseurl }}/docs/getting-started/choosing-firmware)
Understand the differences between ArduPilot and Betaflight to choose the right firmware for your application.

### [First Flight Checklist]({{ site.baseurl }}/docs/getting-started/first-flight)
Pre-flight safety checks and procedures to ensure your first flight goes smoothly.

---

## Before You Begin

### Required Items

To get started with your H7-Digital, you'll need:

- **H7-Digital flight controller** (what you just unboxed)
- **Frame/airframe** appropriate for your application
- **ESCs** compatible with your chosen firmware (DShot recommended)
- **Motors** and propellers matched to your frame
- **Battery** (3S-6S LiPo)
- **Radio transmitter and receiver** (ELRS, CRSF, SBUS, or PWM)
- **Digital VTX** (OpenIPC, DJI, Walksnail, or HDZero)
- **USB-C cable** for connecting to computer
- **Computer** running Windows, macOS, or Linux

### Optional but Recommended

- **GPS module** (required for ArduPilot autonomous flight)
- **Companion computer** (Raspberry Pi, Jetson, etc.)
- **MicroSD card** (for blackbox logging or firmware updates)
- **External compass** (if using GPS for autonomous flight)

---

## Quick Start Overview

Here's the high-level process to get flying:

1. **Unbox and inspect** your hardware
2. **Choose your firmware** (ArduPilot or Betaflight)
3. **Mount the flight controller** on your frame
4. **Connect peripherals** (ESCs, receiver, GPS, etc.)
5. **Install firmware** using appropriate tools
6. **Configure and calibrate** following firmware-specific guides
7. **Perform pre-flight checks** using the checklist
8. **Test flight** in safe environment

---

## Safety First

{: .warning }
> **Important Safety Information**
> 
> - Always remove propellers when testing on a bench
> - Never arm your aircraft with propellers installed indoors
> - Follow all local regulations and airspace restrictions
> - Complete all calibration procedures before first flight
> - Test in open area away from people and property

---

## Firmware Decision

Not sure which firmware to use? Here's a quick comparison:

| Feature | ArduPilot | Betaflight |
|---------|-----------|------------|
| **Best For** | Autonomous flight, waypoints | Manual FPV flight, racing |
| **GPS Required** | Yes (for auto modes) | No |
| **Companion Computer** | Excellent support (MAVLink) | Limited |
| **Tuning Complexity** | Moderate | Simple to advanced |
| **NDAA Compliance** | Yes (with secure firmware) | Standard builds |
| **Mission Planning** | Full support | Not applicable |
| **Acrobatic Flight** | Basic | Excellent |

See the [Choosing Firmware]({{ site.baseurl }}/docs/getting-started/choosing-firmware) page for a detailed comparison of ArduPilot vs Betaflight capabilities, use cases, and installation instructions.

---

## Support Resources

If you run into any issues during setup:

- **H7-Digital Hardware**: [support@aerocogito.com](mailto:support@aerocogito.com)
- **ArduPilot Firmware**: [ArduPilot Forum](https://discuss.ardupilot.org/) | [Discord](https://ardupilot.org/discord)
- **Betaflight Firmware**: [Betaflight Discord](https://discord.gg/betaflight)

---

## Next Steps

Ready to begin? Start with unboxing your flight controller:

[Unbox Your H7-Digital →]({{ site.baseurl }}/docs/getting-started/unboxing){: .btn .btn-primary }