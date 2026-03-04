# Ember Mk.II — Flexible LED Watch

A cyberpunk-inspired wristwatch built on a custom flexible PCB, wrapping high-density alphanumeric LED displays around the wrist in an infinite loop. Displays animated effects and real-time clock output.

---

## PCB Layout

![PCB Layout](Assets/Screenshot%202026-03-02%20232211.png)

| Top Layer | Bottom Layer | All Layers |
|---|---|---|
| ![Top](Assets/Screenshot%202026-03-02%20235317.png) | ![Bottom](Assets/Screenshot%202026-03-02%20235324.png) | ![Layers](Assets/Screenshot%202026-03-02%20235329.png) |

---

## Schematic

![Schematic](Assets/Screenshot%202026-03-02.png)

---

## Overview

The STM32U083KCU6 microcontroller powers the system — chosen for its ultra-low power consumption, built-in peripherals, and minimal external component requirements. The flexible PCB integrates the microcontroller, charging circuitry, power regulation, display drivers, and user input into one continuous wearable design.

---

## How It Works
The basic concept is to daisy-chain a strip of alphanumeric LED displays (HCMS-3901 or similar Broadcom displays) and wrap it around the wrist using a flexible PCB. Upon boot, the watch displays a Matrix-style waterfall animation before transitioning to a clock display.Upon boot, the watch displays a Matrix-style waterfall animation before transitioning to a clock display.

Displays are driven in series, so the further from the microcontroller, the more sensitive they are to power fluctuations. During refresh, all pixels briefly turn off, causing a power spike. Small bypass capacitors near each display's logic supply are needed to absorb spikes and prevent resets.

A single-cell LiPo battery of 200–500mAh powers the watch. A charging IC handles USB charging, and an LDO or buck regulator provides stable 3.3V logic. At full brightness, the displays draw about 1A, so battery life is limited. The watch is off by default and activates only when a button is pressed to save power.

The flexible PCB has stiffeners under components to prevent flex damage. It attaches to the wrist with pin headers; USB-C was considered but risks tearing the board. Programming and charging use the same cable. The STM32U083 supports USB natively, so no extra programmer is needed if the bootloader is set up. An SWD debugger like ST-Link can also be used.

---

## Project Scope

- Custom schematic design
- Flexible PCB layout with stiffener layers
- Embedded firmware development
- Animated display effects (Matrix-style boot sequence)
- Real-time clock implementation
- USB programming and charging support

---

## Firmware & Dependencies

Display control is based on a modified version of:
> https://github.com/Andy4495/HCMS39xx

To compile:
- Install **STM32Duino** (Arduino core for STM32)
- Target: STM32U08 series

---

> ⚠️ This is an experimental hardware project intended for research and development purposes only.
