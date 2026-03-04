# Ember Mk.II — Flexible LED Watch

A cyberpunk-inspired wristwatch built on a custom flexible PCB, wrapping high-density alphanumeric LED displays around the wrist in an infinite loop. Displays animated effects and real-time clock output.

---

## What is it?

Ember Mk.II is a handmade wristwatch built around a custom flexible PCB that wraps 14 Broadcom HCMS-2901 alphanumeric LED displays around the wrist. On wake, it plays a Matrix-style waterfall animation before transitioning to a live clock display. Everything — the microcontroller, charging circuit, power regulation, displays, and clasp — lives on one continuous bendable strip.

## How to use it

Press the center button to wake the display and trigger the boot animation. Use the up and down buttons to cycle through menus: time display, mini time, brightness, current, timezone offset, and wave mode. The watch charges and programs over the same 4-pin header used as the wrist clasp — no separate programmer needed. To set the time, send a Unix epoch timestamp over serial.

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

## Bill of Materials

| Ref | Component | Value / Part | Package | Qty | Datasheet / Source |
|-----|-----------|-------------|---------|-----|-------------------|
| U5–U18 | Alphanumeric LED Display | **HCMS-3901** | Custom SMD (bent TH pins) | 14 | [Broadcom Datasheet](https://docs.broadcom.com/doc/AV02-0683EN) · [Mouser](https://www.mouser.com/ProductDetail/Broadcom-Avago/HCMS-3901) |
| U1 | Microcontroller | **STM32U083KCUx** | QFN-32 | 1 | [ST Datasheet](https://www.st.com/resource/en/datasheet/stm32u083kc.pdf) · [Mouser](https://www.mouser.com/ProductDetail/STMicroelectronics/STM32U083KCU6) |
| U2 | LDO Regulator | **LP2980** (3.3V fixed) | SOT-23-5 | 1 | [TI Datasheet](https://www.ti.com/lit/ds/symlink/lp2980.pdf) · [Mouser](https://www.mouser.com/ProductDetail/Texas-Instruments/LP2980IM5X-3.3) |
| U3 | LiPo Charger | **STC4054** | SOT-23-5 | 1 | [Datasheet](https://datasheet.lcsc.com/lcsc/2210211830_Shenzhen-Fuman-Elec-STC4054_C89852.pdf) · [LCSC](https://www.lcsc.com/product-detail/Battery-Management_Shenzhen-Fuman-Elec-STC4054_C89852.html) |
| J1 | Wrist clasp socket | 4-pin SMD socket | custom:SMD_2.54_1x04_LONG | 1 | Standard 2.54mm pin header |
| J2 | Wrist clasp pin | 4-pin SMD pin | custom:SMD_2.54_1x04_SHORT | 1 | Standard 2.54mm pin header |
| SW1 | Boot mode button | BOOT0 | PTS636 | 1 | [C&K PTS636 Datasheet](https://www.ckswitches.com/media/1471/pts636.pdf) · [Mouser](https://www.mouser.com/ProductDetail/CK/PTS636-SK25-SMTR-LFS) |
| SW2 | Up button | UP | PTS636 | 1 | [C&K PTS636 Datasheet](https://www.ckswitches.com/media/1471/pts636.pdf) |
| SW3 | Center button | CENTER | PTS636 | 1 | [C&K PTS636 Datasheet](https://www.ckswitches.com/media/1471/pts636.pdf) |
| SW4 | Down button | DOWN | PTS636 | 1 | [C&K PTS636 Datasheet](https://www.ckswitches.com/media/1471/pts636.pdf) |
| R1, R2 | Resistor | TBD | 0402 | 2 | Standard 0402 SMD resistor |
| TP1 | Test point | SWDIO | D1.5mm pad | 1 | SWD debug interface |
| TP2 | Test point | SWCLK | D1.5mm pad | 1 | SWD debug interface |
| TP3 | Test point | BATT | D1.5mm pad | 1 | Battery voltage monitor |
| TP4 | Test point | GND | D1.5mm pad | 1 | Ground reference |

> ⚠️ **Display availability note:** The HCMS-2901 is a discontinued Broadcom part. Stock availability varies — check Mouser, DigiKey, and secondary markets. Expect ~$25–35 per unit. The HCMS-3901 is a pin-compatible alternative.

---

## Overview

The STM32U083KCU6 microcontroller powers the system — chosen for its ultra-low power consumption, built-in peripherals, and minimal external component requirements. The flexible PCB integrates the microcontroller, charging circuitry, power regulation, display drivers, and user input into one continuous wearable design.

---

## How It Works

The basic concept is to daisy-chain a strip of alphanumeric LED displays (HCMS-2901 or similar Broadcom displays) and wrap it around the wrist using a flexible PCB. Upon boot, the watch displays a Matrix-style waterfall animation before transitioning to a clock display.

Displays are driven in series, so the further from the microcontroller, the more sensitive they are to power fluctuations. During refresh, all pixels briefly turn off, causing a power spike. Small bypass capacitors near each display's logic supply are needed to absorb spikes and prevent resets.

A single-cell LiPo battery of 200–500mAh powers the watch. A charging IC handles USB charging, and an LDO regulator provides stable 3.3V logic. At full brightness, the displays draw about 1A, so battery life is limited. The watch is off by default and activates only when a button is pressed to save power.

The flexible PCB has stiffeners under components to prevent flex damage. It attaches to the wrist with pin headers; USB-C was considered but risks tearing the board. Programming and charging use the same cable. The STM32U083 supports USB natively, so no extra programmer is needed if the bootloader is set up. An SWD debugger like ST-Link can also be used via TP1/TP2.

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

Display control is based on a modified version of the HCMS39xx Arduino library:
> https://github.com/Andy4495/HCMS39xx

Modifications include:
- `printDirectBufferOverlay()` — bitwise OR compositing for animation over UI
- `printDirectBufferXOR()` — XOR compositing for blinking effects
- `refreshDisplay()` — decoupled buffer writes from display refresh for frame timing

To compile:
- Install **STM32Duino** (Arduino core for STM32): [https://github.com/stm32duino/Arduino_Core_STM32](https://github.com/stm32duino/Arduino_Core_STM32)
- Install **STM32RTC** library: [https://github.com/stm32duino/STM32RTC](https://github.com/stm32duino/STM32RTC)
- Target board: **STM32U08 series** (Generic STM32U0)

---

## Resources

| Resource | Link |
|----------|------|
| STM32U083 Reference Manual | [ST.com](https://www.st.com/resource/en/reference_manual/rm0503-stm32u0-series-advanced-armbased-32bit-mcus-stmicroelectronics.pdf) |
| HCMS-2901 Application Brief | [Broadcom](https://docs.broadcom.com/doc/5988-7539EN) |
| STM32Duino Core | [GitHub](https://github.com/stm32duino/Arduino_Core_STM32) |
| STM32RTC Library | [GitHub](https://github.com/stm32duino/STM32RTC) |
| HCMS39xx Arduino Library (base) | [GitHub](https://github.com/Andy4495/HCMS39xx) |
| KiCad EDA | [kicad.org](https://www.kicad.org) |

---

> ⚠️ This is an experimental hardware project intended for research and development purposes only.
