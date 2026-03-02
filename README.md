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

The core idea is to have a strip of alphanumeric LED displays (HCMS-2901 or similar Broadcom displays) daisy-chained together and wrapped around the wrist on a flexible PCB. On boot, the watch plays a Matrix-style waterfall animation before transitioning into a clock display. A few buttons on the board let you navigate menus and control brightness.

The displays are driven in series — data passes from one display to the next down the chain. This means the further a display is from the microcontroller, the more sensitive it is to power fluctuations. During display refresh, all pixels briefly cut out and then blast back on, which causes a spike on the power rail. Without proper decoupling capacitors on the logic lines, this can cause the furthest displays to reset. Small bypass capacitors placed near each display's logic supply pin are needed to absorb these spikes and keep everything stable.

Power comes from a small single-cell LiPo battery — something in the 200–500mAh range should work, salvaged or otherwise. The battery connects to a charging IC which handles USB charging, and a small LDO or buck regulator steps the voltage down to a stable 3.3V for the logic. At full brightness the displays draw around 1A total, so battery life will be short at max settings — the watch is designed to stay off by default and only activate on button press to conserve power.

The flexible PCB is manufactured with stiffener layers in the regions where components are soldered, preventing the board from flexing at those points and snapping components off. The board clasps around the wrist using a pin header connector on each end — a USB-C connector was considered but would have been too tight and risked tearing the PCB when removing the watch.

Programming and charging both happen over the same cable — a standard USB cable with a pin header on one end. The STM32U083 supports USB natively, so no additional programmer hardware is needed as long as the bootloader is set up correctly. Alternatively, an SWD debugger like an ST-Link can be used for programming if needed.

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
