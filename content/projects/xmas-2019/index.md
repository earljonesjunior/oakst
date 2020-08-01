---
title: "xMas 2019"
date: 2019-12-25
slug: "xmas-2019"
draft: false
tags: ["hardware", "firmware", "mcu", "pcb"]
project: "xmas-2019"

inProgress: false
started: 2019-11-01
completed: 2019-12-25

description: "A continuation of a new tradition."
---

Last year I built some [Christmas tree ornaments](/projects/xmas-2018) to give away as presents to my friends and family. This year I built some snowflakes.

Similar to last year, I interfaced an 8-bit Microchip MCU with a TI 16-channel LED driver. Last year's ornaments
drove 5 RGB LEDs plus one solid LED (16 total channels) while this year's ornament drove 16 solid LEDs. New to
this year was a little push button that could be pressed to switch between 5 different lighting patterns.

Last year's tree ornament was powered by a AAA battery. I wasn't very satisfied with the heft and thickness of 
the final assembly so I instead went with a 3V CR2450 coin battery. This lowered the total current capacity
by about 400mAh so I had to make some software changes to optimize battery life. 

Additionally I removed the 
bulky hard power switch from last year's design and instead implement a low-power mode that turns off 
the LED driver and turns off unnecessary hardware peripherals like the MSSP I2C module. This *sort of* worked - 
there was some strange bug somewhere (I'm not sure hardware or software) where the LED's would *sometimes* remain on even with the driver turned off. 
I didn't have enough time to squash the bug before Christmas!

I also threw in some test points for easier I2C debugging. That's one of those things that you learn from having
previously struggled with the issue.

The circuit is shown below:

![schematic][schematic]

A screen shot of the PCB is shown below:

![pcb][pcb]

And the final assembly here:

![final][final]

The source code and gerbers can be found in my [Github repository](https://github.com/bruthaearl/chrimbus2019).


[pcb]: /projects/xmas-2019/pcb.png "PCB"
[schematic]: /projects/xmas-2019/schematic.png "Schematic"
[final]: /projects/xmas-2019/final.jpg "Final Assembly"