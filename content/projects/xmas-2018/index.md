---
title: "xMas 2018"
date: 2018-12-25
slug: "xmas-2018"
draft: false
tags: ["hardware", "firmware", "mcu", "pcb"]
project: "xmas-2018"

inProgress: false
started: 2018-07-01
completed: 2018-12-25

description: "I designed and built some PCB Christmas tree ornaments."
---

I designed and built some PCB Christmas tree ornaments for my friends and family. Nothing says "Christmas" like blinky LEDs. 

These boards interface an 8-bit Microchip microcontroller with a TI 16-channel LED driver to control several RGB LEDs. Random LED colors are generated by the MCU and sent to the driver over an I2C bus. The LED driver sinks the LED current using its open-drain PWM-controlled outputs. Specific colors are selected by setting the PWM duty cycle of the R, G,and B channels to the desired color's R, G, and B hex codes. [The source code of the firmware can be found on my Git Hub repository](https://github.com/bruthaearl/chrimbus/blob/master/src/chrimbus.X/main.c). 

The circuit is shown below:

![schematic][schematic]

A screen shot of the PCB is shown below:

![pcb][pcb]

And the final product here:

![trees][final]


[pcb]: /projects/xmas-2018/pcb.png "PCB"
[schematic]: /projects/xmas-2018/schematic.png "Schematic"
[final]: /projects/xmas-2018/trees.jpg "Final Product"