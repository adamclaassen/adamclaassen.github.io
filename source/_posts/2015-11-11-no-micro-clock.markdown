---
layout: post
title: "No-Micro Clock"
date: 2015-11-11 11:26:55 -0600
comments: true
categories:
---
## A Microcontroller-less LED Clock

Lately, I've been getting a bit tired of programming. It's fiddly, and I can't
see what's inside. I decided on a project that requires no coding, a clock based
entirely on basic logic blocks. Conveniently, I also need a clock in my dorm room.

### The Display
The clock's display is built out of 1206 SMD LEDs arranged into 4 7-segment displays.
Each display segment has 4 LEDs, connected in parallel.
The display is controlled by 4 SN74LS74 Binary 7-Segment display drivers, which can sink 24 mA of current per channel.
A 105 Ohm resistor is used to limit the current through the LED array to 20 mA (4 LEDs x 5 mA max current each). The display drivers also feature zero suppression and Lamp Test modes. Zero suppression mode is selectable via a jumper on the 1st clock digit, and Lamp Test is selectable via another jumper on all digits.

### Display Control
The current time is kept in two CD4520B dual binary counters, with AND gates (In a 74LS08 Quad-AND IC) used to reset and increment the counters when they reach the appropriate state.

### Timekeeping
<img src="/assets/pictures/no-micro-timebase-sch.jpg" alt="Time Base Screenshot"/>

The timebase schematic section from KiCAD

Many projects use a 555 for timing, but those drift with temperature, and my dorm room is not always the same temperature, especially as we plunge into a Kansas winter. Although the temperature is not constant in the long term, it is fairly stable short-term, making an OCXO. I used Roman Black's [XOven](http://www.romanblack.com/xoven.htm) design to create a cheap OCXO suitable for relatively small temperature ranges. The XOven is basically just some heater resistors, a thermistor, and a epoxied to an Xtal, with a trimpot for adjusting the heater. For this project, I'm using a 32.768 kHz quartz crystal oscillator, which can be divided to a 1 Hz signal. I based my frequency divider circuit on a 1 Hz Time Base project from [Hacker's Bench](http://www.hackersbench.com/Projects/1Hz/). The 2 Hz signal from the CD4060 14 flip-flop IC is fed into an 8-bit binary counter, which is reset after 120 counts by another 74LS08 Quad-AND IC.

### Power
Currently, the project's power supply is still in progress. The current thinking is either to use a 5v 2.5-3 A wallwart, or a 12v 1.5 A wallwart and a 12v to 5v high current buck converter.

### Design and construction
Currently, the clock is still in the schematic stage. I intend to go to board design and construction in plenty of time for K-State Engineering Open House. The clock is being designed in KiCAD (Git commit `2cfa79e`). The current project files are hosted on [GitHub](https://www.github.com/adamclaassen/no-micro-led-clock.git). All of the parts should be embedded in the design, but if they're not, the ones not in KiCAD's standard libraries can be found at my [parts repository](https://www.github.com/adamclaassen/adamclaassen-kicad-parts.git).
