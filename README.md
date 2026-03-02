Flexible LED Watch for Cyberpunk


This project is a cyberpunk-inspired wristwatch idea that uses high-density alphanumeric LED displays mounted on a custom flexible PCB. The goal is to make a small, wearable "infinite loop" display that can show animated effects and tell the time in real time.


The STM32U083KCU6 microcontroller was chosen for the system because it uses very little power, has built-in peripherals, and needs very few outside parts. The flexible PCB is meant to combine the microcontroller, charging circuitry, power regulation, display drivers, and user input into one continuous design.


The scope of the project


Design of custom schematics


Layout of flexible PCBs


Making firmware for embedded systems


Effects that make the display move


Setting up a real-time clock


Support for programming and charging via USB


Things to think about when designing


Based on what we learned from looking at other high-current display systems, careful power management and decoupling will probably be very important to make sure that the flexible PCB works properly. During display refresh cycles, voltage drops and transient spikes can happen. To avoid these problems, you need to have enough bypass capacitance and optimize the layout.


In areas where components are mounted, mechanical reinforcement (stiffener layers) will also be needed to keep the PCB from breaking when it bends.


Firmware and Dependencies


The implementation of the display control is based on a changed version of the following library:


https://github.com/Andy4495/HCMS39xx


This repository has the .ino file and the modified library files.


You need to install STM32Duino (Arduino core for STM32) to compile the firmware.
This project is for the STM32U08 series of microcontrollers.


Warning


This is an experimental hardware project meant for research and development.
