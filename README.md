# Ember Mk.II — Flexible LED Watch

A cyberpunk-inspired wristwatch built on a custom flexible PCB, wrapping high-density alphanumeric LED displays around the wrist in an infinite loop. Displays animated effects and real-time clock output.

---

## PCB Layout

![PCB Layout](assets/Screenshot_2026-03-02_232211.png)

| Top Layer | Bottom Layer | All Layers |
|---|---|---|
| ![Top](assets/Screenshot_2026-03-02_235317.png) | ![Bottom](assets/Screenshot_2026-03-02_235324.png) | ![Layers](assets/Screenshot_2026-03-02_235329.png) |

---

## Schematic

![Schematic](assets/Screenshot_2026-03-02.png)

---

## Overview

The STM32U083KCU6 microcontroller powers the system — chosen for its ultra-low power consumption, built-in peripherals, and minimal external component requirements. The flexible PCB integrates the microcontroller, charging circuitry, power regulation, display drivers, and user input into one continuous wearable design.

---

## Project Scope

- Custom schematic design
- Flexible PCB layout with stiffener layers
- Embedded firmware development
- Animated display effects (Matrix-style boot sequence)
- Real-time clock implementation
- USB programming and charging support

---

## Hardware

### Key Design Considerations

Power management is critical in this design. During display refresh cycles, voltage drops and transient spikes can cause logic resets — particularly on displays furthest from the power supply. Adequate bypass capacitance and careful layout are required to mitigate this.

Mechanical stiffener layers are applied to component mounting areas to prevent flex fatigue and component delamination during normal wear.

---

## Firmware & Dependencies

Display control is based on a modified version of:
> https://github.com/Andy4495/HCMS39xx

To compile:
- Install **STM32Duino** (Arduino core for STM32)
- Target: STM32U08 series

---

> ⚠️ This is an experimental hardware project intended for research and development purposes only.
