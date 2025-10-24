---
layout: default
title: Hardware
nav_order: 3
has_children: true
permalink: /docs/hardware
---

# Hardware Documentation
{: .no_toc }

Complete technical reference for the H7-Digital flight controller hardware, including specifications, pinouts, connectors, and electrical characteristics.
{: .fs-6 .fw-300 }

---

## What's in This Section

This section provides detailed technical documentation for the H7-Digital hardware:

### [Specifications]({{ site.baseurl }}/docs/hardware/specifications)
Complete technical specifications including processor, sensors, power system, communication interfaces, and physical dimensions.

### [Pinout & Connectors]({{ site.baseurl }}/docs/hardware/pinout)
Unified pin assignments and connector reference with detailed UART mapping, motor outputs, servo outputs, and all JST-SH connector pinouts following the BetaFlight standard.

### [Power System]({{ site.baseurl }}/docs/hardware/power)
Power architecture, voltage regulators, battery monitoring, and VTX power control documentation.

### [Sensors]({{ site.baseurl }}/docs/hardware/sensors)
Details on the IMU, barometer, and external sensor support including GPS and compass modules.

---

## Quick Reference

For detailed pin assignments and connector information, see the [Pinout & Connectors]({{ site.baseurl }}/docs/hardware/pinout) page.

---

## When to Reference This Section

Use the hardware documentation when:

- **Wiring your build** - Connector pinouts and power requirements
- **Troubleshooting connections** - Verify correct pin assignments
- **Planning peripherals** - Check available UARTs, I2C, and power
- **Mounting considerations** - Physical dimensions and orientation
- **Understanding capabilities** - Processor specs and sensor details
- **Selecting components** - Compatible GPS, compass, VTX modules

---

## Hardware Overview

The H7-Digital is a high-performance flight controller built around the STM32H743 microcontroller, featuring professional-grade sensors, dual voltage regulators, and comprehensive I/O. Designed exclusively for digital HD video systems and companion computer integration.

![H7-Digital Flight Controller - Top View]({{ site.baseurl }}/assets/images/pinout/H7-Digital_top.png)

**Key Highlights:**
- **Processing**: 480MHz ARM Cortex-M7 with 1MB RAM, 2MB Flash
- **Sensors**: ICM-42688-P IMU (isolated power) + DPS368 barometer
- **Connectivity**: 6 UARTs (all with DMA), 2 I2C, USB-C, MicroSD
- **Power**: Dual BECs (5V/2A + 10V/2A software-controlled)
- **I/O**: 10 PWM outputs with bidirectional DShot support

For complete technical specifications, see [Specifications]({{ site.baseurl }}/docs/hardware/specifications).

For detailed connector locations and pinout, see [Pinout & Connectors]({{ site.baseurl }}/docs/hardware/pinout).

---

## Design Philosophy

### Digital HD Only

The H7-Digital is designed exclusively for **modern digital HD video systems**. There is no analog video hardware on this board.

**Supported Systems**:
- OpenIPC
- DJI
- Walksnail/Avatar
- HDZero

This focus allows for:
- Modern, future-proof design
- Cleaner power delivery
- Reduced electromagnetic interference
- Reduced complexity and increased reliability
- Increased supply chain resilience (analog OSD implementation relies on outdated/EOL components)

### Companion Computer First

Unlike many flight controllers where companion computer support is an afterthought, the H7-Digital prioritizes companion computer integration:

- **Dedicated UART5** with DMA for high-bandwidth MAVLink
- **Optimized for Raspberry Pi and Jetson** platforms
- **Tested extensively** with common companion setups

### NDAA Compliance Ready

Hardware designed from the start for compliance:
- **Auditable supply chain**
- **US-based manufacturing**
- **No covered entities** in design or production
- **Secure firmware support** available

---

## Compliance

The H7-Digital hardware is designed and manufactured to meet:

- **NDAA Section 848** - Operating Software compliance ready
- **DIU Blue List Framework** - compliant (pending)
- **Made in USA** - Designed and manufactured domestically

---

## Hardware Revision History

### Current Production: **REV-A**

- Initial production release
- All features verified and tested
- Full documentation available

{: .note }
> Hardware revision information is printed on the bottom of the PCB. Contact support@aerocogito.com if you need assistance identifying your hardware version.

---

## Related Documentation

- [Getting Started]({{ site.baseurl }}/docs/getting-started/) - Unboxing and first setup
- [ArduPilot Documentation](https://ardupilot.org/copter/docs/initial-setup.html) - Official firmware setup guide
- [Betaflight Documentation](https://www.betaflight.com/docs/wiki/getting-started) - Official firmware setup guide

---

## Support

For hardware-related questions:

- **Email**: [support@aerocogito.com](mailto:support@aerocogito.com)

---

## Next Steps

Start with the specifications page to understand the complete technical capabilities:

[View Specifications →]({{ site.baseurl }}/docs/hardware/specifications){: .btn .btn-primary }
