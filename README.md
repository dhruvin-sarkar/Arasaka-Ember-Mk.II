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
The basic concept is to daisy-chain a strip of alphanumeric LED displays (HCMS-2901 or similar Broadcom displays) and wrap it around the wrist using a flexible PCB. Upon boot, the watch displays a Matrix-style waterfall animation before transitioning to a clock display.Upon boot, the watch displays a Matrix-style waterfall animation before transitioning to a clock display.


A few buttons on the board allow you to navigate menus and adjust brightness.A few buttons on the board allow you to navigate menus and adjust brightness. The displays are driven in series, with data passing from one to the next down the chain.The displays are driven in series, with data passing from one to the next down the chain. This means that the further a display is from the microcontroller, the more susceptible it is to power fluctuations.This means that the further a display is from the microcontroller, the more susceptible it is to power fluctuations. During display refresh, all pixels briefly turn off and then back on, resulting in a spike on the power rail.During display refresh, all pixels briefly turn off and then back on, resulting in a spike on the power rail. Without proper decoupling capacitors on the logic lines, the furthest displays may reset.Without proper decoupling capacitors on the logic lines, the furthest displays may reset. Small bypass capacitors near each display's logic supply pin are required to absorb these spikes and maintain stability.Small bypass capacitors near each display's logic supply pin are required to absorb these spikes and maintain stability.


A small single-cell LiPo battery ranging in size from 200 to 500mAh should suffice, salvaged or not.A small single-cell LiPo battery ranging in size from 200 to 500mAh should suffice, salvaged or not. The battery is connected to a charging IC that handles USB charging, while a small LDO or buck regulator reduces the voltage to a stable 3.3V for the logic.The battery is connected to a charging IC that handles USB charging, while a small LDO or buck regulator reduces the voltage to a stable 3.3V for the logic. At full brightness, the displays draw approximately 1A total, so battery life will be limited at maximum settings — the watch is designed to be turned off by default and only activated when a button is pressed to conserve power.At full brightness, the displays draw approximately 1A total, so battery life will be limited at maximum settings — the watch is designed to be turned off by default and only activated when a button is pressed to conserve power. The flexible PCB has stiffener layers in the areas where components are soldered, which prevents the board from flexing and snapping components off.The flexible PCB has stiffener layers in the areas where components are soldered, which prevents the board from flexing and snapping components off. The board attaches to the wrist with a pin header connector on each end; a USB-C connector was considered, but it would have been too tight and risked tearing the PCB when removing the watch.The board attaches to the wrist with a pin header connector on each end; a USB-C connector was considered, but it would have been too tight and risked tearing the PCB when removing the watch. Programming and charging occur via the same cable — a standard USB cable with a pin header on one end.Programming and charging occur via the same cable — a standard USB cable with a pin header on one end. The STM32U083 supports USB natively, so no additional programmer hardware is required as long as the bootloader is properly configured.The STM32U083 supports USB natively, so no additional programmer hardware is required as long as the bootloader is properly configured. An SWD debugger, such as an ST-Link, can also be used for programming.An SWD debugger, such as an ST-Link, can also be used for programming. 

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
