---
layout: default
title: Home
nav_order: 1
description: "AeroCogito H7-Digital - NDAA-compliant STM32H743 flight controller for ArduPilot and Betaflight"
permalink: /
---

<div class="hero-landing" markdown="1">

# AeroCogito H7-Digital Flight Controller

High-performance, DCMA Blue UAS / NDAA-compliant flight controller designed for professional UAS applications with companion computer integration.

</div>

<div class="button-group-center" markdown="1">
[Get Started]({{ site.baseurl }}/docs/getting-started/){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Buy Now](https://aerocogito.com/store/p/h7-digital){: .btn .btn-orange .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Secure Firmware](https://github.com/AeroCogito/h7-digital-firmware){: .btn .btn-purple .fs-5 .mb-4 .mb-md-0 }
</div>

<div style="text-align: center; margin: 2rem 0;">
  <img src="{{ site.baseurl }}/assets/images/pinout/H7-Digital_top.png" alt="H7-Digital Flight Controller - Top View" style="max-width: 100%; height: auto; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.1);">
</div>

---

## Wiring Your H7-Digital

<div id="wiring-diagram-thumbnail" style="cursor: pointer;" onclick="openImageModal('{{ site.baseurl }}/assets/images/pinout/H7-Digital_wiring.png')">
  <img src="{{ site.baseurl }}/assets/images/pinout/H7-Digital_wiring.png" alt="H7-Digital wiring diagram showing all connector locations and pin assignments for ESC, GPS, VTX, RC receiver, and other components" style="max-width: 100%; height: auto; border: 2px solid #e0e0e0; border-radius: 4px;">
  <p style="text-align: center; font-size: 0.875rem; color: #666; margin-top: 0.5rem; margin-bottom: 0;"><em>Click image to view full size</em></p>
  <p style="text-align: center; font-size: 0.9rem; color: #888; margin-top: 0.25rem;"><em>Labeled wiring diagram showing all connector locations and pin assignments</em></p>
</div>

<!-- Image Modal -->
<div id="imageModal" style="display: none; position: fixed; z-index: 9999; left: 0; top: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.9); opacity: 0; transition: opacity 0.3s ease;">
  <span onclick="closeImageModal()" style="position: absolute; top: 20px; right: 35px; color: #f1f1f1; font-size: 40px; font-weight: bold; cursor: pointer; transition: color 0.3s;">&times;</span>
  <img id="modalImage" style="margin: auto; display: block; max-width: 95%; max-height: 95%; position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%) scale(0.7); transition: transform 0.3s ease;">
</div>

<script>
function openImageModal(imageSrc) {
  const modal = document.getElementById('imageModal');
  const modalImg = document.getElementById('modalImage');

  modalImg.src = imageSrc;
  modal.style.display = 'block';

  // Trigger animation
  setTimeout(() => {
    modal.style.opacity = '1';
    modalImg.style.transform = 'translate(-50%, -50%) scale(1)';
  }, 10);

  // Add keyboard support
  document.addEventListener('keydown', handleEscKey);
}

function closeImageModal() {
  const modal = document.getElementById('imageModal');
  const modalImg = document.getElementById('modalImage');

  modal.style.opacity = '0';
  modalImg.style.transform = 'translate(-50%, -50%) scale(0.7)';

  setTimeout(() => {
    modal.style.display = 'none';
  }, 300);

  // Remove keyboard listener
  document.removeEventListener('keydown', handleEscKey);
}

function handleEscKey(event) {
  if (event.key === 'Escape') {
    closeImageModal();
  }
}

// Close modal when clicking outside the image
document.getElementById('imageModal')?.addEventListener('click', function(event) {
  if (event.target === this) {
    closeImageModal();
  }
});
</script>

**Hardware Documentation Quick Links:**

[Pinout & Connectors]({{ site.baseurl }}/docs/hardware/pinout){: .btn .btn-blue .fs-5 .mb-2 .mr-2 }
[Specifications]({{ site.baseurl }}/docs/hardware/specifications){: .btn .btn-blue .fs-5 .mb-2 .mr-2 }
[Power System]({{ site.baseurl }}/docs/hardware/power){: .btn .btn-blue .fs-5 .mb-2 .mr-2 }
[Sensors]({{ site.baseurl }}/docs/hardware/sensors){: .btn .btn-blue .fs-5 .mb-2 }

- **[Pinout & Connectors]({{ site.baseurl }}/docs/hardware/pinout)** - Complete pin assignments, UART mapping, ESC telemetry methods
- **[Specifications]({{ site.baseurl }}/docs/hardware/specifications)** - Technical specs, LED indicators, dimensions
- **[Power System]({{ site.baseurl }}/docs/hardware/power)** - Power architecture, BEC specifications, battery monitoring
- **[Sensors]({{ site.baseurl }}/docs/hardware/sensors)** - IMU, barometer, GPS configuration

---

{: .tip }
> 📚 **Looking for something specific?** Use the search box above (press `/` to focus) or browse the navigation sidebar.

---

## New to FPV Drones?

**Start with our guided Getting Started journey:**

[Get Started →]({{ site.baseurl }}/docs/getting-started/){: .btn .btn-green .fs-6 .mb-3 }

- **Unboxing** - What's in the box and initial inspection
- **Building** - Wiring your FPV drone with H7-specific guidance
- **Choosing Firmware** - ArduPilot vs Betaflight comparison and installation
- **First Flight** - Pre-flight safety checklist

Perfect for first-time builders with detailed explanations, safety guidelines, and complete setup checklists.

---

## What Makes H7-Digital Different?

### 🇺🇸 Professional UAS Platform with NDAA Compliance
Designed from the ground up for professional operations with full NDAA Section 848 compliance. Features secure signed firmware, auditable supply chain, and US-based manufacturing. Perfect for government, defense, and commercial operators.

### ⚡ High-Performance Hardware
- **MCU**: STM32H743 @ 480MHz with 1MB RAM, 2MB Flash
- **IMU**: ICM-42688-P 6-axis with isolated power rail for noise-free operation
- **Barometer**: DPS368 high-precision altitude sensor
- **10 PWM Outputs**: DShot300/600 with bidirectional support
- **Storage**: MicroSD card for blackbox logging and firmware updates

### 🚁 Dual Open-Source Firmware
Run **ArduPilot** for autonomous missions and waypoint navigation, or **Betaflight** for high-performance FPV flight. Both firmware options fully supported with optimized configurations.

### 🤖 Companion Computer Optimized
Dedicated high-speed UART with DMA support, optimized for Raspberry Pi and Nvidia Jetson. Perfect for AI/ML applications, computer vision, and autonomous operations.

### 📡 Digital HD Video Only
Designed exclusively for modern digital HD video systems with software-controlled 10V power. See [Hardware Overview]({{ site.baseurl }}/docs/hardware/#digital-hd-only) for supported systems.

---

## Support & Resources

**Hardware Support**: [support@aerocogito.com](mailto:support@aerocogito.com)

**ArduPilot Community**: [Forum](https://discuss.ardupilot.org/) • [Discord](https://ardupilot.org/discord)

**Betaflight Community**: [Discord](https://discord.gg/betaflight)

**Purchase**: [aerocogito.com/store/p/h7-digital](https://aerocogito.com/store/p/h7-digital)

**Firmware Repository**: [github.com/AeroCogito/h7-digital-firmware](https://github.com/AeroCogito/h7-digital-firmware)

**Hardware Repository**: [github.com/AeroCogito/h7-digital-docs](https://github.com/AeroCogito/h7-digital-docs)

---

**Made in the USA** 🇺🇸 | **NDAA Compliant** | **Open Source Firmware**
